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
    font-size: clamp(2.5rem, 6vw, 3.5rem);
    line-height: 1.2;
    margin: 0 0 20px 0;
    letter-spacing: -1px;
    white-space: nowrap;
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
    text-align: justify;
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
      font-size: 2.5rem;
      white-space: normal;
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

  /* Personalized CV Buttons */
  .cv-button-container {
    margin-top: 40px;
    display: flex;
    gap: 16px;
    flex-wrap: wrap;
  }

  .btn-cv {
    display: inline-flex;
    align-items: center;
    gap: 10px;
    padding: 13px 26px;
    border-radius: 12px;
    font-weight: 700;
    font-size: 0.95rem;
    text-decoration: none !important;
    transition: var(--transition-smooth);
    cursor: pointer;
    border: none;
  }

  .btn-cv-primary {
    background: linear-gradient(135deg, var(--primary-color), #0056b3);
    color: white !important;
    box-shadow: 0 8px 20px -6px rgba(0, 123, 255, 0.3);
  }

  .btn-cv-primary:hover {
    transform: translateY(-3px);
    box-shadow: 0 12px 25px -6px rgba(0, 123, 255, 0.4);
    filter: brightness(1.05);
  }

  .btn-cv-outline {
    background: white;
    color: var(--primary-color) !important;
    border: 2px solid var(--primary-color);
    box-sizing: border-box;
  }

  .btn-cv-outline:hover {
    background: var(--bg-accent);
    transform: translateY(-3px);
    box-shadow: 0 8px 15px -5px rgba(0, 0, 0, 0.08);
  }

  /* Certificates Section */
  .cert-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 30px;
    margin-top: 40px;
  }

  .cert-card {
    background: white;
    border-radius: 20px;
    overflow: hidden;
    border: 1px solid var(--border-soft);
    transition: var(--transition-smooth);
    display: flex;
    flex-direction: column;
  }

  .cert-card:hover {
    transform: translateY(-8px);
    box-shadow: var(--shadow-premium);
    border-color: var(--primary-color);
  }

  .cert-img-container {
    width: 100%;
    aspect-ratio: 4/3;
    overflow: hidden;
    background: #f1f5f9;
    position: relative;
    cursor: zoom-in;
  }

  .cert-img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: var(--transition-smooth);
  }

  .cert-card:hover .cert-img {
    transform: scale(1.05);
  }

  .cert-content {
    padding: 25px;
    flex-grow: 1;
    display: flex;
    flex-direction: column;
  }

  .cert-issuer {
    font-size: 0.75rem;
    font-weight: 700;
    color: var(--primary-color);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 8px;
  }

  .cert-title {
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 12px;
    line-height: 1.4;
    color: var(--text-main);
  }

  .cert-date {
    margin-top: auto;
    font-size: 0.85rem;
    color: var(--text-muted);
    display: flex;
    align-items: center;
    gap: 6px;
  }

  @media (max-width: 600px) {
    .cert-grid {
      grid-template-columns: 1fr;
    }
  }
</style>

<div class="profile-wrapper">
  
  <!-- Hero Section -->
  <section class="profile-hero">
    <div class="profile-photo-container">
      <img src="{{ '/assets/me.png?v=3' | relative_url }}" alt="Trương Quang Trường" class="profile-img-premium">
    </div>
    
    <div class="profile-info">
      <span class="profile-tagline">Hành trình học tập & Nghiên cứu</span>
      <h1 class="profile-name-title">Trương Quang Trường</h1>
      <div class="profile-role-line">Sinh viên năm 4 | An toàn thông tin</div>
      <div class="profile-specialty">Trường Đại học Công nghệ TP.HCM (HUTECH)</div>
      
      <p class="profile-bio-text">
        Chào bạn, mình là Trương Quang Trường. Tại HUTECH, mình không chỉ học cách bảo mật hệ thống, mà còn đam mê việc xây dựng chúng từ những viên gạch đầu tiên. 
        Blog này chính là "phòng thí nghiệm" cá nhân của mình — nơi chuỗi 09 bài nghiên cứu về lập trình mạng được thành hình. 
        Từ việc làm chủ luồng dữ liệu với <strong>Java</strong> đến sự linh hoạt của <strong>JavaScript</strong>, mình tin rằng việc hiểu rõ cách hệ thống vận hành chính là nền tảng vững chắc nhất để bảo vệ thế giới số.
      </p>
      
      <div class="tech-badges-group">
        <span class="tech-badge">Hạ tầng mạng</span>
        <span class="tech-badge">Lập trình hệ thống</span>
        <span class="tech-badge">Nghiên cứu Java</span>
        <span class="tech-badge">Học tập & Chia sẻ</span>
      </div>

      <div class="cv-button-container">
        <a href="{{ '/assets/cv-ca-nhan-tqt.pdf' | relative_url }}" target="_blank" class="btn-cv btn-cv-primary">
          <span>👁️</span> Xem CV Online
        </a>
        <a href="{{ '/assets/cv-ca-nhan-tqt.pdf' | relative_url }}" download class="btn-cv btn-cv-outline">
          <span>📥</span> Tải CV (PDF)
        </a>
      </div>
    </div>
  </section>

  <hr class="section-divider">

  <!-- Story Section -->
  <section class="story-container">
    <h2 class="content-section-title">Câu chuyện nghiên cứu & học tập</h2>
    <p class="story-paragraph">
      Chào bạn! Tôi là Trưởng. Blog này không chỉ là một trang web, mà là cuốn nhật ký ghi lại hành trình tôi tự phá vỡ giới hạn của chính mình. Xuất phát điểm là một sinh viên chuyên về mạng (Network), tôi luôn tự hỏi: <em>"Làm sao để dữ liệu đi từ Server Java đến trình duyệt JavaScript một cách an toàn và nhanh nhất?"</em>. 
    </p>
    <p class="story-paragraph">
      Câu hỏi đó đã dẫn dắt tôi đi qua <strong>09 bài nghiên cứu thực nghiệm</strong>. Tôi đã dành hàng giờ để nghiên cứu cách Java "đóng gói" thực thể qua Serialization, cách thiết lập kênh truyền thời gian thực bằng WebSockets, và cách xây dựng những lá chắn bảo mật kiên cố để bảo vệ dữ liệu người dùng.
    </p>
  </section>

  <!-- Focus Section -->
  <section>
    <h2 class="content-section-title">Mục tiêu phát triển</h2>
    <div class="focus-grid">
      <div class="focus-card">
        <h4>🛠 Tìm hiểu Kỹ thuật</h4>
        <p>
          Tập trung vào bản chất của các giao thức truyền tải. Tôi đang nỗ lực nắm vững cách dữ liệu di chuyển trong các hệ thống hiện đại để tối ưu hóa khả năng bảo mật từ những bước cơ bản nhất.
        </p>
      </div>
      
      <div class="focus-card">
        <h4>🚀 Định hướng Security</h4>
        <p>
          Trong tương lai, tôi mong muốn trở thành một kỹ sư có khả năng vừa thiết kế hạ tầng mạng an toàn, vừa phát triển Backend Java tối ưu và hiểu rõ quy trình vận hành hệ thống.
        </p>
      </div>
    </div>
  </section>

  <hr class="section-divider">

  <!-- Certificates Section -->
  <section>
    <h2 class="content-section-title">Chứng chỉ & Thành tựu chuyên môn</h2>
    <div class="cert-grid">
      <!-- Cert 1 -->
      <div class="cert-card">
        <div class="cert-img-container" onclick="window.open('{{ '/assets/cert-networking-basics.png' | relative_url }}', '_blank')">
          <img src="{{ '/assets/cert-networking-basics.png' | relative_url }}" alt="Networking Basics" class="cert-img">
        </div>
        <div class="cert-content">
          <span class="cert-issuer">Cisco Networking Academy</span>
          <h3 class="cert-title">Networking Basics</h3>
          <div class="cert-date"><span>📅</span> Nov 24, 2025</div>
        </div>
      </div>

      <!-- Cert 2 -->
      <div class="cert-card">
        <div class="cert-img-container" onclick="window.open('{{ '/assets/cert-js-essentials-1.png' | relative_url }}', '_blank')">
          <img src="{{ '/assets/cert-js-essentials-1.png' | relative_url }}" alt="JavaScript Essentials 1" class="cert-img">
        </div>
        <div class="cert-content">
          <span class="cert-issuer">Cisco / OpenEDG</span>
          <h3 class="cert-title">JavaScript Essentials 1</h3>
          <div class="cert-date"><span>📅</span> Nov 25, 2025</div>
        </div>
      </div>

      <!-- Cert 3 -->
      <div class="cert-card">
        <div class="cert-img-container" onclick="window.open('{{ '/assets/cert-js-essentials-2.png' | relative_url }}', '_blank')">
          <img src="{{ '/assets/cert-js-essentials-2.png' | relative_url }}" alt="JavaScript Essentials 2" class="cert-img">
        </div>
        <div class="cert-content">
          <span class="cert-issuer">Cisco / OpenEDG</span>
          <h3 class="cert-title">JavaScript Essentials 2</h3>
          <div class="cert-date"><span>📅</span> Dec 08, 2025</div>
        </div>
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
