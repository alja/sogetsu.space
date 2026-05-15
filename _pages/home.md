---
title: "Program and Activities"
layout: splash
permalink: /
hidden: true
header:
  overlay_color: "sound_color"
  overlay_image: /assets/images/zzz2.png
  overlay_filter: "rgba(55, 180, 30, 0.5)"
  actions:
    - label: "Event Program"
      url: "/year-archive/"
excerpt: >
    2026 Dates:<br>Bimonthly workshops: <b>2/19, 4/30, 6/18, 8/20, 12/17 </b><br>Shoka Kai: <b>3/9, 5/11, 9/14, 11/9</b><br>

intro: 
feature_row:
  - image_path: /assets/images/sogetsusto.jpg
    title: "100 Years of Sogetsu"
    url: "https://www.sogetsu.or.jp/e/events/hq-org/33339/"
    btn_label: "April 18th, 2027"
    btn_size: "small"
gallery:
  - url: /assets/images/yu2.png
    image_path: /assets/images/yu2.png
    alt: "placeholder image 1"
    title: "Arrangement by branch director Yumi Rakers"
  - url: /assets/images/sfront3.jpg
    image_path: /assets/images/sfront3.jpg
    alt: "placeholder image 2"
    title: "Group arrangement with Hiroko Szechinski"
  - url: /assets/images/shoka-nov-dry.jpg
    image_path: /assets/images/shoka-nov-dry.jpg
    alt: "Kika Shibata"
    title: "Kika Shibata demonstration at Shoka Kai classes"
  
---
{% assign latest_post = site.posts | first %}
{% assign event = latest_post.event %}
{% if event %}
<div class="latest-event" style="margin-bottom: 2rem;">

  <h2>Latest Workshop: {{ event.title }}</h2>
  <p>
    🗓 {{ event.start | date: "%B %d, %Y" }}<br>
    📍 {{ event.location }}
  </p>
  {% assign start_utc = event.start | date: "%Y%m%dT%H%M%SZ" %}
  {% assign end_utc   = event.end   | date: "%Y%m%dT%H%M%SZ" %}
  <h2>
  <a href="{{ latest_post.url | relative_url }}">
    {{ event.title }}
  </a>
</h2>
<a href="https://calendar.google.com/calendar/render?action=TEMPLATE&text={{ event.title | uri_escape }}&dates={{ start_utc }}/{{ end_utc }}&details={{ event.description | uri_escape }}&location={{ event.location | uri_escape }}"
     target="_blank">
  </a>
</div>
{% endif %}
{% include gallery %}
{% include feature_row id="intro" type="center" %}
{% include feature_row id="feature_row" type="center" %}