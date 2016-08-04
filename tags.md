---
layout: default
---

{% include header.html %}

<!-- Get the tag name for every tag on the site and set them
to the `site_tags` variable. -->
{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}

<!-- `tag_words` is a sorted array of the tag names. -->
{% assign tag_words = site_tags | split:',' %}

Tags:

<!-- List of all tags -->
<!-- <ul class="tags">
  {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tag_words[item] }}{% endcapture %}
	<li>
	      <a href="#{{ this_word | cgi_escape }}" class="tag">{{ this_word }}
	        <span>({{ site.tags[this_word].size }})</span>
	      </a>
	    </li>
  {% endunless %}{% endfor %}
</ul> -->

<!-- Newer way  -->
{% capture site_tags %}{% for tag in site.tags %}{{ tag[1].size }}#{{ tag | first | downcase }}#{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign tag_hashes = site_tags | split:',' | sort %}
<ul class="list-group">
{% for hash in tag_hashes reversed %}
  {% assign keyValue = hash | split: '#' %}
  {% capture tag_word %}{{ keyValue[2] | strip_newlines }}{% endcapture %}

    <a href="/tags/{{ tag_word }}">
      {{tag_word}}<span class="badge pull-right">({{ site.tags[tag_word].size }})</span>
    </a>

{% endfor %}
</ul>

---
<!-- Posts by Tag -->
<div>
  {% for item in (0..site.tags.size) %}{% unless forloop.last %}
    {% capture this_word %}{{ tag_words[item] }}{% endcapture %}
    <h1 id="{{ this_word | cgi_escape }}">{{ this_word }}</h1>
    {% for post in site.tags[this_word] %}{% if post.title != null %}
      <div>
	  <ul>
	  	<li><span class="date">{{ post.date | date: '%Y %b %d' }}</span> - <a href="{{ post.url }}">{{ post.title }}</a></li>
	  </ul>
      </div>
      <div style="clear: both;"></div>
    {% endif %}{% endfor %}
	<br>
  {% endunless %}{% endfor %}
</div>
