---
layout: page
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
    <li>
      <article>
        <h1><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h1>
        <span class="date">{{ post.date | date_to_string }}</span> 
        <p>
          {{ post.content | strip_html | truncatewords: 50 }}
        </p>
        <a class="more" href="{{ BASE_PATH }}{{ post.url }}">Read more</a>
      </article>
    </li>
  {% endfor %}
</ul>
