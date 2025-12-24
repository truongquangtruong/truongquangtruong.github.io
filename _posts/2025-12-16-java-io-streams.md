---
title: "Khóa Học #02: Nghiên cứu Thực thể Client - Phân tích Luồng Gửi và Nhận Dữ liệu trong Mạng TCP"
date: 2025-12-16
tags: ["java", "network", "socket-client", "tcp", "backend"]
categories: ["Java Network"]
draft: false
---

![Nghiên cứu thực thể Client](https://securitydaily.net/wp-content/uploads/2014/11/Client-Server-700x350.png)

---

### 📍 Mục lục nội dung
1. [Bản chất của thực thể Client](#1-bản-chất-của-thực-thể-client)
2. [Cơ chế luồng dữ liệu (Input/Output Stream)](#2-cơ-chế-luồng-dữ-liệu-inputoutput-stream)
3. [Triển khai mã nguồn Client nghiên cứu](#3-triển-khai-mã-nguồn-client-nghiên-cứu)

---

Chào các bạn! Sau khi đã xây dựng xong "ngôi nhà" Server ở Bài 1, hôm nay chúng ta sẽ tạo ra một **Thực thể Client**. Trong mô hình mạng, nếu Server là bên cung cấp dịch vụ thì Client chính là bên khởi tạo nhu cầu. Việc hiểu cách Client đóng gói gói tin là bước đệm quan trọng để chúng ta tiến tới nghiên cứu JavaScript ở các bài sau.

### 1. Bản chất của thực thể Client
Client không đứng yên chờ đợi như Server. Nó chủ động thực hiện một cuộc gọi đến địa chỉ IP và Port xác định. Trong đồ án này, chúng ta sẽ nghiên cứu cách một ứng dụng Java Client tìm thấy Server của chính mình trên cùng một máy chủ thông qua địa chỉ `localhost` (127.0.0.1).

### 2. Cơ chế luồng dữ liệu (Input/Output Stream)
Để giao tiếp được, thực thể Client cần hai "đường ống":
* **OutputStream**: Để đẩy dữ liệu từ bộ nhớ RAM ra card mạng, gửi tới Server.
* **InputStream**: Để hứng dữ liệu mà Server phản hồi về.



### 3. Triển khai mã nguồn Client nghiên cứu
Dưới đây là mã nguồn Client được thiết kế để tương tác trực tiếp với Server ở Bài 1.

```java
import java.io.*;
import java.net.*;
import java.util.Scanner;

/**
 * Thực thể Client nghiên cứu
 * Tác giả: Trương Quang Trưởng
 */
public class ResearchClient {
    public static void main(String[] args) {
        String serverAddress = "127.0.0.1";
        int port = 8080;

        try (Socket socket = new Socket(serverAddress, port);
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream(), "UTF-8"))) {

            System.out.println("=== CLIENT NGHIÊN CỨU [TRƯỞNG] ĐÃ KẾT NỐI ===");
            
            // Nhập liệu thủ công để kiểm tra tính thời gian thực
            Scanner scanner = new Scanner(System.in);
            System.out.print("Nhập thông điệp gửi Server: ");
            String message = scanner.nextLine();

            // 1. Gửi dữ liệu qua OutputStream
            out.println(message);
            System.out.println("[SEND] Đã gửi: " + message);

            // 2. Chờ nhận phản hồi từ InputStream
            String response = in.readLine();
            System.out.println("[RECEIVE] Server phản hồi: " + response);

        } catch (IOException e) {
            System.err.println("[ERR] Không thể kết nối tới Server: " + e.getMessage());
        }
    }
}
| Bài trước | Trang chủ | Bài tiếp theo |
| :--- | :---: | ---: |
| [⬅️ Bài #01](/java%20network%20research/2025/12/15/bai-01/) | [🏠 Danh sách](/) | [Bài #03: Đa luồng ➡️](/java%20network%20research/2025/12/17/bai-03/) |