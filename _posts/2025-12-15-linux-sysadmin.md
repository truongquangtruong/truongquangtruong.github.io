---
layout: post
title: "Bài 1: Làm chủ quản trị hệ thống Linux dành cho kỹ sư mạng"
date: 2025-12-15
categories: [System, Linux]
---

**Tóm tắt:** Bài viết cung cấp kiến thức nền tảng về Kernel, hệ thống tệp tin và các kỹ năng dòng lệnh thiết yếu để vận hành Server và thiết bị mạng.

### Mục lục
1. [Kiến trúc nhân Linux](#1-kiến-trúc-nhân-linux)
2. [Sơ đồ cây thư mục](#2-sơ-đồ-cây-thư-mục)
3. [Cấu hình hệ thống thực tế](#3-cấu-hình-hệ-thống-thực-tế)

## 1. Kiến trúc nhân Linux
Linux Kernel đóng vai trò trung gian điều phối tài nguyên giữa phần cứng và ứng dụng người dùng.

## 2. Sơ đồ cây thư mục
<div style="text-align: center; margin: 20px 0;">
  <img src="{{ '/assets/linux-structure.png' | relative_url }}" style="border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.1);">
  <p style="font-style: italic; color: #666;">Hình 1: Cấu trúc thư mục tiêu chuẩn của Linux (FHS)</p>
</div>

## 3. Cấu hình hệ thống thực tế
```bash
# Phân quyền tệp tin nâng cao
sudo chmod 755 /var/www/html
# Kiểm tra tài nguyên hệ thống theo thời gian thực
htop