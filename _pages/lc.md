---
layout: default
permalink: /leetcode
title: LeetCode
nav: true
nav_order: 3
pagination:
  enabled: true
  collection: leetcode
  permalink: /page/:num/
  per_page: 5
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="leetcode">

{% assign postlist = site.leetcode %}

{% for post in postlist %}
{% assign read_time = post.feed_content | strip_html | number_of_words | divided_by: 180 | plus: 1 %}
{% assign year = post.date | date: "%Y" %}
{% assign tags = post.tags | join: "" %}
{% assign categories = post.categories | join: "" %}



</div>