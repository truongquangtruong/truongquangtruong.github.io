---
title: "Khóa Học #09: Nghiên cứu Kiến trúc Microservices - Phân rã và Kết nối thực thể Java trong hệ thống Fullstack"
date: 2025-12-23
tags: ["java", "javascript", "microservices", "distributed-systems", "api-gateway", "fullstack-research", "software-architecture"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu kiến trúc Microservices - Lợi ích và Thách thức](https://jamstackvietnam.com/uploads/Blog/2023-07-04-14-57-microservices-loi-ich-va-thach-thuc-khi-su-dung-trong-website-jamstack.jpg)

Chào các bạn! Sau khi đã làm chủ việc truyền tải dữ liệu thời gian thực ở Bài 8, hôm nay chúng ta sẽ bước lên một cấp độ tư duy hoàn toàn mới: **Kiến trúc hệ thống phân tán (Distributed Systems)** thông qua mô hình **Microservices**.

Trong lộ trình nghiên cứu, nếu Java là "linh hồn" xử lý và JavaScript là "giao diện" tương tác, thì Microservices chính là phương pháp để chúng ta tổ chức các linh hồn đó thành một đội quân tinh nhuệ, thay vì một gã khổng lồ chậm chạp.

---

### 1. Phân tích thực thể: Sự sụp đổ của "Đế chế" Monolith

Trong các nghiên cứu trước đây (từ Bài 1 đến Bài 8), chúng ta thường xây dựng ứng dụng theo kiểu **Monolith (Đơn khối)**. Mọi thứ từ xác thực, quản lý sản phẩm, đến thanh toán đều đóng gói vào một file `.jar` hoặc `.war` duy nhất.

#### 1.1. Những "nỗi đau" thực nghiệm
Nghiên cứu sâu vào vận hành, mô hình đơn khối bộc lộ những hạn chế chí mạng:
* **Sự cồng kềnh (Large Codebase):** Khi số lượng dòng code vượt mức hàng triệu, việc khởi động Server Java có thể mất cả chục phút.
* **Rủi ro triển khai (Deployment Risk):** Chỉ cần sửa một dòng code nhỏ ở phần "Gửi Mail", bạn phải build lại và restart toàn bộ hệ thống, gây gián đoạn cho cả phần "Thanh toán".
* **Nút thắt cổ chai về công nghệ:** Nếu một module cần tính toán AI (phù hợp với Python) nhưng cả khối đang chạy Java, bạn buộc phải hy sinh hiệu năng để viết bằng Java.

#### 1.2. Cuộc cách mạng Microservices
Microservices không chỉ là chia nhỏ code, mà là chia nhỏ **trách nhiệm**. Mỗi dịch vụ là một thực thể độc lập:
* Đứng độc lập (Independently Deployable).
* Có Database riêng (Database per Service).
* Giao tiếp qua mạng (HTTP/REST, gRPC, hoặc Message Broker).

---

### 2. Nghiên cứu thực nghiệm: Mô hình API Gateway đa luồng và Điều hướng thông minh

Trong hệ thống Microservices, thực thể JavaScript (Frontend) không nên biết có bao nhiêu dịch vụ Java phía sau. Nó chỉ cần biết một điểm chạm duy nhất: **API Gateway**.

Dưới đây là mã nguồn nghiên cứu một Gateway có khả năng quản lý luồng dữ liệu phức tạp:

```java
import java.io.*;
import java.net.*;
import java.util.concurrent.*;

/**
 * Advanced Microservices API Gateway Research
 * Cơ chế Routing, Load Balancing và Thread Management
 * Tác giả: Trương Quang Trưởng
 */
public class AdvancedJavaGateway {
    private static final int PORT = 8000;
    // Sử dụng ThreadPool để tối ưu hóa tài nguyên, tránh tạo Thread vô hạn
    private static final ExecutorService virtualThreads = Executors.newFixedThreadPool(100);

    public static void main(String[] args) throws IOException {
        ServerSocket gateway = new ServerSocket(PORT);
        System.out.println("----------------------------------------------------");
        System.out.println("CORE GATEWAY SYSTEM [TRƯỞNG] - CHẾ ĐỘ PHÂN TÁC");
        System.out.println("----------------------------------------------------");

        while (true) {
            Socket clientRequest = gateway.accept();
            virtualThreads.execute(() -> handleRouting(clientRequest));
        }
    }

    private static void handleRouting(Socket client) {
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(client.getInputStream()));
             PrintWriter writer = new PrintWriter(client.getOutputStream())) {

            String header = reader.readLine();
            if (header == null) return;

            System.out.println("[INCOMING] Yêu cầu: " + header);

            // Logic Routing thực nghiệm dựa trên Endpoint
            String responseBody;
            if (header.contains("/services/auth")) {
                responseBody = forwardTo("AUTH-SERVICE-01", "Port: 8081", "JWT-Manager");
            } else if (header.contains("/services/inventory")) {
                responseBody = forwardTo("INVENTORY-SERVICE-05", "Port: 8082", "Stock-Manager");
            } else if (header.contains("/services/billing")) {
                responseBody = forwardTo("BILLING-SERVICE-02", "Port: 8083", "Payment-Gateway");
            } else {
                responseBody = "{\"error\": \"Service Not Found\", \"code\": 404}";
            }

            // Trả kết quả về cho JavaScript
            writer.println("HTTP/1.1 200 OK");
            writer.println("Content-Type: application/json");
            writer.println("Access-Control-Allow-Origin: *");
            writer.println();
            writer.println(responseBody);
            writer.flush();

        } catch (Exception e) {
            System.err.println("[CRITICAL ERROR] Gateway Failure: " + e.getMessage());
        }
    }

    private static String forwardTo(String name, String address, String module) {
        System.out.println("   >>> Đang điều hướng đến: " + name + " (" + address + ")");
        return String.format("{\"target\": \"%s\", \"address\": \"%s\", \"module\": \"%s\", \"status\": \"success\"}", 
                             name, address, module);
    }
}