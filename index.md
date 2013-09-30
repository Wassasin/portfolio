---
layout: page
title: Hello!
tagline: Supporting tagline
---
{% include JB/setup %}

I am Wouter Geraedts, a graduate student at the [Radboud University Nijmegen](http://www.ru.nl/), in The Netherlands. I am currently working to get my [Mathematical Foundations of Computing Science (MFoCS)](http://www.ru.nl/masters/programme/science/mathematics/specialisations/foundations/) graduate (master's) degree. Professionally I have worked on several websites and mailing systems. Currently I am working on several concepts involving (web)scrapers.

# Posts

{% if site.posts == empty %}
I have yet to post something.
{% else %}
<ul class="posts">
  {% for post in site.posts %}
	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
{% endif %}
