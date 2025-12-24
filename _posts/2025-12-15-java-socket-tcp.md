---
title: "Khóa Học #01: Chinh Phục Socket TCP - Từ Lý Thuyết Mô Hình OSI Đến Thực Thi Java Chuyên Sâu"
date: 2025-12-15
tags: ["java", "network", "socket", "tcp", "backend"]
categories: ["Java Network"]
draft: false
---

 ![Mô hình OSI và TCP/IP](https://media.bkns.vn/uploads/2020/01/osi-chia-giao-tiep-mang-thanh-7-tang.jpg)

<div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 10px; padding: 20px; margin-bottom: 30px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <h4 style="margin-top: 0; color: #007bff; display: flex; align-items: center;">
    <span style="margin-right: 10px;">📍</span> Mục lục nội dung
  </h4>
  <div style="color: #2d3748; line-height: 1.6;">

* TOC
{:toc}

  </div>
</div>

Chào các bạn! Đây là bài viết đầu tiên trong series đồ án lập trình mạng của mình. Để bắt đầu hành trình này, chúng ta sẽ cùng nhau "mổ xẻ" nền tảng quan trọng nhất của mọi kết nối tin cậy trên Internet: **Socket TCP**.

### 1. Socket là gì? Vị trí của nó trong thế giới kết nối
Trong quá trình tự nghiên cứu, mình nhận thấy nhiều bạn thường nhầm lẫn Socket là một giao thức. Thực chất, Socket là một **Giao diện lập trình ứng dụng (API)**, một "điểm cuối" (endpoint) cho phép các tiến trình trao đổi dữ liệu với nhau qua mạng. 

Nếu xét theo mô hình 7 tầng OSI, Socket nằm ở ranh giới giữa tầng **Application (Tầng 7)** và tầng **Transport (Tầng 4)**. Nó giúp chúng ta (những lập trình viên Java) tương tác với các giao thức phức tạp ở tầng thấp mà không cần quan tâm đến việc bit dữ liệu di chuyển như thế nào trên dây cáp.



### 2. Tại sao lại là TCP? Cơ chế "Bắt tay 3 bước" (3-Way Handshake)
Tại sao trong đồ án này mình ưu tiên sử dụng TCP thay vì UDP? Câu trả lời nằm ở sự **Tin cậy**. TCP (Transmission Control Protocol) đảm bảo rằng dữ liệu bạn gửi đi sẽ đến đích nguyên vẹn, không mất mát và đúng thứ tự.

Để làm được điều đó, TCP thực hiện một quy trình thỏa thuận ngầm cực kỳ chặt chẽ trước khi truyền dữ liệu:
1.  **SYN (Synchronize)**: Client gửi một tín hiệu yêu cầu kết nối kèm theo một số thứ tự khởi tạo (Sequence Number).
2.  **SYN-ACK (Acknowledgment)**: Server phản hồi lại rằng "Tôi đã sẵn sàng" và gửi lại số thứ tự của chính mình.
3.  **ACK**: Client gửi xác nhận cuối cùng để chính thức thiết lập đường truyền.



### 3. Triển khai Hệ thống Server chuyên nghiệp bằng Java
Dưới đây là đoạn mã nguồn Server mà mình đã tối ưu hóa sau nhiều lần thử nghiệm. Khác với những ví dụ cơ bản trên mạng, Server này được thiết kế để xử lý dữ liệu có cấu trúc và quản lý tài nguyên hệ thống một cách an toàn.

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
        // Cổng kết nối mà chúng ta sẽ lắng nghe
        final int PORT = 8080;
        
        // Sử dụng try-with-resources để tự động đóng cổng sau khi sử dụng
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== SERVER MẠNG CỦA TRƯỞNG ĐANG LẮNG NGHE TẠI CỔNG " + PORT + " ===");
            System.out.println("[LOG] Thời gian khởi tạo hệ thống: " + new Date());

            while (true) {
                // Chấp nhận kết nối từ Client (Cơ chế Blocking I/O)
                try (Socket clientSocket = serverSocket.accept()) {
                    System.out.println("\n[+] Có thiết bị mới kết nối: " + clientSocket.getInetAddress());

                    // Thiết lập luồng đọc dữ liệu với bộ đệm (Buffer) để tối ưu hiệu suất
                    BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream(), "UTF-8"));
                    // Thiết lập luồng gửi dữ liệu phản hồi
                    PrintWriter writer = new PrintWriter(new OutputStreamWriter(clientSocket.getOutputStream(), "UTF-8"), true);

                    // Đọc tin nhắn từ Client gửi đến
                    String clientInput = reader.readLine();
                    if (clientInput != null) {
                        System.out.println("[DATA] Client gửi nội dung: " + clientInput);
                        
                        // Xử lý logic tại Server: Chuyển dữ liệu thành chữ in hoa và phản hồi
                        String result = "Xác nhận: [" + clientInput.toUpperCase() + "] - Đã xử lý vào " + new Date();
                        writer.println(result);
                        System.out.println("[SEND] Đã gửi phản hồi thành công.");
                    }
                } catch (IOException e) {
                    System.err.println("[ERR] Lỗi kết nối Client đơn lẻ: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi hệ thống: Không thể khởi động Server trên cổng " + PORT);
            e.printStackTrace();
        }
    }
}