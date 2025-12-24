---
layout: page
title: "Blog"
permalink: /blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="display: flex; flex-direction: column; gap: 25px;">
    {% assign posts = site.posts | sort: "weight" %}
    {% for post in posts %}
    <div style="border: 1px solid #e1e8ed; border-radius: 12px; padding: 20px; transition: 0.3s; background: #fff;" onmouseover="this.style.boxShadow='0 5px 15px rgba(0,0,0,0.1)'" onmouseout="this.style.boxShadow='none'">
      <span style="color: #007bff; font-weight: bold; font-size: 0.85em; text-transform: uppercase;">{{ post.date | date: "%b %d, %Y" }}</span>
      <h3 style="margin: 10px 0;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #1a202c;">{{ post.title }}</a>
      </h3>
      <p style="color: #4a5568; font-size: 0.95em;">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
      <a href="{{ post.url | relative_url }}" style="color: #007bff; font-weight: 600; text-decoration: none; font-size: 0.9em;">Đọc thêm →</a>
    </div>
    {% endfor %}
  </div>
</div>