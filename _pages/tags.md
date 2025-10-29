---
layout: page
title: "标签"
permalink: /tags/
---

<div class="tags-page">
  <h1>所有标签</h1>
  
  {% assign sorted_tags = site.tags | sort %}
  {% for tag in sorted_tags %}
    <div class="tag-group">
      <h2 id="{{ tag[0] | slugify }}">{{ tag[0] }}</h2>
      <ul class="tag-posts">
        {% for post in tag[1] %}
          <li>
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
            <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
          </li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>

<style>
.tags-page {
  max-width: 800px;
  margin: 0 auto;
}

.tag-group {
  margin-bottom: 2rem;
}

.tag-group h2 {
  color: #159957;
  border-bottom: 1px solid #eaecef;
  padding-bottom: 0.5rem;
}

.tag-posts {
  list-style: none;
  padding: 0;
}

.tag-posts li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem 0;
  border-bottom: 1px solid #f1f1f1;
}

.tag-posts li:last-child {
  border-bottom: none;
}

.post-date {
  color: #666;
  font-size: 0.9em;
}

.tag-posts a {
  color: #0366d6;
  text-decoration: none;
}

.tag-posts a:hover {
  text-decoration: underline;
}
</style>
