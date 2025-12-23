Tối ưu hóa hệ thống cấp phát IP với DHCP Server
---
layout: post
title: "Bài 4: Triển khai dịch vụ DHCP và các giải pháp bảo mật"
---

### Mục lục
1. [Quy trình DORA](#1-quy-trình-dora)
2. [Bảo mật DHCP Snooping](#2-bảo-mật-dhcp-snooping)

## 1. Quy trình DORA
Tiến trình cấp phát IP diễn ra qua 4 bước: Discover, Offer, Request và Acknowledge.

## 2. Bảo mật DHCP Snooping
```bash
Switch(config)# ip dhcp snooping
Switch(config-if)# ip dhcp snooping trust