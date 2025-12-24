---
title: "Khóa Học #05: Nghiên cứu Thượng tầng Giao thức HTTP - Kiến trúc Truyền tải và Đồng bộ hóa Thực thể Java-JavaScript"
date: 2025-12-19
tags: ["java", "javascript", "http-protocol", "network-research", "fullstack", "deep-dive", "low-level"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu giao thức truyền tải dữ liệu](https://npp.com.vn/wp-content/uploads/2024/06/dong-bo-hoa-du-lieu.jpg)

Chào các bạn! Sau khi đã hoàn thành nghiên cứu về hạ tầng định danh DNS ở Bài 4, chúng ta đã hiểu cách các thực thể tìm thấy nhau. Tuy nhiên, để "nói chuyện" được với nhau một cách chuyên nghiệp, chúng ta cần một ngôn ngữ chung. Bài nghiên cứu số 5 này sẽ tập trung hoàn toàn vào **HTTP (HyperText Transfer Protocol)** — "trái tim" của mọi giao tiếp mạng.

Chúng ta sẽ không học cách dùng các thư viện như `axios` hay `Spring Web` một cách hời hợt. Mục tiêu của bài này là **bóc tách lớp vỏ** để quan sát luồng dữ liệu thô chạy qua Socket, nghiên cứu cách Java xử lý từng byte dữ liệu từ JavaScript gửi tới.

---

### 1. Phân tích thực thể: Cấu trúc văn bản của giao thức HTTP

HTTP thực chất là một giao thức dựa trên văn bản (Text-based). Mọi "ý đồ" của JavaScript đều được đóng gói thành các dòng chữ cái gửi qua đường ống TCP.

#### 1.1. Cấu trúc Request từ JavaScript
Khi JavaScript thực thi một yêu cầu, nó tạo ra một cấu trúc gồm 3 phần chính mà Java Server phải bóc tách:
1.  **Request Line**: Chứa phương thức (GET, POST,...) và URI.
2.  **Headers**: Chứa Metadata (thông tin về thiết bị, kiểu dữ liệu, token bảo mật).
3.  **Entity Body**: Dữ liệu thực tế (thường là JSON).



#### 1.2. Cơ chế xử lý của Java (The Byte Processor)
Tại tầng Java, hệ thống không nhận được một "đối tượng" ngay lập tức. Nó nhận được một **InputStream**. Kỹ sư Java phải đọc từng byte, chuyển về