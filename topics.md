---
layout: default
title: Topics
permalink: /topics/
---

<h1>📁 The Directory</h1>
<p class="small">Every post on the site, organized the way we organized the
web before search engines got good: by hand, into folders.</p>

{% for topic in site.data.topics %}
<section id="{{ topic.slug }}">
  <h2><a href="{{ '/topics/' | append: topic.slug | append: '/' | relative_url }}">{{ topic.name }}</a></h2>
  <p class="small">{{ topic.blurb }}</p>
  {% assign topic_posts = site.posts | where_exp: "post", "post.tags contains topic.slug" %}
  {% if topic_posts.size > 0 %}
  <table class="post-table">
    {% for post in topic_posts %}
    <tr>
      <td class="pt-date">{{ post.date | date: "%m/%d/%y" }}</td>
      <td><div class="pt-title"><a href="{{ post.url | relative_url }}">{{ post.title }}</a></div></td>
    </tr>
    {% endfor %}
  </table>
  {% else %}
  <div class="constructo"><span>🚧 UNDER CONSTRUCTION 🚧</span></div>
  {% endif %}
</section>
{% endfor %}
