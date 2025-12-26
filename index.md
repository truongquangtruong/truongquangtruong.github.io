---
layout: default
title: Trang Chủ
---

<style>
  :root {
    --primary-color: #007bff;
    --primary-hover: #0056b3;
    --text-main: #1e293b;
    --text-muted: #64748b;
    --bg-card: #ffffff;
    --border-color: #e2e8f0;
    --shadow-sm: 0 1px 3px rgba(0,0,0,0.1);
    --shadow-md: 0 10px 30px rgba(0,0,0,0.08);
    --transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
    color: var(--text-main);
    background-color: #fcfcfc;
    margin: 0;
  }

  .hero-container {
    padding: 100px 20px;
    background: white;
    margin-bottom: 2rem;
    display: flex;
    justify-content: center;
  }

  .hero-flex-wrapper {
    max-width: 1100px;
    width: 100%;
    display: flex;
    gap: 60px;
    align-items: flex-start;
    text-align: left;
  }

  .profile-column {
    flex-shrink: 0;
  }

  .profile-img {
    width: 320px;
    height: 420px;
    border-radius: 20px;
    object-fit: cover;
    display: block;
    box-shadow: var(--shadow-md);
    transition: var(--transition);
  }

  .profile-img:hover {
    transform: translateY(-5px);
    box-shadow: 0 20px 40px rgba(0,0,0,0.12);
  }

  .content-column {
    flex: 1;
    padding-top: 10px;
  }

  .intro-label {
    color: var(--primary-color);
    font-size: 0.9rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 2px;
    margin-bottom: 15px;
    display: block;
  }

  .hero-name {
    font-size: 4.5rem;
    font-weight: 800;
    margin: 0 0 15px 0;
    line-height: 1;
    color: #111;
    letter-spacing: -2px;
    text-transform: uppercase;
  }

  .hero-title-group {
    display: flex;
    align-items: center;
    gap: 15px;
    font-size: 1.25rem;
    color: var(--text-main);
    margin-bottom: 20px;
  }

  .hero-title-group span {
    color: var(--text-muted);
  }

  .hero-major {
    font-size: 1.5rem;
    font-weight: 600;
    margin-bottom: 25px;
    color: #334155;
  }

  .hero-description {
    font-size: 1.1rem;
    color: var(--text-muted);
    line-height: 1.8;
    margin-bottom: 40px;
    max-width: 700px;
  }

  .cta-group {
    display: flex;
    gap: 15px;
    margin-bottom: 50px;
  }

  .btn-primary {
    padding: 14px 35px;
    background: var(--primary-color);
    color: white !important;
    border-radius: 12px;
    font-weight: 600;
    text-decoration: none;
    transition: var(--transition);
    box-shadow: 0 4px 14px rgba(0,123,255,0.3);
  }

  .btn-primary:hover {
    background: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 6px 20px rgba(0,123,255,0.4);
  }

  .btn-outline {
    padding: 14px 35px;
    border: 2px solid var(--border-color);
    color: var(--text-main) !important;
    border-radius: 12px;
    font-weight: 600;
    text-decoration: none;
    transition: var(--transition);
  }

  .btn-outline:hover {
    background: #f8fafc;
    border-color: var(--text-main);
  }

  /* Search Section */
  .search-section {
    max-width: 100%;
    margin: 0; /* Remove previous margin */
    padding: 0; /* Remove previous padding */
  }

  .search-box-wrapper {
    position: relative;
    display: flex;
    gap: 12px;
    align-items: stretch;
  }

  .search-input-container {
    position: relative;
    flex: 1;
    min-width: 0; /* Quan trọng để flex: 1 không bị tràn */
  }

  .search-input {
    width: 100%;
    padding: 16px 24px;
    border: 2px solid var(--border-color);
    border-radius: 16px;
    font-size: 1rem;
    outline: none;
    transition: var(--transition);
    background: white;
    box-sizing: border-box;
  }

  .btn-search {
    padding: 0 35px;
    background: var(--primary-color);
    color: white !important;
    border-radius: 16px;
    font-weight: 600;
    text-decoration: none;
    transition: var(--transition);
    border: none;
    cursor: pointer;
    white-space: nowrap;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0; /* Ngăn nút bị co lại */
  }

  .btn-search:hover {
    background: var(--primary-hover);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0,123,255,0.2);
  }

  .search-input:focus {
    border-color: var(--primary-color);
    box-shadow: 0 0 0 4px rgba(0,123,255,0.1);
  }

  #suggestion-box {
    display: none;
    position: absolute;
    width: 100%;
    background: white;
    border: 1px solid var(--border-color);
    border-radius: 16px;
    z-index: 1000;
    box-shadow: 0 15px 40px rgba(0,0,0,0.1);
    top: 65px;
    max-height: 400px;
    overflow-y: auto;
    text-align: left;
  }

  @media (max-width: 992px) {
    .hero-flex-wrapper {
      flex-direction: column;
      align-items: center;
      text-align: center;
      gap: 40px;
    }
    .hero-name {
      font-size: 3.5rem;
    }
    .hero-title-group {
      justify-content: center;
    }
    .cta-group {
      justify-content: center;
    }
    .hero-description {
      margin: 0 auto 40px;
    }
    .profile-img {
      width: 280px;
      height: 380px;
    }
  }

  @media (max-width: 600px) {
    .hero-name {
      font-size: 2.5rem;
      letter-spacing: -1px;
    }
    .hero-container {
      padding: 60px 20px;
    }
  }

  /* Sections */
  .section-title {
    text-align: center;
    font-size: 2rem;
    font-weight: 700;
    margin-bottom: 3rem;
    position: relative;
    padding-bottom: 15px;
  }

  .section-title::after {
    content: '';
    position: absolute;
    bottom: 0;
    left: 50%;
    transform: translateX(-50%);
    width: 60px;
    height: 4px;
    background: var(--primary-color);
    border-radius: 2px;
  }

  .grid-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 25px;
    padding: 20px;
    max-width: 1100px;
    margin: 0 auto 5rem;
  }

  .card {
    background: white;
    padding: 35px 25px;
    border-radius: 20px;
    border: 1px solid var(--border-color);
    transition: var(--transition);
    text-align: center;
  }

  .card:hover {
    transform: translateY(-8px);
    box-shadow: var(--shadow-md);
    border-color: var(--primary-color);
  }

  .card-icon {
    font-size: 2.5rem;
    margin-bottom: 20px;
    display: block;
  }

  .card h3 {
    margin-bottom: 15px;
    color: var(--text-main);
  }

  .card p {
    color: var(--text-muted);
    font-size: 0.95rem;
    line-height: 1.6;
  }

  /* Recent Posts */
  .recent-posts {
    background: #f8fafc;
    padding: 80px 20px;
    border-radius: 40px;
    margin-bottom: 5rem;
  }

  .post-card-mini {
    background: white;
    padding: 25px;
    border-radius: 16px;
    border: 1px solid var(--border-color);
    transition: var(--transition);
    text-decoration: none;
    color: inherit;
    display: block;
  }

  .post-card-mini:hover {
    transform: scale(1.02);
    box-shadow: var(--shadow-md);
  }

  .post-date {
    font-size: 0.8rem;
    color: var(--primary-color);
    font-weight: 700;
    text-transform: uppercase;
    margin-bottom: 10px;
    display: block;
  }

  .post-title {
    font-size: 1.25rem;
    font-weight: 700;
    margin-bottom: 10px;
  }
</style>

<div class="hero-container">
  <div class="profile-wrapper">
    <img src="{{ '/assets/me.png' | relative_url }}" alt="Trương Quang Trường" class="profile-img">
  </div>
  
  <h1 class="hero-title">Trương Quang Trường</h1>
  <p class="hero-subtitle">
    Khám phá hành trình nghiên cứu chuyên sâu về <strong>An ninh mạng</strong>, 
    hạ tầng mạng và kiến trúc hệ thống hiện đại.
  </p>

  <div class="cta-group">
    <a href="{{ '/about/' | relative_url }}" class="btn-primary">Tìm hiểu thêm</a>
    <a href="{{ '/blog/' | relative_url }}" class="btn-outline">Xem bài viết</a>
  </div>

  <div class="search-section">
    <div class="search-box-wrapper">
      <div class="search-input-container">
        <input type="text" id="search-home" class="search-input" placeholder="Bạn muốn tìm hiểu về điều gì?">
        <div id="suggestion-box"></div>
      </div>
      <button onclick="executeSearch()" class="btn-search">
        Tìm kiếm
      </button>
    </div>
  </div>
</div>

<div class="container">
  <h2 class="section-title">Lĩnh vực chuyên môn</h2>
  <div class="grid-container">
    <div class="card">
      <span class="card-icon">🌐</span>
      <h3>Hạ tầng mạng</h3>
      <p>Thiết kế và tối ưu hóa hạ tầng mạng đa tầng, thành thạo các giao thức OSPF, BGP và kiến trúc SDN.</p>
    </div>
    <div class="card">
      <span class="card-icon">🛡️</span>
      <h3>An ninh mạng</h3>
      <p>Phân tích lỗ hổng, thiết lập tường lửa đa tầng và bảo vệ hệ thống trước các cuộc tấn công hiện đại.</p>
    </div>
    <div class="card">
      <span class="card-icon">💻</span>
      <h3>Fullstack Dev</h3>
      <p>Xây dựng ứng dụng web bảo mật cao với quy trình Fullstack, chú trọng vào hiệu suất và trải nghiệm người dùng.</p>
    </div>
    <div class="card">
      <span class="card-icon">⚙️</span>
      <h3>Hệ thống phân tán</h3>
      <p>Nghiên cứu về tính khả dụng cao, khả năng mở rộng và quản lý dữ liệu trong các hệ thống phân tán.</p>
    </div>
  </div>

  <div class="recent-posts">
    <h2 class="section-title">Bài viết mới nhất</h2>
    <div class="grid-container" style="margin-bottom: 0;">
      {% assign recent_posts = site.posts | sort: "date" | reverse | limit: 3 %}
      {% for post in recent_posts %}
      <a href="{{ post.url | relative_url }}" class="post-card-mini">
        <span class="post-date">{{ post.date | date: "%-d/%m/%Y" }}</span>
        <div class="post-title">{{ post.title }}</div>
        <p style="margin: 0; font-size: 0.9rem;">{{ post.description }}</p>
      </a>
      {% endfor %}
    </div>
    <div style="text-align: center; margin-top: 3rem;">
      <a href="{{ '/blog/' | relative_url }}" style="color: var(--primary-color); font-weight: 700; text-decoration: none;">
        Xem toàn bộ kho tàng kiến thức →
      </a>
    </div>
  </div>
</div>

<script>
  // Tái sử dụng dữ liệu bài viết
  const posts = [
    {% for post in site.posts %}
    {
      title: {{ post.title | jsonify }},
      url: {{ post.url | relative_url | jsonify }},
      description: {{ post.description | jsonify }},
      date: "{{ post.date | date: '%-d/%m/%Y' }}"
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  const searchInput = document.getElementById('search-home');
  const suggestionBox = document.getElementById('suggestion-box');

  searchInput.addEventListener('keyup', (e) => {
    const query = e.target.value.toLowerCase();
    if (query.length > 0) {
      const matches = posts.filter(post => 
        post.title.toLowerCase().includes(query) || 
        (post.description && post.description.toLowerCase().includes(query))
      ).slice(0, 5);

      if (matches.length > 0) {
        suggestionBox.innerHTML = matches.map(post => `
          <div onclick="window.location.href='${post.url}'" 
               style="padding: 15px 20px; cursor: pointer; border-bottom: 1px solid #f1f5f9; transition: 0.2s;"
               onmouseover="this.style.backgroundColor='#f8fafc'"
               onmouseout="this.style.backgroundColor='transparent'">
            <div style="font-weight: 600; color: #1e293b; margin-bottom: 4px;">${post.title}</div>
            <div style="font-size: 0.85em; color: #64748b;">${post.date} - ${post.description || ''}</div>
          </div>
        `).join('');
        suggestionBox.style.display = 'block';
      } else {
        suggestionBox.style.display = 'none';
      }
    } else {
      suggestionBox.style.display = 'none';
    }

    if (e.key === 'Enter') {
      executeSearch();
    }
  });

  function executeSearch() {
    const query = searchInput.value.toLowerCase();
    if (query.trim() !== "") {
      window.location.href = "{{ '/blog/' | relative_url }}?q=" + encodeURIComponent(query);
    } else {
      window.location.href = "{{ '/blog/' | relative_url }}";
    }
  }

  document.addEventListener('click', (e) => {
    if (!e.target.closest('#search-home') && !e.target.closest('#suggestion-box')) {
      suggestionBox.style.display = 'none';
    }
  });
</script>
