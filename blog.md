---
layout: page
title: "Blog"
permalink: /Quang Trường Blog/
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; max-width: 1000px; margin: 0 auto;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px; text-align: center;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="display: flex; gap: 15px; margin-bottom: 40px; align-items: center; justify-content: center;">
    <div style="position: relative; flex: 1; max-width: 700px;">
      <input type="text" id="search-blog" placeholder="Tìm kiếm bài viết ..." 
             onkeyup="handleKeyUp(event)"
             style="width: 100%; padding: 15px 20px; border: 1px solid #ddd; border-radius: 10px; font-size: 16px; outline: none;">
      
      <span onclick="clearSearch()" id="clear-btn" 
            style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #aaa; display: none;">✕</span>
      
      <div id="suggestion-box" 
           style="display: none; position: absolute; width: 100%; background: white; border: 1px solid #ddd; border-radius: 8px; z-index: 1000; box-shadow: 0 10px 25px rgba(0,0,0,0.1); top: 60px; max-height: 250px; overflow-y: auto;">
      </div>
    </div>

    <button onclick="executeSearch()" 
            style="padding: 15px 35px; background-color: #007bff; color: white; border: none; border-radius: 10px; font-weight: bold; cursor: pointer; font-size: 16px;">
      Tìm kiếm
    </button>
  </div>

  <div id="blog-posts-container">
    {% assign posts = site.posts | sort: "date" | reverse %}
    {% for post in posts %}
    <div class="post-card" style="border: 1px solid #e1e8ed; border-radius: 15px; padding: 25px; margin-bottom: 25px; background: #fff;">
      <span style="color: #007bff; font-weight: bold; font-size: 0.85em;">{{ post.date | date: "%b %d, %Y" }}</span>
      <h3 style="margin: 10px 0;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #1a202c;">{{ post.title }}</a>
      </h3>
      <p style="color: #4a5568;">{{ post.excerpt | strip_html | truncatewords: 25 }}</p>
      <a href="{{ post.url | relative_url }}" style="color: #007bff; font-weight: 600; text-decoration: none;">Đọc thêm →</a>
    </div>
    {% endfor %}
  </div>
</div>

<script>
const postData = [{% for post in site.posts %}{ title: "{{ post.title | escape }}", url: "{{ post.url | relative_url }}" }{% unless forloop.last %},{% endunless %}{% endfor %}];
function handleKeyUp(event) {
    const v = event.target.value.trim();
    document.getElementById('clear-btn').style.display = v ? "block" : "none";
    if (event.key === "Enter") executeSearch(); else showSuggestions(v);
}
function showSuggestions(q) {
    const b = document.getElementById('suggestion-box');
    if (!q) { b.style.display = "none"; return; }
    const m = postData.filter(p => p.title.toLowerCase().includes(q.toLowerCase()));
    if (m.length > 0) {
        b.innerHTML = m.map(p => `<div style="padding: 12px 20px; cursor: pointer; border-bottom: 1px solid #eee;" onclick="location.href='${p.url}'">${p.title}</div>`).join('');
        b.style.display = "block";
    } else { b.style.display = "none"; }
}
function executeSearch() {
    const i = document.getElementById('search-blog').value.trim().toLowerCase();
    if (!i) return;
    const match = postData.find(p => p.title.toLowerCase().includes(i));
    if (match) window.location.href = match.url;
}
function clearSearch() {
    document.getElementById('search-blog').value = "";
    document.getElementById('suggestion-box').style.display = "none";
    document.getElementById('clear-btn').style.display = "none";
}
</script>