---
layout: default
title: Trang Chủ
---

<style>
  /* Light Theme Styles for Homepage */
  .hero-section {
    text-align: center;
    padding: 60px 20px;
    background: #ffffff;
    border-radius: 12px;
    margin-bottom: 40px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  .profile-img {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    object-fit: cover;
    border: 4px solid #007bff;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    margin-bottom: 20px;
  }

  .hero-title {
    color: #1a202c;
    font-size: 2.5em;
    font-weight: 700;
    margin-bottom: 10px;
  }

  .hero-subtitle {
    color: #4a5568;
    font-size: 1.2em;
    margin-bottom: 30px;
  }

  .btn-hero {
    display: inline-block;
    padding: 10px 25px;
    background-color: #007bff;
    color: white;
    text-decoration: none;
    border-radius: 25px;
    font-weight: 600;
    transition: transform 0.2s;
  }

  .btn-hero:hover {
    transform: translateY(-2px);
    background-color: #0056b3;
    text-decoration: none;
    color: white;
  }

  /* Info Cards */
  .features-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 30px;
    margin-bottom: 50px;
  }

  .feature-card {
    background: #f8f9fa;
    padding: 25px;
    border-radius: 12px;
    border: 1px solid #e9ecef;
    transition: 0.3s;
  }

  .feature-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 5px 15px rgba(0,0,0,0.05);
    border-color: #007bff;
  }

  .feature-title {
    color: #007bff;
    font-size: 1.25em;
    margin-top: 0;
    margin-bottom: 15px;
  }

  .feature-text {
    color: #4a5568;
    line-height: 1.6;
    margin-bottom: 0;
  }
</style>

<div class="hero-section">
  <img src="assets/me.png" alt="Trương Quang Trường" class="profile-img">
  
  <h1 class="hero-title">Chào mừng bạn đến với blog của tôi</h1>
  <p class="hero-subtitle">Tôi là <strong>Trương Quang Trường</strong> – Chuyên ngành an ninh mạng</p>

  <div style="margin-bottom: 20px;">
    <a href="{{ '/about/' | relative_url }}" class="btn-hero">Giới thiệu về tôi</a>
  </div>
</div>

<div class="features-grid">
  <div class="feature-card">
    <h3 class="feature-title">🌐 Network Engineering</h3>
    <p class="feature-text">Nghiên cứu triển khai hạ tầng mạng đa tầng, tập trung tối ưu hóa các giao thức định tuyến OSPF và BGP.</p>
  </div>
  <div class="feature-card">
    <h3 class="feature-title">🛡️ Cyber Security</h3>
    <p class="feature-text">Thiết lập lá chắn bảo mật đa tầng qua xác thực JWT và kiểm soát luồng dữ liệu Fullstack.</p>
  </div>
</div>

