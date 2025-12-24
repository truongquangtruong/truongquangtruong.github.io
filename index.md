---
layout: default
title: Trang Chủ
---

<div style="text-align: center; padding: 40px 0; background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%); border-radius: 20px; margin-bottom: 40px;">
  <h1 style="font-size: 2.5em; color: #1a202c; margin-bottom: 10px;">Chào mừng bạn đến với khóa học của tôi</h1>
  <p style="font-size: 1.2em; color: #4a5568;">Tôi là <strong>Trương Quang Trường</strong> – Chuyên ngành an ninh mạng </p>
  
  <div style="margin-top: 30px; display: flex; justify-content: center; gap: 10px; max-width: 500px; margin-left: auto; margin-right: auto;">
    <input type="text" id="search-input" placeholder="Tìm kiếm bài viết" 
      style="flex: 1; padding: 12px 20px; border: 2px solid #fff; border-radius: 8px; outline: none; font-size: 16px; box-shadow: 0 4px 6px rgba(0,0,0,0.05);"
      onkeyup="filterPosts()">
    <button onclick="jumpToPost()" style="padding: 12px 25px; background: #007bff; color: white; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; transition: 0.3s;">
      Tìm kiếm
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
    <p>Nghiên cứu triển khai hạ tầng mạng đa tầng, tập trung tối ưu hóa các giao thức định tuyến OSPF và BGP. Đi sâu vào quản trị hệ thống Linux, tối ưu nhân Kernel.</p>
  </div>
  <div style="padding: 20px; border: 1px solid #e2e8f0; border-radius: 12px; background: #fff; box-shadow: 0 4px 6px rgba(0,0,0,0.02);">
    <h3 style="color: #007bff;">🛡️ Cyber Security</h3>
    <p>Thiết lập lá chắn bảo mật đa tầng qua xác thực JWT và kiểm soát luồng dữ liệu Fullstack. Ngăn chặn các véc-tơ tấn công XSS, CSRF và cấu hình CORS nghiêm ngặt.</p>
  </div>
</div>

<h2 style="color: #1a202c; border-bottom: 2px solid #007bff; padding-bottom: 10px; margin-bottom: 30px;">
  📚 Lộ trình nghiên cứu (Bài #01 - Bài #09)
</h2>

<div id="post-container">
  {% comment %} 
    LẤY BÀI VIẾT THEO THỨ TỰ TỪ 1 ĐẾN 9
    Lưu ý: Phải có dòng 'weight' trong mỗi bài viết để code này chạy đúng.
  {% endcomment %}
  {% assign sorted_posts = site.posts | sort: "weight" %}
  {% for post in sorted_posts %}
    <article class="post-item" style="margin-bottom: 15px; background: #fff; border: 1px solid #edf2f7; border-radius: 10px; transition: 0.3s;">
      <a href="{{ post.url | relative_url }}" class="post-link" style="display: flex; justify-content: space-between; align-items: center; padding: 20px; text-decoration: none; color: inherit;">
        <div>
          <h4 class="post-title" style="margin: 0; color: #2d3748; font-size: 1.1em;">{{ post.title }}</h4>
          <small style="color: #a0aec0;">Ngày đăng: {{ post.date | date: "%d/%m/%Y" }}</small>
        </div>
        <span style="color: #007bff; font-weight: bold;">Đọc tiếp →</span>
      </a>
    </article>
  {% endfor %}
</div>

<script>
function filterPosts() {
  let input = document.getElementById('search-input').value.toLowerCase();
  let posts = document.getElementsByClassName('post-item');
  for (let i = 0; i < posts.length; i++) {
    let title = posts[i].getElementsByClassName('post-title')[0].innerText.toLowerCase();
    posts[i].style.display = title.includes(input) ? "" : "none";
  }
}

function jumpToPost() {
  let input = document.getElementById('search-input').value.toLowerCase().trim();
  let posts = document.getElementsByClassName('post-item');
  if (input === "") return;
  for (let i = 0; i < posts.length; i++) {
    let titleText = posts[i].getElementsByClassName('post-title')[0].innerText.toLowerCase();
    let postUrl = posts[i].getElementsByClassName('post-link')[0].getAttribute('href');
    if (titleText.includes(input)) {
      window.location.href = postUrl;
      return;
    }
  }
}
</script>

<style>
  .post-item:hover { border-color: #007bff; transform: translateX(10px); box-shadow: 0 4px 12px rgba(0,123,255,0.1); }
</style>