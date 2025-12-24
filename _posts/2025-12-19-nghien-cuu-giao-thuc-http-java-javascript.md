---
title: "Khóa Học #05: Nghiên cứu Thượng tầng Giao thức HTTP - Kiến trúc Truyền tải và Đồng bộ hóa Thực thể Java-JavaScript"
date: 2025-12-19
tags: ["java", "javascript", "http-protocol", "network-research", "fullstack", "deep-dive", "low-level"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu giao thức truyền tải dữ liệu](https://npp.com.vn/wp-content/uploads/2024/06/dong-bo-hoa-du-lieu.jpg)

<div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 10px; padding: 20px; margin-bottom: 30px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <h4 style="margin-top: 0; color: #007bff; display: flex; align-items: center;">
    <span style="margin-right: 10px;">📍</span> Mục lục nội dung
  </h4>
  <div style="color: #2d3748; line-height: 1.6;">

* TOC
{:toc}

  </div>
</div>

Chào các bạn! Sau khi đã hoàn thành nghiên cứu về hạ tầng định danh DNS ở Bài 4, chúng ta đã hiểu cách các thực thể tìm thấy nhau. Tuy nhiên, để "nói chuyện" được với nhau một cách chuyên nghiệp, chúng ta cần một ngôn ngữ chung. Bài nghiên cứu số 5 này sẽ tập trung hoàn toàn vào **HTTP (HyperText Transfer Protocol)** — "trái tim" của mọi giao tiếp mạng hiện đại.

Chúng ta sẽ không học cách dùng các thư viện như `axios` hay `Spring Web` một cách hời hợt. Mục tiêu của bài này là **bóc tách lớp vỏ** để quan sát luồng dữ liệu thô chạy qua Socket, nghiên cứu cách Java xử lý từng byte dữ liệu từ JavaScript gửi tới.

---

### 1. Phân tích thực thể: Cấu trúc văn bản của giao thức HTTP

HTTP thực chất là một giao thức dựa trên văn bản (Text-based). Mọi "ý đồ" của JavaScript đều được đóng gói thành các dòng chữ cái ASCII gửi qua đường ống TCP. 



#### 1.1. Cấu trúc Request từ JavaScript
Khi JavaScript thực thi một yêu cầu (ví dụ qua lệnh `fetch`), nó tạo ra một cấu trúc gồm 3 phần chính mà Java Server phải bóc tách:
1.  **Request Line**: Chứa phương thức (GET, POST, PUT, DELETE), URI và phiên bản giao thức (HTTP/1.1).
2.  **Headers**: Chứa Metadata như `User-Agent` (định danh trình duyệt), `Content-Type` (kiểu dữ liệu JSON/XML), và `Authorization` (token bảo mật).
3.  **Entity Body**: Dữ liệu thực tế được gửi đi. Đây là nơi chứa các Object JSON phức tạp cần được đồng bộ hóa sang thực thể Java.

#### 1.2. Tính chất "Stateless" và Thách thức đối với Java Server
HTTP là một giao thức **không lưu trạng thái (Stateless)**. Mỗi Request là độc lập. Điều này buộc phía Java Server phải có các cơ chế như Session hoặc JWT để nhận diện lại thực thể JavaScript trong các lần gọi tiếp theo. Trong đồ án này, chúng ta sẽ nghiên cứu cách bóc tách Header `Cookie` hoặc `Authorization` từ luồng Byte thô để xử lý định danh này.

---

### 2. Nghiên cứu thực nghiệm: Cơ chế "The Byte Processor" trong Java

Tại tầng Java Socket, hệ thống không nhận được một "đối tượng" (Object) ngay lập tức. Nó nhận được một **InputStream** (luồng byte). 



Kỹ sư Java phải thực hiện quy trình sau:
1.  **Read**: Đọc từng byte từ Socket.
2.  **Decode**: Chuyển đổi Byte sang ký tự dựa trên bảng mã UTF-8.
3.  **Parse**: Sử dụng các thuật toán xử lý chuỗi để tách biệt đâu là Method, đâu là Header và đâu là Body.

---

### 3. Triển khai mã nguồn: HTTP Raw Processor (Pure Java)

Dưới đây là mã nguồn thực nghiệm nâng cao. Server này không chỉ phản hồi mà còn "soi" chi tiết cấu trúc gói tin mà trình duyệt (JavaScript) gửi đến để chúng ta quan sát.

```java
import java.io.*;
import java.net.*;
import java.nio.charset.StandardCharsets;
import java.util.Date;

/**
 * HTTP Raw Processor - Nghiên cứu sâu cấu trúc giao thức
 * Tác giả: Trương Quang Trưởng
 */
public class HttpRawProcessor {
    public static void main(String[] args) {
        final int PORT = 9000;
        
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== HỆ THỐNG NGHIÊN CỨU HTTP [TRƯỞNG] STARTING AT PORT " + PORT + " ===");

            while (true) {
                try (Socket client = serverSocket.accept();
                     BufferedReader reader = new BufferedReader(new InputStreamReader(client.getInputStream(), StandardCharsets.UTF_8));
                     OutputStream output = client.getOutputStream();
                     PrintWriter writer = new PrintWriter(new OutputStreamWriter(output, StandardCharsets.UTF_8))) {

                    // 1. PHÂN TÍCH REQUEST LINE
                    String requestLine = reader.readLine();
                    if (requestLine == null) continue;
                    System.out.println("\n[MÉTHOD & URI]: " + requestLine);

                    // 2. PHÂN TÍCH HEADERS
                    String header;
                    int contentLength = 0;
                    while (!(header = reader.readLine()).isEmpty()) {
                        System.out.println("[HEADER LOG]: " + header);
                        // Tìm Header Content-Length để biết độ dài Body
                        if (header.startsWith("Content-Length:")) {
                            contentLength = Integer.parseInt(header.split(":")[1].trim());
                        }
                    }

                    // 3. PHÂN TÍCH BODY (Nếu có)
                    if (contentLength > 0) {
                        char[] bodyChars = new char[contentLength];
                        reader.read(bodyChars, 0, contentLength);
                        System.out.println("[BODY DATA]: " + new String(bodyChars));
                    }

                    // 4. PHẢN HỒI HTTP RESPONSE CHUẨN (Low-level implementation)
                    String responseBody = "{ \"status\": \"success\", \"message\": \"Hello from Java Backend Research\", \"time\": \"" + new Date() + "\" }";
                    
                    writer.print("HTTP/1.1 200 OK\r\n"); // Status Line
                    writer.print("Content-Type: application/json; charset=UTF-8\r\n");
                    writer.print("Content-Length: " + responseBody.getBytes(StandardCharsets.UTF_8).length + "\r\n");
                    writer.print("Access-Control-Allow-Origin: *\r\n"); // Cho phép JavaScript gọi từ domain khác
                    writer.print("Connection: close\r\n");
                    writer.print("\r\n"); // Dòng trống bắt buộc
                    writer.print(responseBody);
                    writer.flush();

                    System.out.println("[SUCCESS] Đã phản hồi thực thể JavaScript.");

                } catch (Exception e) {
                    System.err.println("[ERR] Lỗi phân tích gói tin: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Không thể khởi tạo Server nghiên cứu: " + e.getMessage());
        }
    }
}