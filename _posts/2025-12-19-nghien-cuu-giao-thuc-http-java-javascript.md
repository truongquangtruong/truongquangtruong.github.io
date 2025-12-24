---
title: "Khóa Học #05: Nghiên cứu Thượng tầng Giao thức HTTP - Kiến trúc Truyền tải và Đồng bộ hóa Thực thể Java-JavaScript"
date: 2025-12-19
tags: ["java", "javascript", "http-protocol", "network-research", "fullstack", "deep-dive", "low-level"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu giao thức truyền tải dữ liệu](https://npp.com.vn/wp-content/uploads/2024/06/dong-bo-hoa-du-lieu.jpg)

---

### 📍 Mục lục nội dung
1. [Phân tích thực thể: Cấu trúc văn bản của giao thức HTTP](#1-phân-tích-thực-thể-cấu-trúc-văn-bản-của-giao-thức-http)
2. [Cơ chế xử lý của Java (The Byte Processor)](#12-cơ-chế-xử-lý-của-java-the-byte-processor)
3. [Nghiên cứu thực nghiệm: Luồng dữ liệu HTTP thô](#2-nghiên-cứu-thực-nghiệm-luồng-dữ-liệu-http-thô)

---

Chào các bạn! Sau khi đã hoàn thành nghiên cứu về hạ tầng định danh DNS ở Bài 4, chúng ta đã hiểu cách các thực thể tìm thấy nhau. Tuy nhiên, để "nói chuyện" được với nhau một cách chuyên nghiệp, chúng ta cần một ngôn ngữ chung. Bài nghiên cứu số 5 này sẽ tập trung hoàn toàn vào **HTTP (HyperText Transfer Protocol)** — "trái tim" của mọi giao tiếp mạng.

Chúng ta sẽ không học cách dùng các thư viện như `axios` hay `Spring Web` một cách hời hợt. Mục tiêu của bài này là **bóc tách lớp vỏ** để quan sát luồng dữ liệu thô chạy qua Socket, nghiên cứu cách Java xử lý từng byte dữ liệu từ JavaScript gửi tới.

---

### 1. Phân tích thực thể: Cấu trúc văn bản của giao thức HTTP

HTTP thực chất là một giao thức dựa trên văn bản (Text-based). Mọi "ý đồ" của JavaScript đều được đóng gói thành các dòng chữ cái gửi qua đường ống TCP.

#### 1.1. Cấu trúc Request từ JavaScript
Khi JavaScript thực thi một yêu cầu, nó tạo ra một cấu trúc gồm 3 phần chính mà Java Server phải bóc tách:
1.  **Request Line**: Chứa phương thức (GET, POST,...) và URI.
2.  **Headers**: Chứa Metadata (thông tin về thiết bị, kiểu dữ liệu, token bảo mật).
3.  **Entity Body**: Dữ liệu thực tế (thường là JSON).



#### 1.2. Cơ chế xử lý của Java (The Byte Processor)
Tại tầng Java, hệ thống không nhận được một "đối tượng" ngay lập tức. Nó nhận được một **InputStream**. Kỹ sư Java phải đọc từng byte, chuyển về định dạng ký tự, và phân tích cú pháp để hiểu được yêu cầu từ phía Frontend.

---

### 2. Nghiên cứu thực nghiệm: Luồng dữ liệu HTTP thô

Dưới đây là mã nguồn minh họa cách Java Server tiếp nhận và phản hồi một HTTP Request thô mà không cần bất kỳ framework nào. 

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;

/**
 * HTTP Raw Processor - Nghiên cứu cấu trúc giao thức
 * Tác giả: Trương Quang Trưởng
 */
public class HttpRawProcessor {
    public static void main(String[] args) {
        final int PORT = 9000;
        
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("[RESEARCH] HTTP Server đang lắng nghe tại cổng " + PORT);

            while (true) {
                try (Socket client = serverSocket.accept();
                     BufferedReader reader = new BufferedReader(new InputStreamReader(client.getInputStream()));
                     PrintWriter writer = new PrintWriter(client.getOutputStream())) {

                    // 1. Đọc Request Line
                    String requestLine = reader.readLine();
                    if (requestLine == null) continue;
                    System.out.println("\n[REQUEST] " + requestLine);

                    // 2. Đọc Headers (Dừng lại khi gặp dòng trống)
                    String header;
                    while ((header = reader.readLine()) != null && !header.isEmpty()) {
                        System.out.println("[HEADER] " + header);
                    }

                    // 3. Gửi Phản hồi HTTP chuẩn (Response)
                    String responseBody = "<html><body><h1>Hello from Java High-Level Research!</h1></body></html>";
                    
                    writer.println("HTTP/1.1 200 OK");
                    writer.println("Content-Type: text/html; charset=UTF-8");
                    writer.println("Content-Length: " + responseBody.getBytes(StandardCharsets.UTF_8).length);
                    writer.println("Connection: close");
                    writer.println(); // Dòng trống ngăn cách Header và Body
                    writer.println(responseBody);
                    writer.flush();

                } catch (Exception e) {
                    System.err.println("[ERR] Lỗi xử lý Request: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
