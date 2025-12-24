---
title: "Khóa Học #08: Nghiên cứu WebSockets - Cơ chế truyền tải dữ liệu thời gian thực giữa Java và JavaScript"
date: 2025-12-22
tags: ["java", "javascript", "websockets", "real-time", "networking-research", "fullstack", "concurrency"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu kết nối thời gian thực WebSockets](https://imgopt.infoq.com/fit-in/1200x2400/filters:quality(80)/filters:no_upscale()/articles/serverless-websockets-realtime-messaging/en/resources/93-1669754025734.jpeg)

Chào các bạn! Sau khi đã xây dựng được các lớp bảo mật vững chắc ở Bài 7, hôm nay chúng ta sẽ tiến tới một đỉnh cao mới trong việc kết nối thực thể: **Giao tiếp thời gian thực (Real-time Communication)**.

Trong các bài nghiên cứu trước, chúng ta thấy JavaScript luôn đóng vai trò "khách" (Client) chủ động hỏi và Java đóng vai trò "chủ" (Server) trả lời. Tuy nhiên, kiến trúc này gặp giới hạn lớn khi cần cập nhật dữ liệu liên tục như bảng giá chứng khoán hay hệ thống giám sát. Bài nghiên cứu số 8 này sẽ bóc tách **WebSockets** — giao thức phá vỡ rào cản Request-Response để tạo ra một đường ống dẫn dữ liệu song công (Full-duplex) hoàn hảo.

---

### 1. Phân tích thực thể: Sự tiến hóa của giao tiếp liên tầng

Nghiên cứu thực nghiệm chứng minh rằng WebSockets không phải là một sự thay thế hoàn toàn cho HTTP, mà là một sự **"thăng hoa" (Upgrade)** của giao thức này.

#### 1.1. Cơ chế Handshake và mã xác thực 258EAFA5
Mọi kết nối bắt đầu bằng một yêu cầu HTTP GET kèm Header `Upgrade: websocket`. Điểm thú vị nhất là mã `258EAFA5-E914-47DA-95CA-C5AB0DC85B11` — một chuỗi định danh (GUID) cố định toàn cầu. Java Server phải dùng chuỗi này cộng với `Sec-WebSocket-Key` từ JavaScript, sau đó băm (Hash) qua SHA-1 và mã hóa Base64 để chứng minh với trình duyệt rằng: "Tôi thực sự hiểu giao thức WebSocket".



#### 1.2. Khái niệm Statefulness (Duy trì trạng thái)
Khác với HTTP (vốn là Stateless - Bài 5), WebSockets là **Stateful**. Java Server phải duy trì một phiên làm việc (Session) mở liên tục trong bộ nhớ RAM. Điều này cho phép thực thể Java nhận biết chính xác đối tượng JavaScript nào đang kết nối mà không cần gửi lại Token xác thực trong mỗi gói tin.

---

### 2. Nghiên cứu thực nghiệm: Xây dựng WebSocket Server bằng Java NIO

Trong bài thực nghiệm này, chúng ta sẽ đi sâu vào việc xử lý dữ liệu ở tầng thấp nhất: **Phân tích Byte và Frame**. WebSocket không gửi văn bản thuần túy như HTTP, nó đóng gói dữ liệu vào các "Khung" (Frames).

```java
import java.io.*;
import java.net.*;
import java.security.MessageDigest;
import java.util.Base64;
import java.util.Scanner;
import java.util.regex.*;

/**
 * Advanced WebSocket Concurrency Research Tool
 * Nghiên cứu cơ chế bắt tay và quản lý Frame dữ liệu cấp thấp
 * Tác giả: Trương Quang Trưởng
 */
public class DeepWebSocketServer {
    public static void main(String[] args) throws Exception {
        ServerSocket server = new ServerSocket(8080);
        System.out.println("====================================================");
        System.out.println("HỆ THỐNG NGHIÊN CỨU REAL-TIME [TRƯỞNG] KHỞI ĐỘNG...");
        System.out.println("====================================================");

        while (true) {
            Socket client = server.accept();
            new Thread(() -> { // Nghiên cứu mô hình đa luồng xử lý đồng thời
                try {
                    InputStream in = client.getInputStream();
                    OutputStream out = client.getOutputStream();
                    Scanner s = new Scanner(in, "UTF-8");

                    // 1. Phân tích Handshake thô từ JavaScript
                    String data = s.useDelimiter("\\r\\n\\r\\n").next();
                    System.out.println("[HANDSHAKE] Yêu cầu từ Client:\n" + data);

                    Matcher match = Pattern.compile("Sec-WebSocket-Key: (.*)").matcher(data);
                    if (match.find()) {
                        String key = match.group(1).trim();
                        String acceptKey = Base64.getEncoder().encodeToString(
                            MessageDigest.getInstance("SHA-1").digest(
                                (key + "258EAFA5-E914-47DA-95CA-C5AB0DC85B11").getBytes("UTF-8")
                            )
                        );

                        // Phản hồi nâng cấp giao thức thành công
                        PrintWriter pw = new PrintWriter(out);
                        pw.print("HTTP/1.1 101 Switching Protocols\r\n");
                        pw.print("Upgrade: websocket\r\n");
                        pw.print("Connection: Upgrade\r\n");
                        pw.print("Sec-WebSocket-Accept: " + acceptKey + "\r\n\r\n");
                        pw.flush();

                        System.out.println("[SUCCESS] Thực thể Java đã thiết lập kênh Real-time.");

                        // 2. Nghiên cứu Pushing Frame (Đẩy dữ liệu định kỳ)
                        while (true) {
                            Thread.sleep(5000); // Cứ 5s đẩy một bản tin phân tích
                            String msg = "Bản tin nghiên cứu #" + System.currentTimeMillis();
                            byte[] rawData = msg.getBytes("UTF-8");

                            // Cấu hình Frame: Fin=1, Opcode=1 (Text)
                            out.write(0x81); 
                            out.write(rawData.length); // Lưu ý: Chỉ hỗ trợ dữ liệu < 126 bytes trong bài test này
                            out.write(rawData);
                            out.flush();
                            System.out.println("[PUSH] Đã đẩy dữ liệu xuống JS: " + msg);
                        }
                    }
                } catch (Exception e) {
                    System.err.println("[ERROR] Ngắt kết nối: " + e.getMessage());
                }
            }).start();
        }
    }
}