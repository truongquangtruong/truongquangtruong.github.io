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
        <span style="background: #eef6ff; color: #007