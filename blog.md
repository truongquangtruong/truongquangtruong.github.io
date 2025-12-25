---
layout: page
title: "Blog"
permalink: /blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; max-width: 1000px; margin: 0 auto;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px; text-align: center; font-size: 1.1em; line-height: 1.6;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="display: flex; gap: 15px; margin-bottom: 40px; align-items: center; justify-content: center;">
    <div style="position: relative; flex: 1; max-width: 700px;">
      <input type="text" id="search-blog" placeholder="Tìm kiếm bài viết nghiên cứu..." 
             onkeyup="handleKeyUp(event)"
             style="width: 100%; padding: 15px 20px; border: 2px solid #e1e8ed; border-radius: 12px; font-size: 16px; outline: none; transition: 0.3s;"
             onfocus="this.style.borderColor='#007bff'" onblur="this.style.borderColor='#e1e8ed'">
      
      <span onclick="clearSearch()" id="clear-btn" 
            style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #aaa; display: none; font-size: 20px;">✕</span>
      
      <div id="suggestion-box" 
           style="display: none; position: absolute; width: 100%; background: white; border: 1px solid #ddd; border-radius: 12px; z-index: 1000; box-shadow: 0 15px 35px rgba(0,0,0,0.1); top: 65px; max-height: 300px; overflow-y: auto;">
      </div>
    </div>

    <button onclick="executeSearch()" 
            style="padding: 15px 35px; background-color: #007bff; color: white; border: none; border-radius: 12px; font-weight: bold; cursor: pointer; font-size: 16px; box-shadow: 0 4px 12px rgba(0,123,255,0.3); transition: 0.3s;">
      Tìm kiếm
    </button>
  </div>

  <div id="blog-posts-container">
    {% assign posts = site.posts | sort: "date" | reverse %}
    {% for post in posts %}
    <div class="post-card" style="border: 1px solid #e1e8ed; border-radius: 20px; padding: 30px; margin-bottom: 30px; background: #fff; transition: 0.3s; box-shadow: 0 4px 6px rgba(0,0,0,0.02);" 
         onmouseover="this.style.transform='translateY(-5px)'; this.style.boxShadow='0 12px 20px rgba(0,0,0,0.08)';" 
         onmouseout="this.style.transform='translateY(0)'; this.style.boxShadow='0 4px 6px rgba(0,0,0,0.02)';">
      
      <div style="display: flex; align-items: center; gap: 10px; margin-bottom: 12px;">
        <span style="background: #eef6ff; color: #007bff; padding: 5px 12px; border-radius: 8px; font-size: 0.85em; font-weight: bold;">
          {{ post.date | date: "%-d/%m/%Y" }}
        </span>
        {% if post.categories %}
          <span style="color: #94a3b8; font-size: 0.9em;">•</span>
          <span style="color: #64748b; font-size: 0.85em; font-weight: 500;">{{ post.categories | join: ", " }}</span>
        {% endif %}
      </div>

      <h2 style="margin: 0 0 12px 0; font-size: 1.6em;">
        <a href="{{ post.url | relative_url }}" style="color: #1e293b; text-decoration: none; transition: 0.2s;" onmouseover="this.style.color='#007bff'" onmouseout="this.style.color='#1e293b'">
          {{ post.title }}
        </a>
      </h2>

      <p style="color: #475569; line-height: 1.7; margin-bottom: 20px; font-size: 1.05em;">
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>

      <a href="{{ post.url | relative_url }}" style="color: #007bff; text-decoration: none; font-weight: 600; font-size: 0.95em; display: inline-flex; align-items: center; gap: 5px;">
        Đọc tiếp <span>→</span>
      </a>
    </div>
    {% endfor %}
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
    const input = document.getElementById('search-blog');
    input.value = '';
    document.getElementById('suggestion-box').style.display = 'none';
    document.getElementById('clear-btn').style.display = 'none';
    executeSearch();
  }

  function executeSearch() {
    const query = document.getElementById('search-blog').value.toLowerCase();
    const container = document.getElementById('blog-posts-container');
    const postCards = container.getElementsByClassName('post-card');
    
    document.getElementById('suggestion-box').style.display = 'none';

    for (let card of postCards) {
      const content = card.innerText.toLowerCase();
      if (content.includes(query)) {
        card.style.display = 'block';
      } else {
        card.style.display = 'none';
      }
    }
  }

  // Đóng suggestion box khi click ra ngoài
  document.addEventListener('click', function(e) {
    if (!e.target.closest('#search-blog') && !e.target.closest('#suggestion-box')) {
      document.getElementById('suggestion-box').style.display = 'none';
    }
  });
</script>