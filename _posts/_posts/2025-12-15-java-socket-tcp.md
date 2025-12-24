---
layout: post
title: "Khóa học #01: Chinh phục Socket TCP - Từ lý thuyết mô hình OSI đến thực thi Java chuyên sâu"
date: 2025-12-15
categories: [Java, Network-Programming]
author: "Trương Quang Trưởng"
---

## 1. Đặt vấn đề và Mục tiêu nghiên cứu (Introduction)
Trong kỷ nguyên kết nối vạn vật, việc hiểu rõ cách thức các ứng dụng "nói chuyện" với nhau là kỹ năng sống còn của một lập trình viên. Qua quá trình tự tìm tòi và thực hiện đồ án này, tôi nhận thấy nhiều người chỉ dừng lại ở việc copy code Socket mà không hiểu bản chất luồng dữ liệu bên dưới. Bài viết này không chỉ là mã nguồn, mà là kết quả của việc nghiên cứu sâu về tầng Giao vận (Transport Layer) và cách Java hiện thực hóa nó một cách hoàn hảo.

## 2. Kiến trúc nền tảng: Socket nằm ở đâu trong mô hình OSI?
Để hiểu Socket, trước hết phải nhìn vào mô hình 7 tầng OSI. Socket không phải là một giao thức, nó là một **Giao diện lập trình ứng dụng (API)** nằm giữa tầng Ứng dụng (Application Layer) và tầng Giao vận (Transport Layer).



Khi chúng ta khởi tạo một Socket trong Java, chúng ta đang yêu cầu hệ điều hành cung cấp một tài nguyên mạng được định danh bởi bộ ba: **(Giao thức TCP, Địa chỉ IP, Số cổng Port)**. Nếu thiếu một trong ba yếu tố này, sự giao tiếp sẽ không bao giờ diễn ra.

## 3. Phân tích cơ chế tin cậy của TCP (Transmission Control Protocol)
TCP là giao thức hướng kết nối (Connection-oriented), đảm bảo dữ liệu đến đích đúng thứ tự và không mất mát. 

### 3.1. Cơ chế Bắt tay 3 bước (The 3-Way Handshake)
1. **Client gửi SYN (Synchronize):** Thông báo muốn kết nối kèm số thứ tự khởi tạo (Seq=X).
2. **Server phản hồi SYN-ACK:** Xác nhận đã nhận X (Ack=X+1) và gửi số thứ tự của mình (Seq=Y).
3. **Client gửi ACK (Acknowledge):** Xác nhận đã nhận Y (Ack=Y+1) và chính thức mở kênh truyền.



## 4. Hiện thực hóa bằng ngôn ngữ Java (Implementation)
```java
import java.io.*;
import java.net.*;
import java.util.Date;

public class ProfessionalTcpServer {
    public static void main(String[] args) {
        int PORT = 8080; 
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== HỆ THỐNG QUẢN TRỊ MẠNG CỦA TRƯỞNG ===");
            while (true) {
                try (Socket clientSocket = serverSocket.accept()) {
                    BufferedReader reader = new BufferedReader(new InputStreamReader(clientSocket.getInputStream(), "UTF-8"));
                    PrintWriter writer = new PrintWriter(new OutputStreamWriter(clientSocket.getOutputStream(), "UTF-8"), true);
                    String clientMessage = reader.readLine();
                    if (clientMessage != null) {
                        writer.println("Server xác nhận: [" + clientMessage.toUpperCase() + "] nhận lúc " + new Date());
                    }
                }
            }
        } catch (IOException e) { e.printStackTrace(); }
    }
}