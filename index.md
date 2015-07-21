---
layout: default
---

<div class="home">


<h1> Notes </h1>


  <ul class="posts">
    {% for page in site.pages %}
      <li>
      {% unless page.categories contains 'navigation' %}
        <a class="post-link" href="{{ page.url | prepend: site.baseurl }}">{{ page.title }}</a>
      {%endunless %}
      </li>
    {% endfor %}
  </ul>

  <h1>Posts</h1>

  <ul class="posts">
    {% for post in site.posts %}
      <li>
        <span class="post-date">{{ post.date | date: "%b %-d, %Y" }}</span>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </li>
    {% endfor %}
  </ul>

  <p class="rss-subscribe">subscribe <a href="{{ "/feed.xml" | prepend: site.baseurl }}">via RSS</a></p>

</div>
