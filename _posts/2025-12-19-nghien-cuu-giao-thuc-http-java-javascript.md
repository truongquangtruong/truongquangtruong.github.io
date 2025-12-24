---
title: "Khóa Học #05: Nghiên cứu Sâu Giao thức HTTP - Kiến trúc Truyền tải và Đồng bộ hóa Thực thể Java-JavaScript"
date: 2025-12-19
tags: ["java", "javascript", "http-protocol", "network-research", "fullstack", "deep-dive"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu giao thức truyền tải dữ liệu](https://npp.com.vn/wp-content/uploads/2024/06/dong-bo-hoa-du-lieu.jpg)
 
Chào các bạn! Sau khi đã hoàn thành nghiên cứu về DNS ở Bài 4, chúng ta đã biết cách các thực thể tìm thấy nhau. Tuy nhiên, trong môi trường Fullstack hiện đại, việc "thấy" nhau chỉ là bước khởi đầu. Bài nghiên cứu số 5 này sẽ tập trung hoàn toàn vào **HTTP (HyperText Transfer Protocol)** — "trái tim" của mọi giao tiếp Web.

Mục tiêu của bài nghiên cứu này là bóc tách lớp vỏ của các thư viện như Axios (JS) hay Spring RestTemplate (Java) để quan sát luồng dữ liệu thô chạy qua Socket. Chúng ta sẽ tìm hiểu cách hai ngôn ngữ khác biệt về bản chất này lại có thể hiểu ý nhau thông qua một tập hợp các quy tắc văn bản nghiêm ngặt.

---

### 1. Phân tích thực thể: Sự bất đối xứng trong giao tiếp HTTP

Trong mô hình nghiên cứu này, chúng ta định nghĩa sự tương tác giữa JavaScript và Java là một mối quan hệ **Client-Server truyền thống**, nhưng mang những đặc thù riêng về kiểu dữ liệu.

#### 1.1. Thực thể JavaScript: Vai trò "Người khởi xướng" (The Initiator)
JavaScript chạy trên trình duyệt thường bị giới hạn bởi các chính sách bảo mật (Sandbox). Do đó, cấu trúc HTTP Request mà nó tạo ra phải mang đầy đủ "chứng minh thư" để Java Server chấp nhận:
* **Verb (Phương thức)**: Nghiên cứu các hành động cụ thể (GET để lấy, POST để tạo, DELETE để xóa).
* **Header `Accept`**: JavaScript thông báo: "Tôi chỉ hiểu dữ liệu dạng JSON, làm ơn đừng gửi XML".
* **User-Agent**: Cung cấp thông tin về trình duyệt để Java có thể tối ưu hóa nội dung phản hồi.



#### 1.2. Thực thể Java: Vai trò "Người điều phối" (The Orchestrator)
Java Server đứng ở phía sau, phải xử lý đa luồng (Multi-threading) để tiếp nhận hàng ngàn yêu cầu HTTP cùng lúc. Nhiệm vụ khó nhất của Java là **Header Parsing** — đọc và phân loại các yêu cầu để điều hướng đến đúng hàm xử lý.

---

### 2. Nghiên cứu thực nghiệm: Xây dựng Bộ phân tích HTTP thô (Raw HTTP Analyzer)

Thay vì dùng các Server có sẵn như Tomcat, chúng ta sẽ tự viết một Socket Server trong Java để "soi" từng byte mà JavaScript gửi qua.

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;
import java.util.StringTokenizer;

/**
 * Advanced HTTP Protocol Research Tool
 * Phân tích chuyên sâu cấu trúc Header và Body từ JavaScript
 * Tác giả: Trương Quang Trưởng
 */
public class DeepHTTPAnalyzer {
    public static void main(String[] args) {
        final int PORT = 8080;

        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("====================================================");
            System.out.println("HỆ THỐNG NGHIÊN CỨU GIAO THỨC HTTP [TRƯỞNG] START...");
            System.out.println("Đang lắng nghe tín hiệu từ JavaScript tại cổng: " + PORT);
            System.out.println("====================================================");

            while (true) {
                try (Socket client = serverSocket.accept()) {
                    InputStream input = client.getInputStream();
                    BufferedReader reader = new BufferedReader(new InputStreamReader(input, StandardCharsets.UTF_8));

                    // 1. Phân tích Request Line chuyên sâu
                    String firstLine = reader.readLine();
                    if (firstLine == null) continue;
                    
                    StringTokenizer st = new StringTokenizer(firstLine);
                    String method = st.nextToken();
                    String uri = st.nextToken();
                    String version = st.nextToken();

                    System.out.println("\n[NODE ANALYZER] PHÁT HIỆN YÊU CẦU MỚI:");
                    System.out.println("   ├── Phương thức: " + method);
                    System.out.println("   ├── Đường dẫn: " + uri);
                    System.out.println("   └── Phiên bản: " + version);

                    // 2. Nghiên cứu toàn bộ tập hợp Headers
                    System.out.println("[HEADER RESEARCH]");
                    String header;
                    int contentLength = 0;
                    while (!(header = reader.readLine()).isEmpty()) {
                        System.out.println("   │ " + header);
                        // Đặc biệt lưu ý Content-Length để đọc Body
                        if (header.startsWith("Content-Length:")) {
                            contentLength = Integer.parseInt(header.substring(16).trim());
                        }
                    }

                    // 3. Nghiên cứu Body (Nếu có - thường là POST request từ JS)
                    if (contentLength > 0) {
                        char[] bodyChars = new char[contentLength];
                        reader.read(bodyChars, 0, contentLength);
                        String bodyData = new String(bodyChars);
                        System.out.println("   └── DỮ LIỆU BODY (JSON): " + bodyData);
                    }

                    // 4. Phản hồi chuẩn hóa nghiên cứu (HTTP Response)
                    OutputStream output = client.getOutputStream();
                    String responseContent = "{\"status\": \"success\", \"message\": \"Nghiên cứu hoàn tất!\"}";
                    
                    String httpResponse = "HTTP/1.1 200 OK\r\n" +
                                         "Content-Type: application/ 