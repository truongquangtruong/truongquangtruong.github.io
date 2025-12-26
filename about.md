---
layout: page
title: "Profile"
permalink: /about/
---

<style>
  :root {
    --primary-color: #007bff;
    --primary-hover: #0056b3;
    --text-main: #1e293b;
    --text-muted: #64748b;
    --border-color: #e2e8f0;
    --shadow-md: 0 10px 30px rgba(0,0,0,0.08);
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .profile-container {
    padding: 60px 0;
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  }

  .profile-hero-wrapper {
    display: flex;
    gap: 60px;
    align-items: flex-start;
    margin-bottom: 50px;
  }

  .profile-column-left {
    flex-shrink: 0;
  }

  .profile-img-rect {
    width: 300px;
    height: 400px;
    border-radius: 20px;
    object-fit: cover;
    display: block;
    box-shadow: var(--shadow-md);
    transition: var(--transition);
  }

  .profile-img-rect:hover {
    transform: translateY(-5px);
    box-shadow: 0 20px 40px rgba(0,0,0,0.12);
  }

  .profile-column-right {
    flex: 1;
    padding-top: 5px;
  }

  .profile-intro-tag {
    color: var(--primary-color);
    font-size: 0.85rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 12px;
    display: block;
  }

  .profile-main-name {
    font-size: 3.5rem;
    font-weight: 800;
    margin: 0 0 10px 0;
    line-height: 1;
    color: #111;
    letter-spacing: -2px;
    text-transform: uppercase;
  }

  .profile-role-sub {
    font-size: 1.2rem;
    color: var(--text-main);
    margin-bottom: 15px;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .profile-role-sub span {
    color: var(--text-muted);
  }

  .profile-major-highlight {
    font-size: 1.4rem;
    font-weight: 600;
    margin-bottom: 20px;
    color: #334155;
  }

  .profile-short-bio {
    font-size: 1.05rem;
    color: var(--text-muted);
    line-height: 1.8;
    margin-bottom: 25px;
  }

  .profile-badges {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }

  .badge-item {
    background: #f8fafc;
    color: #475569;
    padding: 6px 16px;
    border-radius: 50px;
    font-size: 0.85rem;
    font-weight: 600;
    border: 1px solid var(--border-color);
  }

  @media (max-width: 850px) {
    .profile-hero-wrapper {
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 30px;
    }
    .profile-main-name {
      font-size: 2.5rem;
    }
    .profile-role-sub {
      justify-content: center;
    }
    .profile-badges {
      justify-content: center;
    }
    .profile-img-rect {
      width: 260px;
      height: 350px;
    }
  }
</style>

<div class="profile-container">
  <div class="profile-hero-wrapper">
    <div class="profile-column-left">
      <img src="{{ '/assets/me.png' | relative_url }}" alt="Trương Quang Trường" class="profile-img-rect">
    </div>
    
    <div class="profile-column-right">
      <span class="profile-intro-tag">Tôi là</span>
      <h1 class="profile-main-name">Trương Quang Trường</h1>
      
      <div class="profile-role-sub">
        Sinh viên An ninh mạng <span>|</span> Nhà nghiên cứu Fullstack Security
      </div>

      <div class="profile-major-highlight">
        Chuyên ngành Hệ thống phân tán
      </div>

      <p class="profile-short-bio">
        Chào bạn! Tôi là Trưởng, hiện đang theo đuổi sự chuyên nghiệp trong lĩnh vực bảo mật hệ thống và hạ tầng mạng.
        Hành trình của tôi là sự kết hợp giữa tư duy phân tích của một chuyên gia an ninh và sự linh hoạt của một nhà nghiên cứu Fullstack.
      </p>

      <div class="profile-badges">
        <span class="badge-item">Hạ tầng mạng</span>
        <span class="badge-item">Lập trình hệ thống</span>
        <span class="badge-item">Bảo mật dữ liệu</span>
      </div>
    </div>
  </div>
</div>

<hr style="border: 0; height: 1px; background: #edf2f7; margin: 40px 0;">

<div style="max-width: 800px; margin: 0 auto; line-height: 1.8; color: #2d3748;">
  
  <h2 style="color: #2b6cb0; border-left: 5px solid #2b6cb0; padding-left: 15px; margin-bottom: 20px;">Câu chuyện nghiên cứu của tôi</h2>
  <p style="text-align: justify;">
    Chào bạn! Tôi là Trưởng. Blog này không chỉ là một trang web, mà là cuốn nhật ký ghi lại hành trình tôi tự phá vỡ giới hạn của chính mình. Xuất phát điểm là một sinh viên chuyên về mạng (Network), tôi luôn tự hỏi: <em>"Làm sao để dữ liệu đi từ Server Java đến trình duyệt JavaScript một cách an toàn và nhanh nhất?"</em>. 
  </p>
  <p style="text-align: justify;">
    Câu hỏi đó đã dẫn dắt tôi đi qua <strong>09 bài nghiên cứu thực nghiệm</strong>. Tôi đã dành hàng giờ để nghiên cứu cách Java "đóng gói" thực thể qua Serialization, cách thiết lập kênh truyền thời gian thực bằng WebSockets, và cách xây dựng những lá chắn bảo mật kiên cố để bảo vệ dữ liệu người dùng.
  </p>

  <h2 style="color: #2b6cb0; border-left: 5px solid #2b6cb0; padding-left: 15px; margin-top: 40px; margin-bottom: 20px;">Những thứ tôi đang tập trung phát triển</h2>
  
  <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px;">
    <div style="background: #fff; padding: 20px; border-radius: 12px; border: 1px solid #e2e8f0;">
      <h4 style="color: #2b6cb0; margin-top: 0;">🛠 Nghiên cứu Kỹ thuật</h4>
      <p style="font-size: 0.95em; margin-bottom: 0;">
        Tôi đang đi sâu vào việc hiểu rõ <strong>bản chất của giao thức</strong>. Không chỉ dừng lại ở việc dùng thư viện, tôi muốn biết từng byte dữ liệu di chuyển như thế nào trong mô hình Microservices và API Gateway mà tôi đã xây dựng.
      </p>
    </div>
    
    <div style="background: #fff; padding: 20px; border-radius: 12px; border: 1px solid #e2e8f0;">
      <h4 style="color: #2b6cb0; margin-top: 0;">🚀 Định hướng tương lai</h4>
      <p style="font-size: 0.95em; margin-bottom: 0;">
        Tôi muốn trở thành một <strong>Fullstack Security Engineer</strong>. Đó là người có thể vừa thiết kế hạ tầng mạng an toàn, vừa có thể viết code Backend Java tối ưu và quản lý được toàn bộ quy trình vận hành trên Cloud.
      </p>
    </div>
  </div>

  <div style="background: #2d3748; color: white; padding: 30px; border-radius: 15px; margin-top: 40px; text-align: center;">
    <h3 style="margin-top: 0;">Bạn có cùng đam mê nghiên cứu?</h3>
    <p>Tôi luôn trân trọng mọi sự kết nối để cùng nhau trao đổi về kỹ thuật.</p>
    <a href="mailto:truongblueblack0702@email.com" style="color: #63b3ed; text-decoration: none; font-weight: bold; font-size: 1.1em;">
      truongblueblack0702@email.com
    </a>
  </div>
</div>