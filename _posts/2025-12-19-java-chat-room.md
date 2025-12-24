---
title: "Khóa Học #05: Xây Dựng Chat Room Đa Người Dùng - Kỹ Thuật Broadcast Và Tư Duy Thiết Kế Hệ Thống Tập Trung"
date: 2025-12-19
tags: ["java", "socket", "broadcast", "chat-app", "concurrency"]
categories: ["Java Network"]
draft: false
---

![Hệ thống Chat Room chuyên nghiệp](https://raw.githubusercontent.com/manish-msh/Socket-Programming-Java/master/Chat-Application.png)

Chào các bạn! Sau khi đã cùng nhau đi qua 4 bài học về TCP, I/O Streams, Đa luồng và UDP, hôm nay chúng ta sẽ bước vào một dấu mốc quan trọng nhất trong toàn bộ series: **Xây dựng ứng dụng Chat Room đa người dùng**. 

Đây không chỉ đơn thuần là một bài tập code, mà là bài học về **Tư duy kiến trúc hệ thống**. Chúng ta sẽ giải quyết bài toán làm thế nào để hàng chục, hàng trăm người có thể trò chuyện với nhau trong thời gian thực thông qua một Server trung tâm.

### 1. Phân tích bài toán: Cơ chế Broadcast là gì?
Trong một phòng chat, thách thức lớn nhất không phải là việc kết nối, mà là việc **phân phối thông tin**. Khi người dùng A gửi một tin nhắn "Chào cả nhà!", làm sao để người dùng B, C, D và E đều nhận được tin nhắn đó ngay lập tức?



Giải pháp chính là **Broadcast (Truyền tin quảng bá)**. Server sẽ đóng vai trò như một điều phối viên:
1. Duy trì một danh sách các kết nối đang hoạt động.
2. Khi nhận được tin nhắn từ một Client, Server sẽ lặp qua danh sách đó.
3. Gửi bản sao tin nhắn cho tất cả mọi người trong danh sách (ngoại trừ chính người vừa gửi).

### 2. Thiết kế cấu trúc dữ liệu: Quản lý ClientHandler
Để Server không bị "rối" khi có quá nhiều người vào, mình sử dụng lớp `ClientHandler` (đã học ở Bài 3) để đại diện cho mỗi người dùng. Toàn bộ các Handler này sẽ được quản lý tập trung trong một cấu trúc dữ liệu an toàn luồng (Thread-safe).

Trong đồ án này, mình ưu tiên sử dụng `CopyOnWriteArrayList`. Tại sao? Vì trong phòng chat, việc người dùng vào/ra (đọc/ghi danh sách) xảy ra liên tục. `ArrayList` thông thường sẽ gây ra lỗi `ConcurrentModificationException` nếu một luồng đang gửi tin nhắn trong khi luồng khác lại xóa một người dùng vừa thoát.

### 3. Triển khai mã nguồn Chat Server "Siêu cấp"
Đây là bộ mã nguồn mình đã tinh chỉnh, tích hợp bộ đệm tối ưu và cơ chế quản lý danh sách thông minh.

```java
import java.io.*;
import java.net.*;
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

/**
 * Hệ thống Chat Server Đa Người Dùng
 * Thực hiện: Trương Quang Trưởng
 */
public class AdvancedChatServer {
    private static final int PORT = 8080;
    // Danh sách quản lý tất cả khách hàng đang online (Thread-safe)
    private static List<ChatHandler> clients = new CopyOnWriteArrayList<>();

    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            System.out.println("=== CHAT SERVER [TRƯỞNG] ĐANG HOẠT ĐỘNG TẠI CỔNG " + PORT + " ===");

            while (true) {
                Socket socket = serverSocket.accept();
                System.out.println("[SYSTEM] Người dùng mới kết nối: " + socket.getInetAddress());

                ChatHandler handler = new ChatHandler(socket);
                clients.add(handler); // Thêm vào danh sách quản lý chung
                new Thread(handler).start();
            }
        } catch (IOException e) {
            System.err.println("[CRITICAL] Lỗi khởi động Server: " + e.getMessage());
        }
    }

    /**
     * Hàm gửi tin nhắn tới tất cả mọi người (Broadcast)
     */
    public static void broadcast(String message, ChatHandler sender) {
        for (ChatHandler client : clients) {
            // Không gửi lại cho chính người vừa nhắn
            if (client != sender) {
                client.sendMessage(message);
            }
        }
    }

    /**
     * Xóa người dùng khi họ offline
     */
    public static void removeClient(ChatHandler handler) {
        clients.remove(handler);
        System.out.println("[SYSTEM] Một người dùng đã rời phòng chat.");
    }
}

class ChatHandler implements Runnable {
    private Socket socket;
    private BufferedReader in;
    private PrintWriter out;
    private String userName;

    public ChatHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try {
            in = new BufferedReader(new InputStreamReader(socket.getInputStream(), "UTF-8"));
            out = new PrintWriter(new OutputStreamWriter(socket.getOutputStream(), "UTF-8"), true);

            // Bước 1: Yêu cầu nhập tên
            out.println("NHẬP TÊN CỦA BẠN: ");
            this.userName = in.readLine();
            AdvancedChatServer.broadcast("[HỆ THỐNG] " + userName + " đã tham gia phòng chat!", this);

            // Bước 2: Vòng lặp nhận tin nhắn
            String input;
            while ((input = in.readLine()) != null) {
                if (input.equalsIgnoreCase("/quit")) break;
                System.out.println("[" + userName + "]: " + input);
                AdvancedChatServer.broadcast("[" + userName + "]: " + input, this);
            }
        } catch (IOException e) {
            System.err.println("[ERR] Lỗi kết nối với " + userName);
        } finally {
            // Bước 3: Dọn dẹp khi thoát
            AdvancedChatServer.removeClient(this);
            AdvancedChatServer.broadcast("[HỆ THỐNG] " + userName + " đã rời cuộc trò chuyện.", this);
            try { socket.close(); } catch (IOException e) { e.printStackTrace(); }
        }
    }

    public void sendMessage(String message) {
        out.println(message);
    }
}