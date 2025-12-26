---
title: "Nghiên cứu Hệ thống định danh DNS - Cơ chế phân giải tên miền trong hạ tầng mạng Java"
date: 2025-12-18
tags: ["java", "dns", "network-infrastructure", "ip-address", "backend-research"]
categories: ["Java Network Research"]
description: "Giải mã cơ chế phân giải tên miền DNS và cách triển khai các thực nghiệm Fullstack giúp tối ưu hóa kết nối giữa Client và Server."
draft: false
---

![Nghiên cứu hệ thống DNS](https://ducmanh.vn/wp-content/uploads/2021/08/dns-la-gi.jpg)

---

### 📍 Mục lục nội dung
* [1. Bản chất của DNS: "Danh bạ" toàn cầu](#phan-tich-1)
* [2. Cơ chế phân giải thực thể trong Java](#phan-tich-2)
* [3. Triển khai mã nguồn DNS Research Tool](#phan-tich-3)

---

Chào các bạn! Ở các bài trước, chúng ta đã kết nối thông qua địa chỉ IP thô (như `127.0.0.1`). Tuy nhiên, con người không thể nhớ hàng tỷ dãy số đó. Đó là lý do bài nghiên cứu số 4 này tập trung vào **DNS (Domain Name System)** — hệ thống giúp biến những cái tên dễ nhớ thành địa chỉ IP mà Java Socket có thể hiểu được.

<h3 id="phan-tich-1">1. Bản chất của DNS: "Danh bạ" toàn cầu</h3>
DNS không chỉ là một máy chủ, nó là một hệ thống phân tán phân cấp. Khi bạn gõ `google.com`, yêu cầu sẽ đi qua các máy chủ Root và TLD để tìm ra địa chỉ IP cuối cùng.



<h3 id="phan-tich-2">2. Cơ chế phân giải thực thể trong Java</h3>
Trong Java, chúng ta sử dụng lớp `InetAddress`. Đây là thực thể cầu nối, nó sẽ gọi các hàm hệ thống (System Calls) để hỏi DNS Server và trả về kết quả.



<h3 id="phan-tich-3">3. Triển khai mã nguồn DNS Research Tool</h3>

```java
import java.net.InetAddress;
import java.net.UnknownHostException;
import java.util.Scanner;

/**
 * DNS Research & Analysis Tool
 * Tác giả: Trương Quang Trường
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
                InetAddress address = InetAddress.getByName(domain);
                System.out.println("--- KẾT QUẢ PHÂN TÍCH ---");
                System.out.println("[+] Host Name: " + address.getHostName());
                System.out.println("[+] IP Address: " + address.getHostAddress());
                System.out.println("[+] Trạng thái: " + (address.isReachable(3000) ? "ONLINE" : "OFFLINE"));
            } catch (UnknownHostException e) {
                System.err.println("[ERR] Khong tim thay: " + domain);
            } catch (Exception e) {
                System.err.println("[ERR] Loi: " + e.getMessage());
            }
        }
        scanner.close();
    }
}
