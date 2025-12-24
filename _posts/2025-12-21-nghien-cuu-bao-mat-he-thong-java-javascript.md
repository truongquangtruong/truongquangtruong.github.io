---
title: "Khóa Học #07: Nghiên cứu Bảo mật Hệ thống - Thiết lập lá chắn đa tầng cho thực thể Java và JavaScript"
date: 2025-12-21
tags: ["java", "javascript", "security", "cors", "xss", "fullstack-research", "cyber-security"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu bảo mật dữ liệu mạng](https://405cyse.com/wp-content/uploads/2025/05/4cd10cf78793671c50d7e0a0d45bddddf3300bf7.gif)

---

### 📍 Mục lục nội dung
* [1. Phân tích thực thể: Các véc-tơ tấn công Fullstack](#1-phân-tích-thực-thể)
* [2. Các kiểu tấn công điển hình (XSS, CORS, MitM)](#2-các-kiểu-tấn-công)
* [3. Nghiên cứu thực nghiệm: Bộ lọc bảo mật Java](#3-nghiên-cứu-thực-nghiệm)

---

Chào các bạn! Trong hành trình nghiên cứu sự kết nối giữa Java và JavaScript, chúng ta cần một câu hỏi sống còn: **Làm sao để đảm bảo dữ liệu không bị đánh cắp?**

Bài nghiên cứu số 7 này sẽ đi sâu vào việc thiết lập các lớp "giáp trụ" bảo mật để bảo vệ sự tương tác giữa hai thực thể Java và JavaScript.

---

<a name="1-phân-tích-thực-thể"></a>
### 1. Phân tích thực thể: Các véc-tơ tấn công Fullstack điển hình

Qua quá trình đo lường, chúng ta xác định được 3 điểm yếu cốt lõi:


<a name="2-các-kiểu-tấn-công"></a>
#### 2. Tấn công XSS, CORS Misconfiguration và MitM

* **XSS (Cross-Site Scripting):** Kẻ tấn công lừa hệ thống lưu trữ mã độc JavaScript vào Database.
* **CORS Misconfiguration:** Java Server cấu hình `Allow-Origin: *` quá lỏng lẻo.
* **MitM (Man-in-the-Middle):** Dữ liệu JSON thô bị bắt gói tin nếu không có HTTPS.

---

<a name="3-nghiên-cứu-thực-nghiệm"></a>
### 3. Nghiên cứu thực nghiệm: Bộ lọc bảo mật Java (Security Sanitizer)

Chúng ta sẽ nghiên cứu cách "rửa sạch" mọi đầu vào từ JavaScript bằng Java.

```java
import java.util.regex.Pattern;

/**
 * Advanced Security Research Tool
 * Tác giả: Trương Quang Trưởng
 */
public class SecurityDataResearch {

    private static final Pattern[] XSS_PATTERNS = {
        Pattern.compile("<script>(.*?)</script>", Pattern.CASE_INSENSITIVE),
        Pattern.compile("javascript:", Pattern.CASE_INSENSITIVE)
    };

    public static String sanitize(String input) {
        if (input == null) return null;
        String cleanData = input;
        for (Pattern scriptPattern : XSS_PATTERNS) {
            cleanData = scriptPattern.matcher(cleanData).replaceAll("[SECURE_REMOVED]");
        }
        return cleanData.replace("'", "''");
    }

    public static void main(String[] args) {
        String maliciousPayload = "{\"comment\": \"<script>alert('hack')</script>\"}";
        System.out.println("=== HỆ THỐNG BẢO MẬT [TRƯỞNG] ===");
        System.out.println("[SAFE OUTPUT]: " + sanitize(maliciousPayload));
    }
}