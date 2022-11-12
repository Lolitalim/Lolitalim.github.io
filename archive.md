---
layout: page
title: Blog Archive

---

{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.date | date: "%B %Y" }} - {{ post.title }}</a></li>
    {% endfor %}
  </ul>
  
  <h3>{{ tag[1] }}</h3>
  <u1>
    {% for post in tag[2] %}
       <li><a href="{{ post.url }}">{{ post.date | date: "%B %Y" }} - {{ post.title }}</a></li>
    {% endfor %}
  </u1>
{% endfor %}
