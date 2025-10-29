---
layout: page
title: "分类"
permalink: /categories/
---

<div class="categories-page">
  <h1>所有分类</h1>
  
  {% assign sorted_categories = site.categories | sort %}
  {% for category in sorted_categories %}
    <div class="category-group">
      <h2 id="{{ category[0] | slugify }}">{{ category[0] }}</h2>
      <ul class="category-posts">
        {% for post in category[1] %}
          <li>
            <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
            <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
            <div class="post-tags">
              {% for tag in post.tags %}
                <span class="tag">{{ tag }}</span>
              {% endfor %}
            </div>
          </li>
        {% endfor %}
      </ul>
    </div>
  {% endfor %}
</div>

<style>
.categories-page {
  max-width: 800px;
  margin: 0 auto;
}

.category-group {
  margin-bottom: 2rem;
}

.category-group h2 {
  color: #159957;
  border-bottom: 1px solid #eaecef;
  padding-bottom: 0.5rem;
}

.category-posts {
  list-style: none;
  padding: 0;
}

.category-posts li {
  padding: 1rem 0;
  border-bottom: 1px solid #f1f1f1;
}

.category-posts li:last-child {
  border-bottom: none;
}

.post-date {
  color: #666;
  font-size: 0.9em;
  margin-left: 1rem;
}

.post-tags {
  margin-top: 0.5rem;
}

.tag {
  display: inline-block;
  background-color: #f1f8ff;
  color: #0366d6;
  padding: 0.2rem 0.5rem;
  border-radius: 3px;
  font-size: 0.8em;
  margin-right: 0.5rem;
}

.category-posts a {
  color: #0366d6;
  text-decoration: none;
  font-weight: 500;
}

.category-posts a:hover {
  text-decoration: underline;
}
</style>
