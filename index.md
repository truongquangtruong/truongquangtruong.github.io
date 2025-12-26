---
layout: default
title: Trang Chủ
---

<style>
  .hero-section {
    text-align: center;
    padding: 60px 20px;
    background: #ffffff;
    border-radius: 12px;
    margin-bottom: 40px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  .profile-img {
    width: 100%;
    max-width: 250px;
    height: auto;
    border-radius: 12px;
    object-fit: contain;
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
    color: white !important;
    text-decoration: none;
    border-radius: 25px;
    font-weight: 600;
    transition: transform 0.2s;
  }

  .btn-hero:hover {
    transform: translateY(-2px);
    background-color: #0056b3;
    text-decoration: none;
    color: white !important;
  }

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
</style>

<div class="hero-section">
  <img src="{{ '/assets/me.png' | relative_url }}" alt="Trương Quang Trường" class="profile-img">
  
  <h1 class="hero-title">Chào mừng bạn đến với blog của tôi</h1>
  <p class="hero-subtitle">Tôi là <strong>Trương Quang Trường</strong> – Chuyên ngành an ninh mạng</p>

  <div style="margin-bottom: 20px;">
    <a href="{{ '/about/' | relative_url }}" class="btn-hero">Giới thiệu về tôi</a>
    <a href="{{ '/blog/' | relative_url }}" class="btn-hero" style="margin-left: 10px;">Bài viết</a>
  </div>

  <!-- Thanh tìm kiếm giống Blog -->
  <div style="display: flex; gap: 15px; margin-top: 40px; align-items: center; justify-content: center;">
    <div style="position: relative; flex: 1; max-width: 700px;">
      <input type="text" id="search-home" placeholder="Tìm kiếm bài viết nghiên cứu..." 
             onkeyup="handleKeyUp(event)"
             style="width: 100%; padding: 15px 20px; border: 2px solid #e1e8ed; border-radius: 12px; font-size: 16px; outline: none; transition: 0.3s;"
             onfocus="this.style.borderColor='#007bff'" onblur="this.style.borderColor='#e1e8ed'">
      
      <span onclick="clearSearch()" id="clear-btn" 
            style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #aaa; display: none; font-size: 20px;">✕</span>
      
      <div id="suggestion-box" 
           style="display: none; position: absolute; width: 100%; background: white; border: 1px solid #ddd; border-radius: 12px; z-index: 1000; box-shadow: 0 15px 35px rgba(0,0,0,0.1); top: 65px; max-height: 300px; overflow-y: auto; text-align: left;">
      </div>
    </div>

    <button onclick="executeSearch()" 
            style="padding: 15px 35px; background-color: #007bff; color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; font-size: 16px; box-shadow: 0 4px 12px rgba(0,123,255,0.3); transition: 0.3s;">
      Tìm kiếm
    </button>
  </div>
</div>

<script>
  // Dữ liệu bài viết để tìm kiếm
  const posts = [
    {% for post in site.posts %}
    {
      title: {{ post.title | jsonify }},
      url: {{ post.url | relative_url | jsonify }},
      excerpt: {{ post.excerpt | strip_html | truncatewords: 20 | jsonify }},
      date: "{{ post.date | date: '%-d/%m/%Y' }}"
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  function handleKeyUp(event) {
    const query = event.target.value.toLowerCase();
    const suggestionBox = document.getElementById('suggestion-box');
    const clearBtn = document.getElementById('clear-btn');

    if (query.length > 0) {
      clearBtn.style.display = 'block';
      const matches = posts.filter(post => 
        post.title.toLowerCase().includes(query) || 
        post.excerpt.toLowerCase().includes(query)
      ).slice(0, 5);

      if (matches.length > 0) {
        suggestionBox.innerHTML = matches.map(post => `
          <div onclick="window.location.href='${post.url}'" 
               style="padding: 15px 20px; cursor: pointer; border-bottom: 1px solid #f1f5f9; transition: 0.2s;"
               onmouseover="this.style.backgroundColor='#f8fafc'"
               onmouseout="this.style.backgroundColor='transparent'">
            <div style="font-weight: 600; color: #1e293b; margin-bottom: 4px;">${post.title}</div>
            <div style="font-size: 0.85em; color: #64748b;">${post.date} - ${post.excerpt}</div>
          </div>
        `).join('');
        suggestionBox.style.display = 'block';
      } else {
        suggestionBox.style.display = 'none';
      }
    } else {
      clearBtn.style.display = 'none';
      suggestionBox.style.display = 'none';
    }

    if (event.key === 'Enter') {
      executeSearch();
    }
  }

  function clearSearch() {
    const input = document.getElementById('search-home');
    input.value = '';
    document.getElementById('suggestion-box').style.display = 'none';
    document.getElementById('clear-btn').style.display = 'none';
  }

  function executeSearch() {
    const query = document.getElementById('search-home').value.toLowerCase();
    if (query.trim() !== "") {
      window.location.href = "{{ '/blog/' | relative_url }}?q=" + encodeURIComponent(query);
    } else {
      window.location.href = "{{ '/blog/' | relative_url }}";
    }
  }

  // Đóng suggestion box khi click ra ngoài
  document.addEventListener('click', function(e) {
    if (!e.target.closest('#search-home') && !e.target.closest('#suggestion-box')) {
      document.getElementById('suggestion-box').style.display = 'none';
    }
  });
</script>

<div class="features-grid">
  <div class="feature-card">
    <h3 style="color: #007bff;">🌐 Network Engineering</h3>
    <p style="color: #4a5568;">Nghiên cứu triển khai hạ tầng mạng đa tầng, tối ưu hóa OSPF và BGP.</p>
  </div>
  <div class="feature-card">
    <h3 style="color: #007bff;">🛡️ Cyber Security</h3>
    <p style="color: #4a5568;">Thiết lập lá chắn bảo mật đa tầng qua xác thực JWT và kiểm soát Fullstack.</p>
  </div>
</div>