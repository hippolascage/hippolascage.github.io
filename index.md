---
layout: default
---

<div class="posts">
  {% for post in site.posts %}
    <article class="post">
      
      <div class="entry">
        {{ post.content }}
      </div>

    </article>
    <hr class="article-break">
    
  {% endfor %}
</div>
