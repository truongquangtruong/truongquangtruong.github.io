---
title: "Khóa Học #01: Chinh Phục Socket TCP - Từ Lý Thuyết Mô Hình OSI Đến Thực Thi Java Chuyên Sâu"
date: 2025-12-15
tags: ["java", "network", "socket", "tcp", "backend"]
categories: ["Java Network"]
draft: false
---

![Mô hình OSI và TCP/IP Socket](https://slideplayer.com/slide/15789990/88/images/22/TCP+Sockets%3A+Overview+Left+side%3A+client+Right+side%3A+server+socket%28%29.jpg)

---

### 📍 Mục lục nội dung
* [1. Socket là gì? Vị trí trong mô hình OSI](#phan-tich-1)
* [2. Cơ chế Bắt tay 3 bước (3-Way Handshake)](#phan-tich-2)
* [3. Triển khai Hệ thống Server chuyên nghiệp bằng Java](#phan-tich-3)

---

Chào các bạn! Đây là bài viết đầu tiên trong series đồ án lập trình mạng của mình. Để bắt đầu hành trình này, chúng ta sẽ cùng nhau "mổ xẻ" nền tảng quan trọng nhất của mọi kết nối tin cậy trên Internet: **Socket TCP**.

<h3 id="phan-tich-1">1. Socket là gì? Vị trí của nó trong thế giới kết nối</h3>
Trong quá trình tự nghiên cứu, mình nhận thấy Socket là một **Giao diện lập trình ứng dụng (API)**, một "điểm cuối" (endpoint) cho phép các tiến trình trao đổi dữ liệu qua mạng. 

<h3 id="phan-tich-2">2. Tại sao lại là TCP? Cơ chế "Bắt tay 3 bước"</h3>
TCP (Transmission Control Protocol) đảm bảo dữ liệu gửi đi sẽ đến đích nguyên vẹn và đúng thứ tự nhờ quy trình: **SYN -> SYN-ACK -> ACK**.

<h3 id="phan-tich-3">3. Triển khai Hệ thống Server chuyên nghiệp bằng Java</h3>

```java
import java.io.*;
import java.net.*;
import java.util.Date;

/**
 * Hệ thống Server Socket chuyên dụng
 * Thực hiện bởi: Trương Quang Trưởng
 */
public class ProfessionalTcpServer {
    public static void main(String[] args) {
        final int PORT = 8080;
        
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== SERVER [TRƯỞNG] ĐANG LẮNG NGHE TẠI CỔNG " + PORT + " ===");

            while (true) {
                try (Socket clientSocket = serverSocket.accept()) {
                    System.out.println("\n[+] Thiết bị kết nối: " + clientSocket.getInetAddress());

                    BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream(), "UTF-8"));
                    PrintWriter writer = new PrintWriter(new OutputStreamWriter(clientSocket.getOutputStream(), "UTF-8"), true);

                    String clientInput = reader.readLine();
                    if (clientInput != null) {
                        String result = "Xác nhận: [" + clientInput.toUpperCase() + "] - Đã xử lý vào " + new Date();
                        writer.println(result);
                    }
                } catch (IOException e) {
                    System.err.println("[ERR] Lỗi: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi hệ thống: " + e.getMessage());
        }
    }
}