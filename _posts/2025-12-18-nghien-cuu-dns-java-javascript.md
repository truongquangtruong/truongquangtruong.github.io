---
title: "Khóa Học #04: Nghiên cứu Chuyên sâu Hệ thống Định danh DNS - Sự giao thoa hạ tầng giữa Java Backend và JavaScript Frontend"
date: 2025-12-18
tags: ["java", "javascript", "dns", "networking-research", "fullstack", "infrastructure"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu hạ tầng kết nối Java và JavaScript](https://tuhoclaptrinh.edu.vn/upload/post/2022/11/16/java-va-javascript-co-gi-khac-nhau-20221116104648-196233.jpg)

Chào các bạn! Trong hành trình trở thành một Fullstack Developer chuyên nghiệp, chúng ta không thể chỉ dừng lại ở việc biết viết code. Một kỹ sư thực thụ cần hiểu rõ cách các thực thể phần mềm tìm thấy nhau trong không gian mạng bao la. 

Hôm nay, chúng ta sẽ thực hiện một bài **nghiên cứu thực nghiệm quy mô lớn** về DNS (Domain Name System). Bài viết này sẽ bóc tách từng lớp mặt nạ của hạ tầng mạng để trả lời câu hỏi: *Làm sao thực thể JavaScript trên trình duyệt và thực thể Java trên Server có thể đồng bộ hóa định danh để thiết lập một kênh truyền dữ liệu an toàn?*

---

### 1. Phân tích cơ chế: Hành trình của một gói tin định danh

Khi bạn thực thi một yêu cầu mạng từ JavaScript, một chuỗi truy vấn định danh xuyên lục địa được kích hoạt để tìm đến "vùng đất" của Java.



#### 1.1. Tầng thực thi JavaScript (Client-side)
JavaScript sống trong trình duyệt, và trình duyệt là "kẻ canh cổng" đầu tiên trong việc phân giải.
* **DNS Prefetching**: Các trình duyệt hiện đại (Chrome, Firefox) nghiên cứu trước các liên kết và phân giải IP ngay cả khi người dùng chưa tương tác.
* **HSTS & DNS**: Nếu tên miền Java Server nằm trong danh sách HSTS, trình duyệt sẽ ép buộc mọi truy vấn phải qua kênh an toàn, ảnh hưởng đến cách DNS được tra cứu từ JS.

#### 1.2. Tầng trung gian: Recursive Resolver & TLD
Đây là nơi gói tin UDP/TCP cổng 53 hoạt động. Nó hỏi qua các Root Server, TLD Server cho đến khi tìm thấy địa chỉ IP thực sự của máy chủ Java.

#### 1.3. Tầng Java Server (The Destination)
Tại đây, Java không chỉ nhận gói tin. Nó thường thực hiện các truy vấn **Reverse DNS** để xác minh xem thực thể JavaScript đang gọi tới có phải là một IP giả mạo hay không.

---

### 2. Nghiên cứu thực nghiệm: So sánh tư duy Java và JavaScript

Qua đo lường, chúng ta thấy rõ sự khác biệt trong cách hai nền tảng xử lý định danh mạng:

| Đặc điểm nghiên cứu | JavaScript (Frontend) | Java (Backend) |
| :--- | :--- | :--- |
| **Quyền kiểm soát** | Bị động (Phụ thuộc trình duyệt/OS). | Chủ động (Can thiệp sâu vào JVM). |
| **Cơ chế Cache** | Ngắn hạn, khó can thiệp. | Thường là dài hạn, có thể cấu hình tay. |
| **Giao thức** | Chỉ qua API Browser (fetch/xhr). | Có thể tự đóng gói tin DNS thô (Raw). |
| **Mục tiêu chính** | Tìm kiếm điểm cuối (Endpoint). | Xác thực và điều phối luồng vào. |

---

### 3. Mã nguồn thực nghiệm: DNS Deep Profiler (Java)

Để minh chứng cho khả năng can thiệp sâu của Java vào hạ tầng, chúng ta sẽ viết một công cụ không chỉ lấy IP mà còn đo lường độ trễ chính xác đến từng phần triệu giây.

```java
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.security.Security;

/**
 * DNS Deep Profiler - Công cụ nghiên cứu thực nghiệm hạ tầng mạng
 * Tác giả: Trương Quang Trưởng
 */
public class DNSDeepProfiler {
    public static void main(String[] args) {
        String targetDomain = "api.google.com"; 

        // 1. Nghiên cứu cấu hình Cache của JVM
        // Mặc định Java lưu cache DNS rất lâu để tối ưu hiệu năng
        String ttl = Security.getProperty("networkaddress.cache.ttl");
        System.out.println("[CONFIG] JVM DNS TTL hiện tại: " + (ttl == null ? "30s (Mặc định)" : ttl + "s"));

        try {
            System.out.println("\n--- BẮT ĐẦU PHÂN TÍCH THỰC THỂ: " + targetDomain + " ---");

            //