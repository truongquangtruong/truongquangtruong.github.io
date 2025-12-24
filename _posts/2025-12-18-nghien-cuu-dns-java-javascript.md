---
title: "Khóa Học #04: Nghiên cứu Hệ thống định danh DNS - Cơ chế phân giải tên miền trong hạ tầng mạng Java"
date: 2025-12-18
tags: ["java", "dns", "network-infrastructure", "ip-address", "backend-research"]
categories: ["Java Network Research"]
draft: false
---

![Nghiên cứu hệ thống DNS](https://vhost.vn/wp-content/uploads/2023/11/DNS-la-gi.png)

---

### 📍 Mục lục nội dung
1. [Bản chất của DNS: "Danh bạ" toàn cầu](#1-bản-chất-của-dns-danh-bạ-toàn-cầu)
2. [Cơ chế phân giải thực thể trong Java](#2-cơ-chế-phân-giải-thực-thể-trong-java)
3. [Triển khai mã nguồn DNS Research Tool](#3-triển-khai-mã-nguồn-dns-research-tool)

---

Chào các bạn! Ở các bài trước, chúng ta đã kết nối thông qua địa chỉ IP thô (như `127.0.0.1`). Tuy nhiên, con người không thể nhớ hàng tỷ dãy số đó. Đó là lý do bài nghiên cứu số 4 này tập trung vào **DNS (Domain Name System)** — hệ thống giúp biến những cái tên dễ nhớ thành địa chỉ IP mà Java Socket có thể hiểu được.

### 1. Bản chất của DNS: "Danh bạ" toàn cầu
DNS không chỉ là một máy chủ, nó là một hệ thống phân tán phân cấp. Khi bạn gõ `google.com`, yêu cầu sẽ đi qua:
* **Root NameServer**: Gốc của toàn bộ hệ thống.
* **TLD NameServer**: Quản lý các đuôi như `.com`, `.net`, `.vn`.
* **Authoritative NameServer**: Nơi lưu giữ chính xác địa chỉ IP của tên miền đó.

### 2. Cơ chế phân giải thực thể trong Java
Trong Java, chúng ta sử dụng lớp `InetAddress`. Đây là thực thể cầu nối, nó sẽ gọi các hàm hệ thống (System Calls) để hỏi DNS Server của nhà mạng (ISP) và trả về kết quả cho ứng dụng của chúng ta.



### 3. Triển khai mã nguồn DNS Research Tool
Dưới đây là công cụ nghiên cứu DNS mà mình đã viết để bóc tách thông tin từ bất kỳ tên miền nào.

```java
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.util.Scanner;

/**
 * DNS Research & Analysis Tool
 * Tác giả: Trương Quang Trưởng
 */
public class DnsResearchTool {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("=== HỆ THỐNG NGHIÊN CỨU ĐỊNH DANH [TRƯỞNG] ===");
        
        while (true) {
            System.out.print("\nNhập tên miền để phân tích (hoặc 'exit'): ");
            String domain = scanner.nextLine();

            if (domain.equalsIgnoreCase("exit")) break;

            try {
                // Phân giải từ Tên miền sang IP
                InetAddress address = InetAddress.getByName(domain);
                
                System.out.println("--- KẾT QUẢ PHÂN TÍCH ---");
                System.out.println("[+] Host Name: " + address.getHostName());
                System.out.println("[+] Canonical Host Name: " + address.getCanonicalHostName());
                System.out.println("[+] IP Address: " + address.getHostAddress());
                
                // Kiểm tra khả năng kết nối (Reachability)
                System.out.println("[+] Trạng thái: " + (address.isReachable(3000) ? "ĐANG HOẠT ĐỘNG" : "KHÔNG PHẢN HỒI"));

            } catch (UnknownHostException e) {
                System.err.println("[ERR] Không thể định danh tên miền: " + domain);
            } catch (Exception e) {
                System.err.println("[ERR] Lỗi hệ thống: " + e.getMessage());
            }
        }
        scanner.close();
    }
}
