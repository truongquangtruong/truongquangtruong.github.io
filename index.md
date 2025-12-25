---
layout: default
title: Trang Chủ
---

<style>
  /* Global Overrides */
  body {
    background-color: #0f172a !important;
    color: #e2e8f0 !important;
    font-family: 'Inter', -apple-system, blinkmacsystemfont, 'Segoe UI', roboto, sans-serif;
  }
  
  /* Header Overrides - Ensuring Visibility */
  .site-header {
    background-color: rgba(15, 23, 42, 0.95) !important;
    border-bottom: 1px solid #1e293b !important;
    position: relative;
    z-index: 1000;
  }
  .site-title, .site-title:visited {
    color: #f8fafc !important;
    opacity: 1 !important;
    display: inline-block !important; /* Ensure it's not hidden */
  }
  .page-link, .page-link:visited {
    color: #e2e8f0 !important;
  }

  /* Hero Section */
  .hero-wrapper {
    text-align: center; 
    padding: 60px 20px; 
    background: radial-gradient(circle at center, #1e293b 0%, #0f172a 100%); 
    border-radius: 20px; 
    margin-bottom: 50px;
    box-shadow: 0 10px 30px -10px rgba(0,0,0,0.5);
    border: 1px solid #334155;
  }

  .profile-avatar {
    width: 150px; 
    height: 150px; 
    border-radius: 50%; 
    object-fit: cover; 
    border: 4px solid #38bdf8; 
    box-shadow: 0 0 25px rgba(56, 189, 248, 0.4); 
    margin-bottom: 20px;
    transition: transform 0.3s;
  }
  .profile-avatar:hover { transform: scale(1.05); }

  .hero-title {
    font-size: 2.8em; 
    font-weight: 800; 
    margin-bottom: 10px;
    color: #f8fafc; /* Fallback */
    background: linear-gradient(to right, #ffffff, #94a3b8);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .hero-desc {
    font-size: 1.25em; 
    color: #94a3b8;
    margin-bottom: 30px;
  }

  /* Social Icons */
  .social-container {
    display: flex; 
    justify-content: center; 
    gap: 20px; 
    margin-bottom: 30px;
  }
  .social-link svg {
    width: 28px; 
    height: 28px; 
    stroke: #94a3b8; 
    transition: 0.3s;
  }
  .social-link:hover svg { stroke: #38bdf8; }

  /* Buttons */
  .hero-buttons {
    display: flex; 
    justify-content: center; 
    gap: 15px; 
    margin-bottom: 30px;
  }
  .btn-custom {
    padding: 12px 30px; 
    border-radius: 50px; 
    font-weight: 600; 
    text-decoration: none !important; 
    transition: all 0.3s;
  }
  .btn-primary { 
    background: #007bff; /* Original Blue */
    color: white !important; 
    box-shadow: 0 4px 15px rgba(0, 123, 255, 0.3); 
  }
  .btn-primary:hover { 
    background: #0069d9; 
    transform: translateY(-2px); 
  }
  .btn-outline { 
    background: transparent; 
    color: #fff !important; 
    border: 2px solid #334155; 
  }
  .btn-outline:hover { 
    border-color: #fff; 
    transform: translateY(-2px); 
  }

  /* Search Bar */
  .search-wrapper {
    display: flex; 
    max-width: 500px; 
    margin: 0 auto; 
    gap: 10px;
  }
  .search-input {
    flex: 1; 
    padding: 12px 20px; 
    background: #1e293b; 
    border: 1px solid #334155; 
    color: white; 
    border-radius: 10px; 
    outline: none;
  }
  .search-input:focus { border-color: #38bdf8; }

  /* Info Cards */
  .info-grid {
    display: grid; 
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); 
    gap: 30px; 
    margin-bottom: 50px;
  }
  .info-card {
    padding: 25px; 
    background: #1e293b; 
    border: 1px solid #334155; 
    border-radius: 15px; 
    transition: 0.3s;
  }
  .info-card:hover { 
    transform: translateY(-5px); 
    border-color: #38bdf8; 
  }
  .info-title { color: #38bdf8; margin-bottom: 10px; }
  .info-text { color: #cbd5e1; line-height: 1.6; }

  /* Post List */
  .section-heading {
    color: #f1f5f9; 
    border-bottom: 2px solid #38bdf8; 
    padding-bottom: 10px; 
    margin-bottom: 30px;
    display: inline-block;
  }
  .post-item {
    background: #1e293b; 
    border: 1px solid #334155; 
    border-radius: 12px; 
    margin-bottom: 15px; 
    transition: 0.3s;
  }
  .post-item:hover {
    border-color: #38bdf8;
    transform: translateX(5px);
  }
  .post-link {
    display: flex; 
    justify-content: space-between; 
    align-items: center; 
    padding: 20px; 
    text-decoration: none !important;
  }
  .post-title { margin: 0; color: #f1f5f9; font-size: 1.1em; font-weight: 500; }
  .post-meta { color: #94a3b8; font-size: 0.9em; margin-top: 5px; display: block; }
  .read-more { color: #38bdf8; font-weight: 600; }

</style>

<div class="hero-wrapper">
  <!-- Avatar -->
  <img src="assets/me.png" alt="Trương Quang Trường" class="profile-avatar">
  
  <!-- Content -->
  <h1 class="hero-title">Chào mừng bạn đến với blog của tôi</h1>
  <p class="hero-desc">Tôi là <strong>Trương Quang Trường</strong> – Chuyên ngành an ninh mạng</p>

  <!-- Hero Buttons -->
  <div class="hero-buttons">
    <a href="{{ '/about/' | relative_url }}" class="btn-custom btn-primary">Giới thiệu</a>
    <a href="{{ '/contact/' | relative_url }}" class="btn-custom btn-outline">Liên Hệ Ngay</a>
  </div>


  
  <!-- Social Icons (Moved below buttons) -->
  <div class="social-container" style="margin-top: 20px;">
    <a href="https://github.com/truongquangtruong" class="social-link" target="_blank">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M9 19c-5 1.5-5-2.5-7-3m14 6v-3.87a3.37 3.37 0 0 0-.94-2.61c3.14-.35 6.44-1.54 6.44-7A5.44 5.44 0 0 0 20 4.77 5.07 5.07 0 0 0 19.91 1S18.73.65 16 2.48a13.38 13.38 0 0 0-7 0C6.27.65 5.09 1 5.09 1A5.07 5.07 0 0 0 5 4.77a5.44 5.44 0 0 0-1.5 3.78c0 5.42 3.3 6.61 6.44 7A3.37 3.37 0 0 0 9 18.13V22"></path></svg>
    </a>
    <a href="#" class="social-link">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 2h-3a5 5 0 0 0-5 5v3H7v4h3v8h4v-8h3l1-4h-4V7a1 1 0 0 1 1-1h3z"></path></svg>
    </a>
    <a href="#" class="social-link">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M16 8a6 6 0 0 1 6 6v7h-4v-7a2 2 0 0 0-2-2 2 2 0 0 0-2 2v7h-4v-7a6 6 0 0 1 6-6z"></path><rect x="2" y="9" width="4" height="12"></rect><circle cx="4" cy="4" r="2"></circle></svg>
    </a>
  </div>

</div>

<!-- Info Cards -->
<div class="info-grid">
  <div class="info-card">
    <h3 class="info-title">🌐 Network Engineering</h3>
    <p class="info-text">Nghiên cứu triển khai hạ tầng mạng đa tầng, tập trung tối ưu hóa các giao thức định tuyến OSPF và BGP.</p>
  </div>
  <div class="info-card">
    <h3 class="info-title">🛡️ Cyber Security</h3>
    <p class="info-text">Thiết lập lá chắn bảo mật đa tầng qua xác thực JWT và kiểm soát luồng dữ liệu Fullstack.</p>
  </div>
</div>

