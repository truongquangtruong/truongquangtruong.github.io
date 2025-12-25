---
layout: page
title: "Giới thiệu"
permalink: /about/ 
---

<div style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; max-width: 900px; margin: 0 auto;">
  <p style="color: #666; font-style: italic; margin-bottom: 30px; text-align: center;">
    "Hành trình thực nghiệm chuyên sâu: Từ hạ tầng mạng vững chắc đến nghệ thuật kết nối, tối ưu hóa dữ liệu liên tầng và thiết lập lá chắn bảo mật hệ thống phân tán hiện đại."
  </p>

  <div style="display: flex; margin-bottom: 40px; position: relative; box-shadow: 0 4px 12px rgba(0,0,0,0.05); border-radius: 10px;">
    
    <div style="position: relative; flex: 1;">
      <input type="text" id="search-blog" placeholder="Tìm kiếm bài học..." 
             onkeyup="handleKeyUp(event)"
             style="width: 100%; padding: 15px 45px 15px 20px; border: 2px solid #007bff; border-right: none; border-radius: 10px 0 0 10px; font-size: 16px; outline: none; box-sizing: border-box;">
      
      <span onclick="clearSearch()" id="clear-btn" 
            style="position: absolute; right: 15px; top: 50%; transform: translateY(-50%); cursor: pointer; color: #a0aec0; font-weight: bold; display: none; user-select: none;">✕</span>
    </div>

    <button onclick="executeSearch()" 
            style="padding: 0 30px; background-color: #007bff; color: white; border: 2px solid #007bff; border-radius: 0 10px 10px 0; font-weight: bold; cursor: pointer; font-size: 16px; transition: 0.3s;">
      Tìm
    </button>

    <div id="suggestion-box" 
         style="display: none; position: absolute; width: calc(100% - 85px); background: white; border: 1px solid #e1e8ed; border-radius: 0 0 8px 8px; z-index: 1000; box-shadow: 0 10px 25px rgba(0,0,0,0.1); top: 52px; max-height: 250px; overflow-y: auto;">
    </div>
  </div>

  <div id="blog-posts-container" style="display: flex; flex-direction: column; gap: 25px;">
    {% assign posts = site.posts | sort: "weight" %}
    {% for post in posts %}
    <div class="post-card" style="border: 1px solid #e1e8ed; border-radius: 12px; padding: 20px; transition: 0.3s; background: #fff;" 
         onmouseover="this.style.boxShadow='0 5px 15px rgba(0,0,0,0.08)'; this.style.borderColor='#007bff'" 
         onmouseout="this.style.boxShadow='none'; this.style.borderColor='#e1e8ed'">
      <span style="color: #007bff; font-weight: bold; font-size: 0.85em; text-transform: uppercase;">{{ post.date | date: "%b %d, %Y" }}</span>
      <h3 style="margin: 10px 0;">
        <a href="{{ post.url | relative_url }}" style="text-decoration: none; color: #1a202c;">{{ post.title }}</a>
      </h3>
      <p style="color: #4a5568; font-size: 0.95em; line-height: 1.6;">
        {{ post.excerpt | strip_html | truncatewords: 30 }}
      </p>
      <a href="{{ post.url | relative_url }}" style="color: #007bff; font-weight: 600; text-decoration: none; font-size: 0.9em;">Đọc</a>
    </div>
    {% endfor %}
  </div>
</div>

<script>
// 1. Khởi tạo dữ liệu từ Jekyll
const postData = [
  {% for post in site.posts %}
    { 
        title: "{{ post.title | escape }}", 
        url: "{{ post.url | relative_url }}" 
    }{% unless forloop.last %},{% endunless %}
  {% endfor %}
];

// 2. Xử lý gõ phím (Hiển thị gợi ý & Enter)
function handleKeyUp(event) {
    const value = event.target.value.trim();
    const clearBtn = document.getElementById('clear-btn');
    
    clearBtn.style.display = value ? "block" : "none";
    
    if (event.key === "Enter") {
        executeSearch();
    } else {
        showSuggestions(value);
    }
}

// 3. Hiển thị danh sách gợi ý
function showSuggestions(query) {
    const suggestionBox = document.getElementById('suggestion-box');
    if (!query) {
        suggestionBox.style.display = "none";
        return;
    }

    const matches = postData.filter(p => p.title.toLowerCase().includes(query.toLowerCase()));
    
    if (matches.length > 0) {
        suggestionBox.innerHTML = matches.map(p => `
            <div style="padding: 12px 20px; cursor: pointer; border-bottom: 1px solid #f0f0f0; font-size: 14px;" 
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

// 4. Thực thi tìm kiếm (Nút bấm hoặc Enter)
function executeSearch() {
    const input = document.getElementById('search-blog').value.trim().toLowerCase();
    if (!input) return;

    // Ưu tiên 1: Khớp tiêu đề 100% (Cho hành động copy-paste)
    const exactMatch = postData.find(p => p.title.toLowerCase() === input);
    if (exactMatch) {
        window.location.href = exactMatch.url;
        return;
    }

    // Ưu tiên 2: Nhảy vào kết quả tương ứng đầu tiên
    const firstMatch = postData.filter(p => p.title.toLowerCase().includes(input))[0];
    if (firstMatch) {
        window.location.href = firstMatch.url;
    } else {
        alert("Không tìm thấy bài học nào!");
    }
}

// 5. Xóa ô tìm kiếm
function clearSearch() {
    const input = document.getElementById('search-blog');
    input.value = "";
    input.focus();
    document.getElementById('suggestion-box').style.display = "none";
    document.getElementById('clear-btn').style.display = "none";
}

// Đóng gợi ý khi click ra ngoài
document.addEventListener('click', function(e) {
    if (!e.target.closest('#search-blog') && !e.target.closest('#suggestion-box')) {
        document.getElementById('suggestion-box').style.display = "none";
    }
});
</script>