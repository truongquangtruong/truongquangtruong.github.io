---
layout: page
title: "Blog"
permalink: /blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="position: relative; margin-bottom: 40px;">
    <input type="text" id="search-blog" placeholder="Tìm kiếm bài học (Gõ từ khóa hoặc dán tiêu đề)..." 
           onkeyup="liveSearch()"
           style="width: 100%; padding: 15px 20px; border: 2px solid #e1e8ed; border-radius: 10px; font-size: 16px; outline: none; transition: 0.3s;"
           onfocus="this.style.borderColor='#007bff'">
    
    <div id="suggestion-box" style="display: none; position: absolute; width: 100%; background: white; border: 1px solid #e1e8ed; border-radius: 8px; z-index: 100; box-shadow: 0 10px 25px rgba(0,0,0,0.1); margin-top: 5px; max-height: 300px; overflow-y: auto;">
    </div>
  </div>

  <div id="blog-posts-container" style="display: flex; flex-direction: column; gap: 25px;">
    {% assign posts = site.posts | sort: "weight" %}
    {% for post in posts %}
    <div class="post-card" data-title="{{ post.title | downcase }}" style="border: 1px solid #e1e8ed; border-radius: 12px; padding: 20px; transition: 0.3s; background: #fff;" onmouseover="this.style.boxShadow='0 5px 15px rgba(0,0,0,0.1)'" onmouseout="this.style.boxShadow='none'">
      <span style="color: #007bff; font-weight: bold; font-size: 0.85em; text-transform: uppercase;">{{ post.date | date: "%b %d, %Y" }}</span>
      <h3 style="margin: 10px 0;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #1a202c;">{{ post.title }}</a>
      </h3>
      <p style="color: #4a5568; font-size: 0.95em;">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
      <a href="{{ post.url | relative_url }}" style="color: #007bff; font-weight: 600; text-decoration: none; font-size: 0.9em;">Đọc thêm →</a>
    </div>
    {% endfor %}
  </div>
</div>

<script>
// Dữ liệu bài viết để gợi ý
const postData = [
  {% for post in site.posts %}
    { title: "{{ post.title }}", url: "{{ post.url | relative_url }}" }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

function liveSearch() {
    const input = document.getElementById('search-blog').value.trim();
    const filter = input.toLowerCase();
    const suggestionBox = document.getElementById('suggestion-box');
    
    if (filter === "") {
        suggestionBox.style.display = "none";
        return;
    }

    // TRƯỜNG HỢP 1: Copy nguyên tiêu đề - Nhảy bài ngay lập tức
    const exactMatch = postData.find(p => p.title.toLowerCase() === filter);
    if (exactMatch) {
        window.location.href = exactMatch.url;
        return;
    }

    // TRƯỜNG HỢP 2: Gợi ý các bài tương tự
    const matches = postData.filter(p => p.title.toLowerCase().includes(filter));
    
    if (matches.length > 0) {
        suggestionBox.innerHTML = matches.map(p => `
            <div style="padding: 12px 20px; cursor: pointer; border-bottom: 1px solid #f0f0f0;" 
                 onclick="window.location.href='${p.url}'"
                 onmouseover="this.style.backgroundColor='#f8f9fa'"
                 onmouseout="this.style.backgroundColor='#fff'">
                ${p.title}
            </div>
        `).join('');
        suggestionBox.style.display = "block";
    } else {
        suggestionBox.style.display = "none";
    }
}
</script>