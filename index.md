---
layout: home
title: Trang Chủ
---

<div style="text-align: center; padding: 40px 0; background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%); border-radius: 20px; margin-bottom: 40px;">
  <h1 style="font-size: 2.5em; color: #1a202c; margin-bottom: 10px;">Chào mừng bạn đến với khóa học của tôi</h1>
  <p style="font-size: 1.2em; color: #4a5568;">Tôi là <strong>Trương Quang Trường</strong> – Chuyên ngành an ninh mạng </p>
  
  <div style="margin-top: 30px; display: flex; justify-content: center; gap: 10px; max-width: 500px; margin-left: auto; margin-right: auto;">
    <input type="text" id="search-input" placeholder="Tìm kiếm bài viết..." 
      style="flex: 1; padding: 12px 20px; border: 2px solid #fff; border-radius: 8px; outline: none; font-size: 16px; box-shadow: 0 4px 6px rgba(0,0,0,0.05);">
    <button style="padding: 12px 25px; background: #007bff; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; transition: 0.3s;">
      Tìm
    </button>
  </div>

  <div style="margin-top: 25px;">
    <a href="/about/" style="background: #007bff; color: white; padding: 12px 25px; border-radius: 8px; text-decoration: none; font-weight: bold; margin-right: 15px; display: inline-block;">Giới thiệu</a>
    <a href="/contact/" style="background: white; color: #007bff; padding: 12px 25px; border-radius: 8px; text-decoration: none; font-weight: bold; border: 1px solid #007bff; display: inline-block;">Liên Hệ Ngay</a>
  </div>
</div>

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; margin-bottom: 50px;">
  <div style="padding: 20px; border: 1px solid #e2e8f0; border-radius: 12px; background: #fff; box-shadow: 0 4px 6px rgba(0,0,0,0.02);">
    <h3 style="color: #007bff;">🌐 Network Engineering</h3>
    <p>Chia sẻ các giải pháp về định tuyến (Routing), chuyển mạch (Switching) và tối ưu hóa hiệu năng hệ thống mạng doanh nghiệp.</p>
  </div>
  <div style="padding: 20px; border: 1px solid #e2e8f0; border-radius: 12px; background: #fff; box-shadow: 0 4px 6px rgba(0,0,0,0.02);">
    <h3 style="color: #007bff;">🛡️ Cyber Security</h3>
    <p>Nghiên cứu và triển khai các giải pháp bảo mật hạ tầng, ngăn chặn tấn công và quản lý lỗ hổng hệ thống.</p>
  </div>
</div>

<h2 style="color: #1a202c; border-bottom: 2px solid #007bff; padding-bottom: 10px; margin-bottom: 30px;">
  📚 Lộ trình nghiên cứu (Bài #01 - Bài #09)
</h2>

<div class="post-list">
  {% for post in site.posts reversed %}
    <article style="margin-bottom: 15px; background: #fff; border: 1px solid #edf2f7; border-radius: 10px; transition: 0.3s;" onmouseover="this.style.borderColor='#007bff'" onmouseout="this.style.borderColor='#edf2f7'">
      <a href="{{ post.url }}" style="display: flex; justify-content: space-between; align-items: center; padding: 20px; text-decoration: none; color: inherit;">
        <div>
          <h4 style="margin: 0; color: #2d3748; font-size: 1.1em;">{{ post.title }}</h4>
          <small style="color: #a0aec0;">Ngày đăng: {{ post.date | date: "%d/%m/%Y" }}</small>
        </div>
        <span style="color: #007bff; font-weight: bold;">Đọc tiếp →</span>
      </a>
    </article>
  {% endfor %}
</div>

<style>
  #search-input:focus { border-color: #007bff; background: #fff; }
  button:hover { background: #0056b3; transform: scale(1.05); }
  article:hover { transform: translateX(10px); box-shadow: 0 4px 12px rgba(0,123,255,0.1); }
</style>