---
layout: page
title: "Blog"
permalink: /blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; max-width: 1000px; margin: 0 auto;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px; text-align: center; font-size: 1.1em; line-height: 1.6;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="display: flex; gap: 15px; margin-bottom: 25px; align-items: center; justify-content: center;">
    <div style="position: relative; flex: 1; max-width: 700px;">
      <input type="text" id="search-blog" placeholder="Tìm kiếm bài viết nghiên cứu..." 
             onkeyup="handleSearch()"
             style="width: 100%; padding: 15px 20px; border: 2px solid #e1e8ed; border-radius: 12px; font-size: 16px; outline: none; transition: 0.3s;"
             onfocus="this.style.borderColor='#007bff'" onblur="this.style.borderColor='#e1e8ed'">
      
      <span onclick="clearSearch()" id="clear-btn" 
            style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #aaa; display: none; font-size: 20px;">✕</span>
      
      <div id="suggestion-box" 
           style="display: none; position: absolute; width: 100%; background: white; border: 1px solid #ddd; border-radius: 12px; z-index: 1000; box-shadow: 0 15px 35px rgba(0,0,0,0.1); top: 65px; max-height: 300px; overflow-y: auto;">
      </div>
    </div>

    <button onclick="handleSearch()" 
            style="padding: 15px 35px; background-color: #007bff; color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; font-size: 16px; box-shadow: 0 4px 12px rgba(0,123,255,0.3); transition: 0.3s;">
      Tìm kiếm
    </button>
  </div>

  <!-- Sorting Feature -->
  <div style="display: flex; justify-content: center; align-items: center; gap: 15px; margin-bottom: 40px; font-family: inherit;">
    <span style="font-size: 0.85em; font-weight: 700; color: #94a3b8; text-transform: uppercase; letter-spacing: 1px;">Sắp xếp theo:</span>
    <div style="background: #f1f5f9; padding: 4px; border-radius: 12px; display: flex; position: relative; width: fit-content; border: 1px solid #e2e8f0;">
      <div id="sort-highlight" style="position: absolute; width: 110px; height: calc(100% - 8px); background: #fff; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.08); transition: transform 0.3s cubic-bezier(0.4, 0, 0.2, 1); z-index: 1; left: 4px;"></div>
      <button onclick="changeSort('newest')" id="btn-newest" style="position: relative; z-index: 2; padding: 8px 20px; border: none; background: transparent; cursor: pointer; font-weight: 600; font-size: 0.9em; color: #007bff; width: 110px; transition: 0.3s;">Mới nhất</button>
      <button onclick="changeSort('oldest')" id="btn-oldest" style="position: relative; z-index: 2; padding: 8px 20px; border: none; background: transparent; cursor: pointer; font-weight: 600; font-size: 0.9em; color: #64748b; width: 110px; transition: 0.3s;">Cũ nhất</button>
    </div>
  </div>

  <div id="blog-posts-container">
    <!-- Posts will be rendered by JavaScript -->
  </div>
</div>

<script>
  // Dữ liệu bài viết để tìm kiếm và sắp xếp
  const allPosts = [
    {% for post in site.posts %}
    {
      title: {{ post.title | jsonify }},
      url: {{ post.url | relative_url | jsonify }},
      excerpt: {{ post.excerpt | strip_html | truncatewords: 30 | jsonify }},
      dateStr: "{{ post.date | date: '%-d/%m/%Y' }}",
      timestamp: {{ post.date | date: "%s" }},
      categories: {{ post.categories | join: ", " | jsonify }}
    }{% unless forloop.last %},{% endunless %}
    {% endfor %}
  ];

  let currentSort = 'newest';
  let searchQuery = '';

  function renderPosts() {
    const container = document.getElementById('blog-posts-container');
    
    // Filter
    let filteredPosts = allPosts.filter(post => 
      post.title.toLowerCase().includes(searchQuery) || 
      post.excerpt.toLowerCase().includes(searchQuery)
    );

    // Sort
    filteredPosts.sort((a, b) => {
      return currentSort === 'newest' ? b.timestamp - a.timestamp : a.timestamp - b.timestamp;
    });

    // Render
    container.innerHTML = filteredPosts.map(post => `
      <div class="post-card" style="border: 1px solid #e1e8ed; border-radius: 20px; padding: 30px; margin-bottom: 30px; background: #fff; transition: 0.3s; box-shadow: 0 4px 6px rgba(0,0,0,0.02);" 
           onmouseover="this.style.transform='translateY(-5px)'; this.style.boxShadow='0 12px 20px rgba(0,0,0,0.08)';" 
           onmouseout="this.style.transform='translateY(0)'; this.style.boxShadow='0 4px 6px rgba(0,0,0,0.02)';"
           onclick="window.location.href='${post.url}'" style="cursor: pointer;">
        
        <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 12px;">
          <span style="background: #eef6ff; color: #007bff; padding: 5px 12px; border-radius: 8px; font-size: 0.85em; font-weight: bold;">
            ${post.dateStr}
          </span>
          ${post.categories ? `
            <span style="color: #94a3b8; font-size: 0.9em;">•</span>
            <span style="color: #64748b; font-size: 0.85em; font-weight: 500;">${post.categories}</span>
          ` : ''}
        </div>

        <h2 style="margin: 0 0 12px 0; font-size: 1.6em;">
          <a href="${post.url}" style="color: #1e293b; text-decoration: none; transition: 0.2s;" onmouseover="this.style.color='#007bff'" onmouseout="this.style.color='#1e293b'">
            ${post.title}
          </a>
        </h2>

        <p style="color: #475569; line-height: 1.7; margin-bottom: 20px; font-size: 1.05em;">
          ${post.excerpt}
        </p>

        <a href="${post.url}" style="color: #007bff; text-decoration: none; font-weight: 600; font-size: 0.95em; display: inline-flex; align-items: center; gap: 5px;">
          Đọc tiếp <span>→</span>
        </a>
      </div>
    `).join('');

    if (filteredPosts.length === 0) {
      container.innerHTML = `<div style="text-align: center; padding: 50px; color: #64748b;">Không tìm thấy bài viết nào phù hợp.</div>`;
    }
  }

  function handleSearch() {
    const input = document.getElementById('search-blog');
    searchQuery = input.value.toLowerCase();
    
    const suggestionBox = document.getElementById('suggestion-box');
    const clearBtn = document.getElementById('clear-btn');

    if (searchQuery.length > 0) {
      clearBtn.style.display = 'block';
      const matches = allPosts.filter(post => 
        post.title.toLowerCase().includes(searchQuery) || 
        post.excerpt.toLowerCase().includes(searchQuery)
      ).slice(0, 5);

      if (matches.length > 0) {
        suggestionBox.innerHTML = matches.map(post => `
          <div onclick="window.location.href='${post.url}'" 
               style="padding: 15px 20px; cursor: pointer; border-bottom: 1px solid #f1f5f9; transition: 0.2s;"
               onmouseover="this.style.backgroundColor='#f8fafc'"
               onmouseout="this.style.backgroundColor='transparent'">
            <div style="font-weight: 600; color: #1e293b; margin-bottom: 4px;">${post.title}</div>
            <div style="font-size: 0.85em; color: #64748b;">${post.dateStr} - ${post.excerpt.split(' ').slice(0, 10).join(' ')}...</div>
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

    renderPosts();
  }

  function changeSort(order) {
    currentSort = order;
    const highlight = document.getElementById('sort-highlight');
    const btnNewest = document.getElementById('btn-newest');
    const btnOldest = document.getElementById('btn-oldest');

    if (order === 'newest') {
      highlight.style.transform = 'translateX(0)';
      btnNewest.style.color = '#007bff';
      btnOldest.style.color = '#64748b';
    } else {
      highlight.style.transform = 'translateX(110px)';
      btnNewest.style.color = '#64748b';
      btnOldest.style.color = '#007bff';
    }

    renderPosts();
  }

  function clearSearch() {
    const input = document.getElementById('search-blog');
    input.value = '';
    searchQuery = '';
    document.getElementById('suggestion-box').style.display = 'none';
    document.getElementById('clear-btn').style.display = 'none';
    renderPosts();
  }

  // Xử lý tìm kiếm từ URL (cho phép trang chủ chuyển hướng đến)
  window.addEventListener('load', function() {
    const urlParams = new URLSearchParams(window.location.search);
    const query = urlParams.get('q');
    if (query) {
      const searchInput = document.getElementById('search-blog');
      if (searchInput) {
        searchInput.value = query;
        searchQuery = query.toLowerCase();
        const clearBtn = document.getElementById('clear-btn');
        if (clearBtn) clearBtn.style.display = 'block';
      }
    }
    renderPosts(); // Luôn render bài viết lần đầu
  });

  // Đóng suggestion box khi click ra ngoài
  document.addEventListener('click', (e) => {
    if (!e.target.closest('#search-blog') && !e.target.closest('#suggestion-box')) {
      const suggestionBox = document.getElementById('suggestion-box');
      if (suggestionBox) suggestionBox.style.display = 'none';
    }
  });
</script>
```