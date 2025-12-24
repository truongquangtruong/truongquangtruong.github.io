---
title: "Khóa Học #01: Chinh Phục Socket TCP - Từ Lý Thuyết Mô Hình OSI Đến Thực Thi Java Chuyên Sâu"
date: 2025-12-15
tags: ["java", "network", "socket", "tcp", "backend"]
categories: ["Java Network"]
draft: false
---

![TCP Sockets Overview](https://slideplayer.com/slide/15789990/88/images/22/TCP+Sockets%3A+Overview+Left+side%3A+client+Right+side%3A+server+socket%28%29.jpg)

---

### 📍 Mục lục nội dung
* [1. Socket là gì? Tầm quan trọng trong hạ tầng mạng](#phan-tich-1)
* [2. Tại sao lại là TCP? Phân tích cơ chế Bắt tay 3 bước](#phan-tich-2)
* [3. Kiến trúc Blocking I/O và dòng chảy dữ liệu](#phan-tich-3)
* [4. Triển khai Hệ thống Server chuyên nghiệp bằng Java](#phan-tich-4)

---

Chào các bạn! Đây là bài viết mở đầu cho chuỗi series đồ án lập trình mạng Java của mình. Để xây dựng được các hệ thống phức tạp như Chat, Game hay Web Server, chúng ta bắt buộc phải hiểu rõ nền tảng cốt lõi: **Socket TCP**. Trong bài này, mình sẽ cùng các bạn "mổ xẻ" từ lý thuyết mô hình OSI đến cách viết một Server có khả năng xử lý dữ liệu thực thụ.

<h3 id="phan-tich-1">1. Socket là gì? Tầm quan trọng trong hạ tầng mạng</h3>

Trong lập trình, **Socket** không phải là một giao thức, mà là một **Giao diện lập trình ứng dụng (API)**. Hãy tưởng tượng nó như một "điểm cuối" (endpoint) của một cuộc hội thoại. Để hai máy tính có thể nói chuyện với nhau, mỗi máy cần một Socket.

Nếu xét theo mô hình 7 tầng OSI, Socket nằm ở ranh giới giữa tầng **Application (Tầng 7)** và tầng **Transport (Tầng 4)**. Nó đóng vai trò như một "phiên dịch viên" cao cấp, giúp lập trình viên chúng ta gửi dữ liệu đi mà không cần quan tâm đến việc các bit điện tử chạy như thế nào dưới dây cáp hay qua các trạm router phức tạp.



<h3 id="phan-tich-2">2. Tại sao lại là TCP? Phân tích cơ chế Bắt tay 3 bước</h3>

Trong đồ án này, mình ưu tiên sử dụng TCP (Transmission Control Protocol) vì tính **Tin cậy tuyệt đối**. Khác với UDP (gửi và quên), TCP đảm bảo dữ liệu đến đích nguyên vẹn, không mất mát và đúng thứ tự.

Để làm được điều đó, TCP thực hiện quy trình **3-Way Handshake** cực kỳ chặt chẽ:
1.  **SYN (Synchronize):** Client gửi yêu cầu kết nối kèm một số thứ tự ngẫu nhiên $x$.
2.  **SYN-ACK (Acknowledgment):** Server phản hồi "Đã nhận", gửi lại $x+1$ và số thứ tự của chính mình $y$.
3.  **ACK:** Client gửi xác nhận cuối cùng $y+1$ để thiết lập đường truyền chính thức.



<h3 id="phan-tich-3">3. Kiến trúc Blocking I/O và dòng chảy dữ liệu</h3>

Mã nguồn dưới đây sử dụng mô hình **Blocking I/O**. Nghĩa là khi Server gọi hàm `accept()`, nó sẽ rơi vào trạng thái "ngủ đông" để lắng nghe card mạng. Chỉ khi có một Client kết nối vào, luồng chương trình mới tiếp tục chạy. Đây là cách tiếp cận trực quan nhất để nghiên cứu về dòng chảy (Stream) của dữ liệu giữa các thực thể mạng.

<h3 id="phan-tich-4">4. Triển khai Hệ thống Server chuyên nghiệp bằng Java</h3>

Dưới đây là đoạn mã nguồn Server mình đã tối ưu hóa, sử dụng bộ đệm (Buffer) để tăng hiệu suất đọc/ghi dữ liệu.

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
        // Cổng kết nối lắng nghe
        final int PORT = 8080;
        
        // Sử dụng try-with-resources để tự động quản lý đóng cổng socket
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== SERVER MẠNG CỦA TRƯỞNG ĐANG LẮNG NGHE TẠI CỔNG " + PORT + " ===");
            System.out.println("[LOG] Thời gian khởi tạo hệ thống: " + new Date());

            while (true) {
                // Chấp nhận kết nối từ Client (Cơ chế Blocking)
                try (Socket clientSocket = serverSocket.accept()) {
                    System.out.println("\n[+] Thiết bị kết nối từ: " + clientSocket.getInetAddress());

                    // Thiết lập luồng đọc/ghi với bộ đệm và Encoding UTF-8 chuẩn
                    BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream(), "UTF-8"));
                    PrintWriter writer = new PrintWriter(new OutputStreamWriter(clientSocket.getOutputStream(), "UTF-8"), true);

                    // Đọc dữ liệu từ Client gửi đến
                    String clientInput = reader.readLine();
                    if (clientInput != null) {
                        System.out.println("[DATA] Client gửi nội dung: " + clientInput);
                        
                        // Xử lý logic tại Server: Chuyển chữ hoa và gắn mốc thời gian
                        String result = "Xác nhận từ Server: [" + clientInput.toUpperCase() + "] - Đã xử lý vào " + new Date();
                        
                        // Gửi phản hồi về Client
                        writer.println(result);
                        System.out.println("[SEND] Đã phản hồi thành công.");
                    }
                } catch (IOException e) {
                    System.err.println("[ERR] Lỗi kết nối Client đơn lẻ: " + e.getMessage());
                }
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi hệ thống: Không thể chiếm dụng cổng " + PORT);
            e.printStackTrace();
        }
    }
}