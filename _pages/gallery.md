---
title: "Photo Gallery"
description: "Photographs of ikebana arrangements from Sogetsu San Diego Branch workshops, Shoka-Kai classes and flower shows, each linked to the event it comes from."
permalink: /gallery/
layout: archive
author_profile: true
header:
  overlay_color: "#333"
  overlay_filter: 0.5

# The gallery is built automatically: an image in /assets/images/ appears here
# only if some post or recipe actually references it, and each thumbnail links
# back to that event. Adding a post with a `gallery:` entry is all it takes -
# there is no list to maintain here.
#
# This list is the one manual override: images that ARE referenced by a post
# but should not show up as gallery photos (flyers, posters, promo graphics).
exclude_images:
  - SD2023flier.png
  - SogetsuFlyer2025.jpg
  - kika-basket-nov-2025.jpg
---

{%- comment -%}
  Posts and recipes are the two collections that describe events.

  Ordering: newest event first. Posts carry a real `date`, but recipes use
  `date` as a hand-tuned sort key for the /shoka/ listing (it runs backwards
  relative to real time), so each recipe declares a true `event_date`. We sort
  on `event_date` where present and fall back to `date`.

  Liquid cannot sort an array on a computed fallback, so we build one
  "YYYYMMDD~<index>" string per document, sort those, and use the trailing
  index to get back to the document.
{%- endcomment -%}
{%- assign event_docs = site.posts | concat: site.recipes -%}

{%- assign order_rows = '' -%}
{%- for doc in event_docs -%}
  {%- assign doc_date = doc.event_date | default: doc.date -%}
  {%- capture order_rows -%}{{ order_rows }}{{ doc_date | date: "%Y%m%d" }}~{{ forloop.index0 }},{%- endcapture -%}
{%- endfor -%}
{%- assign ordered = order_rows | split: ',' | sort | reverse -%}

{%- assign all_images = site.static_files | where_exp: 'f', 'f.path contains "/assets/images/"' -%}

<div class="gallery-container">
  <div class="gallery-grid">
    {%- comment -%}
      `shown` records which images have already been placed, so a photo used by
      two events appears once, under the newer one.
    {%- endcomment -%}
    {%- assign shown = '' -%}

    {%- for row in ordered -%}
      {%- assign doc_index = row | split: '~' | last | plus: 0 -%}
      {%- assign doc = event_docs[doc_index] -%}
      {%- assign doc_date = doc.event_date | default: doc.date -%}

      {%- for file in all_images -%}
        {%- assign ext = file.extname | remove_first: '.' | downcase -%}
        {%- assign rel = file.path | remove_first: '/assets/images/' -%}

        {%- comment -%} Photos only, and only files directly in /assets/images/. {%- endcomment -%}
        {%- if ext == 'jpg' or ext == 'jpeg' or ext == 'png' -%}
        {%- unless rel contains '/' -%}
        {%- unless page.exclude_images contains file.name -%}

          {%- capture marker -%}|{{ file.path }}|{%- endcapture -%}
          {%- unless shown contains marker -%}

            {%- comment -%}
              Match on the path minus its leading slash ("assets/images/foo.jpg")
              so references written with or without a leading slash both hit,
              while a bare filename cannot collide with a longer one (e.g.
              "so.jpg" must not match "also.jpg").

              Two ways a document can reference an image, and we check both
              because Jekyll's render order is not guaranteed:
                - front matter `gallery:` / `gallery2:` entries
                - anywhere in the rendered body (inline <img>, markdown image,
                  or the output of an {% include gallery %})
            {%- endcomment -%}
            {%- assign needle = file.path | remove_first: '/' -%}
            {%- assign hit = false -%}
            {%- for g in doc.gallery -%}
              {%- if g.image_path contains needle or g.url contains needle -%}{%- assign hit = true -%}{%- endif -%}
            {%- endfor -%}
            {%- for g in doc.gallery2 -%}
              {%- if g.image_path contains needle or g.url contains needle -%}{%- assign hit = true -%}{%- endif -%}
            {%- endfor -%}
            {%- if doc.content contains needle -%}{%- assign hit = true -%}{%- endif -%}

            {%- if hit -%}
              {%- capture shown -%}{{ shown }}{{ marker }}{%- endcapture -%}
              <figure class="gallery-item">
                <a href="{{ file.path | relative_url }}" class="gallery-link"
                   aria-label="Enlarge photo: {{ doc.title | escape }}">
                  <img src="{{ file.path | relative_url }}"
                       alt="{{ doc.title | escape }}"
                       loading="lazy" decoding="async" />
                </a>
                <figcaption class="gallery-item-caption">
                  <a class="gallery-item-event" href="{{ doc.url | relative_url }}">{{ doc.title }}{%- if doc_date %} <span class="gallery-item-date">{{ doc_date | date: "%b %Y" }}</span>{% endif -%}</a>
                </figcaption>
              </figure>
            {%- endif -%}

          {%- endunless -%}
        {%- endunless -%}
        {%- endunless -%}
        {%- endif -%}
      {%- endfor -%}
    {%- endfor -%}
  </div>
</div>

<style>
  /* ------------------------------------------------ */
  /* Gallery Grid Styles                              */
  /* ------------------------------------------------ */
  .gallery-container {
    max-width: 1200px;
    margin: 2rem auto;
    padding: 0 1rem;
  }

  .gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 1.5rem;
  }

  .gallery-item {
    position: relative;
    /* <figure> carries a default margin that would break the grid gutters */
    margin: 0;
    display: flex;
    flex-direction: column;
    background: #fff;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
  }

  .gallery-item:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0,0,0,0.12);
  }

  .gallery-item img {
    width: 100%;
    height: 250px;
    object-fit: cover;
    display: block;
  }

  .gallery-item-caption {
    padding: 0.75rem 1rem 1rem;
    text-align: center;
    /* Push the caption to the bottom so cards in a row line up */
    margin-top: auto;
  }

  /* One link per event that uses this photo (usually exactly one) */
  .gallery-item-event {
    display: block;
    font-size: 0.85rem;
    font-weight: 600;
    line-height: 1.35;
    color: #333;
    text-decoration: none;
  }

  .gallery-item-event + .gallery-item-event {
    margin-top: 0.5rem;
    padding-top: 0.5rem;
    border-top: 1px solid rgba(0,0,0,0.08);
  }

  .gallery-item-event:hover,
  .gallery-item-event:focus-visible {
    text-decoration: underline;
  }

  .gallery-item-date {
    display: block;
    font-size: 0.75rem;
    font-weight: 400;
    color: #666;
  }

  .gallery-link {
    display: block;
    text-decoration: none;
    color: inherit;
  }

  .gallery-link:focus-visible {
    outline: 2px solid #37b41e;
    outline-offset: -2px;
  }

  /* ------------------------------------------------ */
  /* Lightbox Styles                                  */
  /* ------------------------------------------------ */
  .lightbox-overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.9);
    z-index: 9999;
    justify-content: center;
    align-items: center;
  }

  .lightbox-overlay.active {
    display: flex;
  }

  .lightbox-overlay img {
    max-width: 90%;
    max-height: 90%;
    border-radius: 4px;
  }

  .lightbox-close {
    position: absolute;
    top: 20px;
    right: 30px;
    color: #fff;
    font-size: 2rem;
    line-height: 1;
    cursor: pointer;
    z-index: 10000;
    background: none;
    border: 0;
    padding: 0.25rem 0.5rem;
  }

  .lightbox-close:focus-visible {
    outline: 2px solid #fff;
    outline-offset: 2px;
  }

  /* ------------------------------------------------ */
  /* Responsive                                       */
  /* ------------------------------------------------ */
  @media (max-width: 768px) {
    .gallery-grid {
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 1rem;
    }

    .gallery-item img {
      height: 180px;
    }
  }
</style>

<script>
  // ------------------------------------------------
  // Simple Lightbox Functionality
  // ------------------------------------------------
  document.addEventListener('DOMContentLoaded', function() {
    const galleryLinks = document.querySelectorAll('.gallery-link');
    
    // Create lightbox overlay
    const lightbox = document.createElement('div');
    lightbox.className = 'lightbox-overlay';
    lightbox.setAttribute('role', 'dialog');
    lightbox.setAttribute('aria-modal', 'true');
    lightbox.setAttribute('aria-label', 'Enlarged photo');
    lightbox.innerHTML = `
      <button type="button" class="lightbox-close" aria-label="Close photo">&times;</button>
      <img src="" alt="" />
    `;
    document.body.appendChild(lightbox);

    const lightboxImg = lightbox.querySelector('img');
    const lightboxClose = lightbox.querySelector('.lightbox-close');
    let lastFocused = null;

    function openLightbox(link) {
      const img = link.querySelector('img');
      lightboxImg.src = img.src;
      // Carry the thumbnail's alt text over so the enlarged image is described too
      lightboxImg.alt = img.alt || '';
      lastFocused = link;
      lightbox.classList.add('active');
      lightboxClose.focus();
    }

    function closeLightbox() {
      if (!lightbox.classList.contains('active')) return;
      lightbox.classList.remove('active');
      // Release the large image so it is not held in memory
      lightboxImg.removeAttribute('src');
      if (lastFocused) lastFocused.focus();
    }

    // Open lightbox
    galleryLinks.forEach(link => {
      link.addEventListener('click', function(e) {
        e.preventDefault();
        openLightbox(this);
      });
    });

    lightboxClose.addEventListener('click', closeLightbox);

    lightbox.addEventListener('click', function(e) {
      if (e.target === lightbox) closeLightbox();
    });

    // Close on Escape, and keep Tab focus inside the dialog
    document.addEventListener('keydown', function(e) {
      if (!lightbox.classList.contains('active')) return;
      if (e.key === 'Escape') {
        closeLightbox();
      } else if (e.key === 'Tab') {
        e.preventDefault();
        lightboxClose.focus();
      }
    });
  });
</script>