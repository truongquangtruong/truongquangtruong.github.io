---
layout: page
title: "Profile"
permalink: /about/
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;0,800;1,700&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>
  :root {
    --primary-color: #007bff;
    --primary-hover: #0056b3;
    --text-main: #111827;
    --text-muted: #4b5563;
    --bg-accent: #f8fafc;
    --border-soft: #f1f5f9;
    --shadow-premium: 0 25px 50px -12px rgba(0, 0, 0, 0.08);
    --transition-smooth: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .profile-wrapper {
    max-width: 1000px;
    margin: 0 auto;
    font-family: 'Inter', sans-serif;
    color: var(--text-main);
    padding: 40px 0;
  }

  /* Hero Section */
  .profile-hero {
    display: flex;
    gap: 80px;
    align-items: center;
    margin-bottom: 80px;
  }

  .profile-photo-container {
    flex-shrink: 0;
    position: relative;
  }

  .profile-photo-container::after {
    content: '';
    position: absolute;
    top: 20px;
    left: 20px;
    right: -20px;
    bottom: -20px;
    background: var(--bg-accent);
    border-radius: 20px;
    z-index: -1;
  }

  .profile-img-premium {
    width: 320px;
    height: 440px;
    border-radius: 20px;
    object-fit: cover;
    box-shadow: var(--shadow-premium);
    display: block;
    transition: var(--transition-smooth);
  }

  .profile-img-premium:hover {
    transform: scale(1.02);
  }

  .profile-info {
    flex: 1;
  }

  .profile-tagline {
    font-size: 0.9rem;
    font-weight: 700;
    color: var(--primary-color);
    text-transform: uppercase;
    letter-spacing: 0.2em;
    margin-bottom: 15px;
    display: block;
  }

  .profile-name-title {
    font-family: 'Playfair Display', serif;
    font-size: 4rem;
    line-height: 1.1;
    margin: 0 0 20px 0;
    letter-spacing: -2px;
  }

  .profile-role-line {
    font-size: 1.25rem;
    color: var(--text-main);
    margin-bottom: 10px;
    font-weight: 500;
  }

  .profile-specialty {
    font-size: 1.1rem;
    color: var(--text-muted);
    margin-bottom: 30px;
    display: flex;
    align-items: center;
    gap: 10px;
  }

  .profile-specialty::before {
    content: '';
    width: 30px;
    height: 2px;
    background: var(--primary-color);
  }

  .profile-bio-text {
    font-size: 1.15rem;
    line-height: 1.8;
    color: var(--text-muted);
    margin-bottom: 35px;
  }

  /* Badges */
  .tech-badges-group {
    display: flex;
    flex-wrap: wrap;
    gap: 12px;
  }

  .tech-badge {
    background: white;
    padding: 8px 20px;
    border-radius: 100px;
    font-size: 0.85rem;
    font-weight: 600;
    color: var(--text-main);
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05);
    border: 1px solid var(--border-soft);
    transition: var(--transition-smooth);
  }

  .tech-badge:hover {
    background: var(--primary-color);
    color: white;
    border-color: var(--primary-color);
    transform: translateY(-2px);
  }

  /* Content Sections */
  .section-divider {
    border: 0;
    height: 1px;
    background: linear-gradient(to right, transparent, var(--border-soft), transparent);
    margin: 80px 0;
  }

  .content-section-title {
    font-family: 'Playfair Display', serif;
    font-size: 2.2rem;
    margin-bottom: 30px;
    text-align: center;
  }

  .story-container {
    background: var(--bg-accent);
    padding: 50px;
    border-radius: 30px;
    margin-bottom: 60px;
  }

  .story-paragraph {
    font-size: 1.1rem;
    line-height: 1.9;
    color: var(--text-muted);
    margin-bottom: 25px;
    text-align: justify;
  }

  .focus-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
    margin-top: 40px;
  }

  .focus-card {
    background: white;
    padding: 40px;
    border-radius: 20px;
    border: 1px solid var(--border-soft);
    transition: var(--transition-smooth);
  }

  .focus-card:hover {
    box-shadow: var(--shadow-premium);
    transform: translateY(-5px);
  }

  .focus-card h4 {
    color: var(--primary-color);
    font-size: 1.3rem;
    margin-top: 0;
    margin-bottom: 15px;
    font-family: 'Playfair Display', serif;
  }

  .focus-card p {
    font-size: 1rem;
    color: var(--text-muted);
    line-height: 1.7;
    margin: 0;
  }

  /* Contact */
  .contact-island {
    margin-top: 100px;
    text-align: center;
    padding: 60px;
    background: #111827;
    color: white;
    border-radius: 40px;
  }

  .contact-island h3 {
    font-family: 'Playfair Display', serif;
    font-size: 2.5rem;
    margin-bottom: 15px;
    letter-spacing: -1px;
  }

  .contact-link {
    display: inline-block;
    color: #60a5fa;
    text-decoration: none;
    font-size: 1.3rem;
    font-weight: 600;
    margin-top: 20px;
    position: relative;
    padding-bottom: 5px;
  }

  .contact-link::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 0;
    width: 0;
    height: 2px;
    background: #60a5fa;
    transition: var(--transition-smooth);
  }

  .contact-link:hover::after {
    width: 100%;
  }

  @media (max-width: 900px) {
    .profile-hero {
      flex-direction: column;
      text-align: center;
      gap: 40px;
    }
    .profile-name-title {
      font-size: 3rem;
    }
    .profile-specialty {
      justify-content: center;
    }
    .tech-badges-group {
      justify-content: center;
    }
    .focus-grid {
      grid-template-columns: 1fr;
    }
    .profile-img-premium {
      width: 280px;
      height: 380px;
    }
  }
</style>

<div class="profile-wrapper">
  
  <!-- Hero Section -->
  <section class="profile-hero">
    <div class="profile-photo-container">
      <img src="{{ '/assets/me.png' | relative_url }}" alt="Trương Quang Trường" class="profile-img-premium">
    </div>
    
    <div class="profile-info">
      <span class="profile-tagline">Hành trình nghiên cứu</span>
      <h1 class="profile-name-title">Trương Quang Trường</h1>
      <div class="profile-role-line">Sinh viên An ninh mạng | Nhà nghiên cứu Fullstack Security</div>
      <div class="profile-specialty">Chuyên ngành Hệ thống phân tán</div>
      
      <p class="profile-bio-text">
        Tôi chuyên tâm vào việc xây dựng hạ tầng kỹ thuật số an toàn và hiệu năng cao. 
        Mọi dòng code tôi viết đều là một viên gạch trong hành trình thấu hiểu bản chất của các kênh truyền dữ liệu.
      </p>
      
      <div class="tech-badges-group">
        <span class="tech-badge">Hạ tầng mạng</span>
        <span class="tech-badge">Lập trình hệ thống</span>
        <span class="tech-badge">Bảo mật dữ liệu</span>
        <span class="tech-badge">Java Expert</span>
      </div>
    </div>
  </section>

  <hr class="section-divider">

  <!-- Story Section -->
  <section class="story-container">
    <h2 class="content-section-title">Câu chuyện nghiên cứu</h2>
    <p class="story-paragraph">
      Chào bạn! Tôi là Trưởng. Blog này không chỉ là một trang web, mà là cuốn nhật ký ghi lại hành trình tôi tự phá vỡ giới hạn của chính mình. Xuất phát điểm là một sinh viên chuyên về mạng (Network), tôi luôn tự hỏi: <em>"Làm sao để dữ liệu đi từ Server Java đến trình duyệt JavaScript một cách an toàn và nhanh nhất?"</em>. 
    </p>
    <p class="story-paragraph">
      Câu hỏi đó đã dẫn dắt tôi đi qua <strong>09 bài nghiên cứu thực nghiệm</strong>. Tôi đã dành hàng giờ để nghiên cứu cách Java "đóng gói" thực thể qua Serialization, cách thiết lập kênh truyền thời gian thực bằng WebSockets, và cách xây dựng những lá chắn bảo mật kiên cố để bảo vệ dữ liệu người dùng.
    </p>
  </section>

  <!-- Focus Section -->
  <section>
    <h2 class="content-section-title">Định hướng phát triển</h2>
    <div class="focus-grid">
      <div class="focus-card">
        <h4>🛠 Nghiên cứu Kỹ thuật</h4>
        <p>
          Đi sâu vào bản chất của các giao thức truyền tải. Tôi muốn nắm vững cách từng byte dữ liệu di chuyển trong các hệ thống Microservices hiện đại để tối ưu hóa khả năng bảo mật từ tầng thấp nhất.
        </p>
      </div>
      
      <div class="focus-card">
        <h4>🚀 Fullstack Security</h4>
        <p>
          Mục tiêu của tôi là trở thành một kỹ sư có khả năng vừa thiết kế hạ tầng mạng an toàn, vừa phát triển Backend Java tối ưu và quản lý quy trình vận hành trên Cloud một cách chuyên nghiệp.
        </p>
      </div>
    </div>
  </section>

  <!-- Contact Island -->
  <section class="contact-island">
    <h3>Kết nối nghiên cứu?</h3>
    <p>Tôi luôn trân trọng mọi cơ hội trao đổi về chiều sâu kỹ thuật và bảo mật hệ thống.</p>
    <a href="mailto:truongblueblack0702@email.com" class="contact-link">
      truongblueblack0702@email.com
    </a>
  </section>

</div>
