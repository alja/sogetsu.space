---
title: "Program and Activities"
description: "Sogetsu School San Diego Branch - a community of Sogetsu ikebana teachers and students in San Diego County. Bimonthly workshops, Shoka-Kai classes with Ms. Kika Shibata, and biannual flower shows in Balboa Park."
layout: splash
permalink: /
hidden: true
header:
  overlay_color: "#37b41e"
  overlay_image: /assets/images/zzz2.png
  overlay_filter: "rgba(55, 180, 30, 0.5)"
  actions:
    - label: "Event Program"
      url: "/year-archive/"
excerpt: >
    2026 Dates:<br>Bimonthly workshops: <b>2/19, 4/30, 6/18, 8/20, 10/22, 12/17 </b><br>Shoka Kai: <b>3/9, 5/11, 9/14, 11/9</b><br>

feature_row:
  - image_path: /assets/images/sogetsusto.jpg
    title: "100 Years of Sogetsu"
    url: "https://www.sogetsu.or.jp/e/events/hq-org/33339/"
    btn_label: "April 18th, 2027"
    btn_size: "small"
gallery:
  - url: /assets/images/yu2.png
    image_path: /assets/images/yu2.png
    alt: "Ikebana arrangement by branch director Yumi Rakers"
    title: "Arrangement by branch director Yumi Rakers"
  - url: /assets/images/sfront3.jpg
    image_path: /assets/images/sfront3.jpg
    alt: "Group ikebana arrangement with Hiroko Szechinski"
    title: "Group arrangement with Hiroko Szechinski"
  - url: /assets/images/shoka-nov-dry.jpg
    image_path: /assets/images/shoka-nov-dry.jpg
    alt: "Kika Shibata demonstrating a dried-material arrangement"
    title: "Kika Shibata demonstration at Shoka Kai classes"
  
---

<div>
  <a href="{{ '/gallery/' | relative_url }}">
    <strong>Events Photo Gallery</strong>
  </a>
</div>

{% assign latest_post = site.posts | first %}
{% assign event = latest_post.event %}
{% if event %}
<div class="latest-event" style="margin-bottom: 2rem;">
  <h2>
    <a href="{{ latest_post.url | relative_url }}">{{ event.title }}</a>
  </h2>
  <p>
    🗓 {% if event.all_day and event.end and event.end != event.start %}{{ event.start | date: "%B %-d" }} – {{ event.end | date: "%B %-d, %Y" }}{% else %}{{ event.start | date: "%B %-d, %Y" }}{% unless event.all_day %}, {{ event.start | date: "%-l:%M %p" }} – {{ event.end | date: "%-l:%M %p" }}{% endunless %}{% endif %}<br>
    📍 {{ event.location }}
  </p>
  {% include add-to-calendar.html event=event %}
</div>
{% endif %}
{% include gallery %}
{% include feature_row id="feature_row" type="center" %}