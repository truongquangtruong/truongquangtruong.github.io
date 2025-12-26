---
layout: page
title: "Liên Hệ"
permalink: /contact/
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;0,800&family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>
  :root {
    --primary-color: #007bff;
    --primary-hover: #0056b3;
    --text-main: #111827;
    --text-muted: #4b5563;
    --bg-accent: #f8fafc;
    --border-soft: #e2e8f0;
    --shadow-premium: 0 10px 25px -5px rgba(0, 0, 0, 0.05);
    --transition: all 0.3s ease;
  }

  .contact-wrapper {
    max-width: 900px;
    margin: 40px auto 80px;
    font-family: 'Inter', sans-serif;
    color: var(--text-main);
  }

  .contact-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 60px;
    padding: 20px 0;
  }

  /* Left Column: Form */
  .contact-form-side {
    flex: 1.5;
    min-width: 320px;
  }

  .contact-title {
    font-family: 'Playfair Display', serif;
    font-size: 2.5rem;
    margin-bottom: 30px;
    letter-spacing: -1px;
    color: var(--text-main);
  }

  .premium-form {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .input-group {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .input-group label {
    font-size: 0.9rem;
    font-weight: 600;
    color: var(--text-muted);
    margin-left: 2px;
  }

  .premium-form input, 
  .premium-form textarea {
    width: 100%;
    padding: 16px;
    border: 2px solid var(--border-soft);
    border-radius: 12px;
    font-size: 1rem;
    transition: var(--transition);
    background: white;
    font-family: inherit;
  }

  .premium-form input:focus, 
  .premium-form textarea:focus {
    outline: none;
    border-color: var(--primary-color);
    box-shadow: 0 0 0 4px rgba(0, 123, 255, 0.1);
  }

  .premium-form textarea {
    height: 180px;
    resize: vertical;
  }

  .btn-submit-premium {
    background: var(--primary-color);
    color: white;
    padding: 18px 30px;
    border: none;
    border-radius: 12px;
    font-size: 1.1rem;
    font-weight: 700;
    cursor: pointer;
    transition: var(--transition);
    box-shadow: 0 4px 14px rgba(0, 123, 255, 0.3);
    margin-top: 10px;
  }

  .btn-submit-premium:hover {
    background: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0, 123, 255, 0.4);
  }

  /* Right Column: Info */
  .contact-info-side {
    flex: 1;
    min-width: 250px;
    padding-left: 40px;
    border-left: 1px solid var(--border-soft);
  }

  .info-title {
    font-family: 'Playfair Display', serif;
    font-size: 1.8rem;
    margin-bottom: 30px;
    color: var(--text-main);
  }

  .info-item {
    margin-bottom: 35px;
  }

  .info-label {
    font-size: 0.8rem;
    font-weight: 800;
    color: var(--primary-color);
    text-transform: uppercase;
    letter-spacing: 0.15em;
    margin-bottom: 8px;
    display: block;
  }

  .info-content {
    font-size: 1.1rem;
    line-height: 1.6;
    color: var(--text-main);
    font-weight: 500;
  }

  .social-links {
    display: flex;
    gap: 15px;
    margin-top: 10px;
  }

  .social-link {
    text-decoration: none;
    font-weight: 600;
    color: var(--text-main);
    transition: var(--transition);
    padding-bottom: 2px;
    border-bottom: 2px solid var(--border-soft);
  }

  .social-link:hover {
    color: var(--primary-color);
    border-color: var(--primary-color);
  }

  @media (max-width: 800px) {
    .contact-grid {
      flex-direction: column;
      gap: 50px;
    }
    .contact-info-side {
      border-left: none;
      padding-left: 0;
      border-top: 1px solid var(--border-soft);
      padding-top: 40px;
    }
  }
</style>

<div class="contact-wrapper">
  <div class="contact-grid">
    
    <!-- Gửi tin nhắn -->
    <div class="contact-form-side">
      <h2 class="contact-title">Gửi tin nhắn</h2>
      
      <form action="https://formspree.io/f/mojarnww" method="POST" class="premium-form">
        <div class="input-group">
          <label>Tên của bạn</label>
          <input type="text" name="name" placeholder="Bạn tên là gì?" required>
        </div>
        
        <div class="input-group">
          <label>Email liên hệ</label>
          <input type="email" name="email" placeholder="example@email.com" required>
        </div>
        
        <div class="input-group">
          <label>Lời nhắn</label>
          <textarea name="message" placeholder="Chia sẻ chi tiết vấn đề bạn muốn trao đổi..." required></textarea>
        </div>
        
        <button type="submit" class="btn-submit-premium">Gửi tin nhắn ngay</button>
      </form>
    </div>

    <!-- Thông tin liên hệ -->
    <div class="contact-info-side">
      <h2 class="info-title">Kết nối trực tiếp</h2>
      
      <div class="info-item">
        <span class="info-label">Email</span>
        <div class="info-content">truongblueblack0702@gmail.com</div>
      </div>

      <div class="info-item">
        <span class="info-label">Vị trí</span>
        <div class="info-content">TP. Hồ Chí Minh, Việt Nam</div>
      </div>

      <div class="info-item">
        <span class="info-label">Mạng xã hội</span>
        <div class="social-links">
          <a href="#" class="social-link">LinkedIn</a>
          <a href="https://github.com/truongquangtruong" target="_blank" class="social-link">GitHub</a>
        </div>
      </div>

      <div style="margin-top: 50px; padding: 25px; background: var(--bg-accent); border-radius: 15px; font-size: 0.95rem; color: var(--text-muted); line-height: 1.6;">
        Tôi thường phản hồi email trong vòng 24h làm việc. Rất mong được kết nối và trao đổi cùng bạn!
      </div>
    </div>

  </div>
</div>