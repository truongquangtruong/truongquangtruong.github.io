---
title: "Khóa Học #06: Nghiên cứu Serialization - Nghệ thuật chuyển hóa thực thể dữ liệu liên tầng Java và JavaScript"
date: 2025-12-20
tags: ["java", "javascript", "json", "serialization", "data-binding", "fullstack-research", "architecture"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu đồng bộ hóa dữ liệu JSON](https://gcalls.co/wp-content/uploads/2023/03/2023-03-24_0005.png)

<div style="background: #f8fafc; border: 1px solid #e2e8f0; border-radius: 10px; padding: 20px; margin-bottom: 30px; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <h4 style="margin-top: 0; color: #007bff; display: flex; align-items: center;">
    <span style="margin-right: 10px;">📍</span> Mục lục nội dung
  </h4>
  <div style="color: #2d3748; line-height: 1.6;">

* TOC
{:toc}

  </div>
</div>

Chào các bạn! Sau khi đã làm chủ giao thức truyền tải HTTP ở Bài 5, chúng ta đối mặt với một thách thức mang tính bản sắc ngôn ngữ: **Làm sao để hai thực thể không cùng hệ tư tưởng hiểu được nhau?** Java là ngôn ngữ định kiểu mạnh (Strongly Typed), nơi mọi thứ phải nằm trong các Class nghiêm ngặt. JavaScript lại là ngôn ngữ định kiểu động (Dynamic Typing), nơi sự linh hoạt được ưu tiên hàng đầu. Để "hòa giải" sự khác biệt này, chúng ta cần một ngôn ngữ trung gian. Bài nghiên cứu số 6 này sẽ tập trung vào **JSON (JavaScript Object Notation)** và cơ chế **Serialization (Tuần tự hóa)** — quá trình biến đổi cấu trúc dữ liệu từ bộ nhớ RAM thành dòng văn bản xuyên lục địa.

---

### 1. Phân tích thực thể: Bản chất của Serialization và Deserialization

Trong nghiên cứu hệ thống phân tán, dữ liệu không thể di chuyển dưới dạng các "đối tượng sống" (Live Objects). Chúng phải được "đóng băng" lại.



#### 1.1. Serialization: Quá trình "Đóng gói" (Java Side)
Tại thực thể Java, Serialization là hành động quét qua cấu trúc vùng nhớ (Heap), trích xuất các giá trị thuộc tính và sắp xếp chúng theo cú pháp JSON (RFC 8259). Đây không chỉ là việc nối chuỗi, mà là việc đảm bảo tính đúng đắn của các kiểu dữ liệu phức tạp.

#### 1.2. Deserialization: Quá trình "Tái sinh" (JavaScript Side)
Khi chuỗi JSON đến trình duyệt, JavaScript thực hiện quá trình ngược lại. Nó phân tích cú pháp văn bản và ánh xạ vào các thuộc tính của một Object mới. Vì JSON vốn là tập con của JavaScript, nên việc "tái sinh" dữ liệu diễn ra với hiệu suất cực cao.

---

### 2. Nghiên cứu thực nghiệm: Xây dựng Bộ chuyển đổi dữ liệu thô (Raw Mapping)

Để thực sự hiểu cách dữ liệu bị biến đổi, chúng ta sẽ thực hiện nghiên cứu bằng cách tự tay xây dựng một bộ máy Serialization thủ công trong Java, không sử dụng bất kỳ thư viện bên thứ ba nào như Jackson hay