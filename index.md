---
layout: default
#title: Mayank Patel
#description: Hello! I’m a front-end engineer who is passionate about developing rich web applications using HTML5, CSS3 & Javascript.
---
{% include JB/setup %}

<ul id='timeline'>
	{% for post in site.posts %}
	<li class='work'>
		<input class='radio' id='{{ forloop.index }}' name='works' type='radio' />

		<div class="relative">
			<label for='{{ forloop.index }}'>{{ post.title }}</label>
			<span class='date'>{{ post.date|| date: "%d-%b-%Y" }}</span>
			<span class='circle'></span>
		</div>
		<div class='content'>
			<p>
				{{ post.description }}
			<button onclick="window.location.href='{{ post.url }}'" class="read-more">read more</button>
			</p>
		</div>
	</li>
	{% endfor %}
</ul>
