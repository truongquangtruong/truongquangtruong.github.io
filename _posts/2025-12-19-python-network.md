Tự động hóa mạng với Python và Netmiko
----

### Bài 5: Network Automation (`2025-12-19-python-network.md`)
```markdown
---
layout: post
title: "Bài 5: Tự động hóa hạ tầng mạng bằng Python và Netmiko"
---

### Mục lục
1. [Tại sao cần Automation?](#1-tại-sao-cần-automation)
2. [Code mẫu Netmiko](#2-code-mẫu-netmiko)

## 1. Tại sao cần Automation?
Giúp triển khai cấu hình hàng loạt, giảm thiểu lỗi con người và tiết kiệm thời gian.

## 2. Code mẫu Netmiko
```python
from netmiko import ConnectHandler
# Script kết nối và lấy thông tin thiết bị
