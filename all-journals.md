---
layout: default
---

# Journal

<ol>
  {% for post in site.posts reversed %}
    <li>
      <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ol>
