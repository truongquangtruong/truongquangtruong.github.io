---
title: "Khóa Học #03: Nghiên cứu Đa luồng (Multi-threading) - Giải pháp tối ưu hóa năng lực xử lý cho thực thể Java Server"
date: 2025-12-17
tags: ["java", "multi-threading", "concurrency", "socket-server", "backend-research"]
categories: ["Java Network"]
draft: false
---

![Nghiên cứu xử lý đa luồng Java](https://i.imgur.com/uS2vMZx.png)

---

### 📍 Mục lục nội dung
1. [Thách thức của mô hình đơn luồng](#1-thách-thức-của-mô-hình-đơn-luồng)
2. [Cơ chế đa luồng (Multi-threading) trong Java](#2-cơ-chế-đa-luồng-multi-threading-trong-java)
3. [Triển khai mã nguồn Multi-threaded Server](#3-triển-khai-mã-nguồn-multi-threaded-server)

---

Chào các bạn! Ở Bài 1 và Bài 2, chúng ta đã xây dựng được một hệ thống Server-Client cơ bản. Tuy nhiên, qua thực nghiệm, nếu có 100 Client cùng kết nối một lúc, Server của chúng ta sẽ bị "treo". Bài nghiên cứu số 3 này sẽ giải quyết bài toán **Năng lực xử lý đồng thời (Concurrency)**.

### 1. Thách thức của mô hình đơn luồng
Trong các bài trước, Server sử dụng cơ chế **Blocking I/O** trên một luồng duy nhất. Khi một Client đang kết nối, Server bị "khóa" lại và không thể chấp nhận bất kỳ ai khác cho đến khi Client đó ngắt kết nối. Đây là nút thắt cổ chai cực lớn trong các hệ thống thực tế.

### 2. Cơ chế đa luồng (Multi-threading) trong Java
Để giải quyết vấn đề này, mỗi khi có một kết nối mới (`accept()`), thực thể Java Server sẽ sinh ra một "Worker" (luồng con) để phục vụ riêng cho Client đó. Server chính sau đó quay lại trạng thái lắng nghe ngay lập tức.



### 3. Triển khai mã nguồn Multi-threaded Server
Dưới đây là mã nguồn Server được nâng cấp với khả năng phục vụ vô số Client cùng lúc bằng cách sử dụng `Thread`.

```java
import java.io.*;
import java.net.*;

/**
 * Multi-threaded Server Research
 * Tác giả: Trương Quang Trưởng
 */
public class MultiThreadedServer {
    public static void main(String[] args) {
        int port = 8080;

        try (ServerSocket serverSocket = new ServerSocket(port)) {
            System.out.println("=== MULTI-THREADED SERVER [TRƯỞNG] STARTING AT PORT " + port + " ===");

            while (true) {
                Socket clientSocket = serverSocket.accept();
                System.out.println("[+] New Client Connected: " + clientSocket.getInetAddress());

                // Khởi tạo một luồng mới cho mỗi Client
                new Thread(new ClientHandler(clientSocket)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class ClientHandler implements Runnable {
    private Socket socket;

    public ClientHandler(Socket socket) {
        this.socket = socket;
    }

    @Override
    public void run() {
        try (BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {

            String input;
            while ((input = in.readLine()) != null) {
                System.out.println("[THREAD " + Thread.currentThread().getId() + "] Received: " + input);
                out.println("Server Echo: " + input);
            }
        } catch (IOException e) {
            System.err.println("Thread Error: " + e.getMessage());
        }
    }
}
|