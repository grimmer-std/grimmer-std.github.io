---
layout: page
title: Tagi 
---

<div class="page-content wc-container">
	<div class="post">
		<h1>Tagi</h1>  
		<ul>
			{% for tag in site.tags %}
			<li><a href="{{ '/tag/' | append:tag[0] | relative_url }}">{{ tag[0] }}</a></li>
			{% endfor %}
		</ul>
	</div>
</div>
