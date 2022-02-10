---
layout: splash
# TODO: figure out what this value can be
entries_layout: grid
# author_profile: true
title: On This Day in Tech
header:
  overlay_image: "/assets/images/faye-cornish-ywRNdDfvMWs-unsplash.jpg"
  overlay_filter: "0.5"
#   actions:
#     - label: "Download"
#       url: "https://github.com/mmistakes/minimal-mistakes/"
#   caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
excerpt: "A collection of the things I've learned"
intro:
  - excerpt: >-
      Hi! I'm Addison, an aspiring developer and software engineer.
      <br>
      This is a collection of my ruminations and musings on the topics I've studied on my journey in tech.
---

{% include feature_row id="intro" type="left" %}

<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>

{% if paginator %}
  {% assign posts = paginator.posts %}
{% else %}
  {% assign posts = site.posts %}
{% endif %}

{% assign entries_layout = page.entries_layout | default: 'list' %}
<div class="entries-{{ entries_layout }}">
  {% for post in posts %}
    {% include archive-single.html type=entries_layout %}
  {% endfor %}
</div>

{% include paginator.html %}
