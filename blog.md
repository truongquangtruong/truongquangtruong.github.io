---
layout: page
title: "Kỹ Thuật Mạng & Hệ Thống"
permalink: /blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px;">
    Nơi chia sẻ kiến thức, kinh nghiệm thực chiến về hạ tầng mạng, bảo mật và hệ thống phân tán.
  </p>

  <div style="display: flex; flex-direction: column; gap: 25px;">
    
    {% for post in site.posts %}
    <div style="border: 1px solid #e1e8ed; border-radius: 12px; padding: 20px; transition: 0.3s; background: #fff;" onmouseover="this.style.boxShadow='0 5px 15px rgba(0,0,0,0.1)'" onmouseout="this.style.boxShadow='none'">
      <span style="color: #007bff; font-weight: bold; font-size: 0.85em; text-transform: uppercase;">{{ post.date | date: "%b %d, %Y" }}</span>
      <h3 style="margin: 10px 0;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #1a202c;">{{ post.title }}</a>
      </h3>
      <p style="color: #4a5568; font-size: 0.95em;">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
      <a href="{{ post.url | relative_url }}" style="color: #007bff; font-weight: 600; text-decoration: none; font-size: 0.9em;">Đọc thêm →</a>
    </div>
    {% endfor %}

    {% if site.posts.size == 0 %}
    <div style="text-align: center; padding: 50px; background: #f8fafc; border-radius: 15px; border: 2px dashed #cbd5e0;">
      <p style="color: #718096; font-size: 1.1em;">Hiện tại chưa có bài viết nào. Hãy đón chờ những chia sẻ sắp tới về Network Engineering!</p>
    </div>
    {% endif %}

  </div>
</div>