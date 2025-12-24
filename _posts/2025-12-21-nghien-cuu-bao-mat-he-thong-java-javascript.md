---
title: "Khóa Học #07: Nghiên cứu Bảo mật Hệ thống - Thiết lập lá chắn đa tầng cho thực thể Java và JavaScript"
date: 2025-12-21
tags: ["java", "javascript", "security", "cors", "xss", "fullstack-research", "cyber-security"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu bảo mật dữ liệu mạng](https://405cyse.com/wp-content/uploads/2025/05/4cd10cf78793671c50d7e0a0d45bddddf3300bf7.gif)

<div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 10px; padding: 20px; margin-bottom: 30px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <h4 style="margin-top: 0; color: #007bff; display: flex; align-items: center;">
    <span style="margin-right: 10px;">📍</span> Mục lục nội dung
  </h4>
  <div style="color: #2d3748; line-height: 1.6;">

* TOC
{:toc}

  </div>
</div>

Chào các bạn! Trong hành trình nghiên cứu sự kết nối giữa Java và JavaScript, chúng ta đã xây dựng được những "con đường" truyền tải dữ liệu tuyệt vời qua HTTP và JSON. Tuy nhiên, một câu hỏi sống còn đặt ra trong môi trường thực tế: **Làm sao để đảm bảo dữ liệu đó không bị đánh cắp hoặc bị can thiệp trái phép?**

Trong mô hình Fullstack, thực thể JavaScript (Frontend) thường là nơi dễ bị tổn thương nhất do mã nguồn phơi bày trên trình duyệt. Ngược lại, Java (Backend) là thành trì cuối cùng nắm giữ logic nghiệp vụ và cơ sở dữ liệu. Bài nghiên cứu số 7 này sẽ đi sâu vào việc thiết lập các lớp "giáp trụ" bảo mật để bảo vệ sự tương tác giữa hai thực thể này.

---

### 1. Phân tích thực thể: Các véc-tơ tấn công Fullstack điển hình

Qua quá trình đo lường và thực nghiệm, chúng ta xác định được 3 điểm yếu cốt lõi trong mối quan hệ Java-JS:



#### 1.1. Tấn công XSS (Cross-Site Scripting)
Kẻ tấn công lừa hệ thống lưu trữ mã độc JavaScript vào Database thông qua Java. Khi người dùng hợp lệ yêu cầu dữ liệu, trình duyệt của họ sẽ thực thi mã độc này, dẫn đến mất Token xác thực hoặc lộ thông tin nhạy cảm.

#### 1.2. CORS Misconfiguration
CORS (Cross-Origin Resource Sharing) là một cơ chế bảo mật quan trọng. Nếu Java Server cấu hình `Allow-Origin: *` một cách lỏng lẻo, các JavaScript từ các tên miền độc hại khác có thể dễ dàng gọi API của bạn để trích xuất dữ liệu của người dùng.

#### 1.3. Tấn công Man-in-the-Middle (MitM)
Dữ liệu JSON thô đi qua đường ống Internet nếu không được mã hóa (HTTPS) sẽ trở thành "mồi ngon" cho các công cụ bắt gói tin, để