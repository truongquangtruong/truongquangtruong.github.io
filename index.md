---
layout: home
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
    <p>"Nghiên cứu triển khai hạ tầng mạng đa tầng, tập trung tối ưu hóa các giao thức định tuyến OSPF và BGP. Đi sâu vào quản trị hệ thống Linux, tối ưu nhân Kernel và quản lý băng thông nhằm đảm bảo sự ổn định tuyệt đối cho các dịch vụ Backend Java, tạo nền tảng vững chắc cho hệ thống chịu tải cao.</p>
  </div>
  <div style="padding: 20px; border: 1px solid #e2e8f0; border-radius: 12px; background: #fff; box-shadow: 0 4px 6px rgba(0,0,0,0.02);">
    <h3 style="color: #007bff;">🛡️ Cyber Security</h3>
    <p>Thiết lập lá chắn bảo mật đa tầng qua xác thực JWT và kiểm soát luồng dữ liệu Fullstack. Tập trung ngăn chặn các véc-tơ tấn công XSS, CSRF và cấu hình CORS nghiêm ngặt nhằm bảo vệ toàn vẹn dữ liệu giữa Java - JavaScript, hướng tới môi trường an ninh chủ động và giám sát API toàn diện.</p>
  </div>
</div>

<h2 style="color: #1a202c; border-bottom: 2px solid #007bff; padding-bottom: 10px; margin-bottom: 30px;">
  📚 Lộ trình nghiên cứu (Bài #01 - Bài #09)
</h2>

<div id="post-container">
  {% for post in site.posts reversed %}
    <article class="post-item" style="margin-bottom: 15px; background: #fff; border: 1px solid #edf2f7; border-radius: 10px; transition: 0.3s;">
      <a href="{{ post.url }}" class="post-link" style="display: flex; justify-content: space-between; align-items: center; padding: 20px; text-decoration: none; color: inherit;">
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
// 1. Hàm lọc bài viết khi đang gõ (vẫn giữ lại để giao diện mượt)
function filterPosts() {
  let input = document.getElementById('search-input').value.toLowerCase();
  let posts = document.getElementsByClassName('post-item');

  for (let i = 0; i < posts.length; i++) {
    let title = posts[i].getElementsByClassName('post-title')[0].innerText.toLowerCase();
    posts[i].style.display = title.includes(input) ? "" : "none";
  }
}

// 2. Hàm "Nhảy" thẳng vào khóa học khi ấn nút Tìm
function jumpToPost() {
  let input = document.getElementById('search-input').value.toLowerCase().trim();
  let posts = document.getElementsByClassName('post-item');
  
  if (input === "") {
    alert("Trưởng ơi, hãy nhập hoặc paste tiêu đề bài viết nhé!");
    return;
  }

  for (let i = 0; i < posts.length; i++) {
    let titleElement = posts[i].getElementsByClassName('post-title')[0];
    let titleText = titleElement.innerText.toLowerCase();
    let postUrl = posts[i].getElementsByClassName('post-link')[0].getAttribute('href');

    // Nếu tiêu đề khớp hoàn toàn hoặc từ khóa nằm trong tiêu đề
    if (titleText.includes(input)) {
      window.location.href = postUrl; // Thực hiện "nhảy" trang
      return;
    }
  }
  
  alert("Không tìm thấy bài học nào khớp với tiêu đề này, Trưởng kiểm tra lại nhé!");
}

// Hỗ trợ ấn phím Enter để tìm kiếm cho nhanh
document.getElementById("search-input").addEventListener("keydown", function(event) {
  if (event.key === "Enter") {
    jumpToPost();
  }
});
</script>

<style>
  #search-input:focus { border-color: #007bff; background: #fff; }
  button:hover { background: #0056b3; transform: scale(1.05); }
  .post-item:hover { border-color: #007bff; transform: translateX(10px); box-shadow: 0 4px 12px rgba(0,123,255,0.1); }
</style>