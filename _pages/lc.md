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
  per_page: 10
  sort_field: date
  sort_reverse: true
  trail:
    before: 1 # The number of links before the current page
    after: 3 # The number of links after the current page
---

<div class="leetcode">

{% assign notelist = paginator.leetcode %}

{% for note in notelist %}
{% assign read_time = post.content | number_of_words | divided_by: 180 | plus: 1 %}
{% assign year = note.date | date: "%Y" %}
{% assign tags = note.tags | join: "" %}
{% endfor %}

</div>