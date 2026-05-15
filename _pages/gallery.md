---
title: "Photo Gallery"
layout: archive
permalink: /gallery/
author_profile: true
header:
  overlay_color: "#333"
  overlay_filter: 0.5
---

{% assign caption_prefix = "SDBranch_" %}

<div class="gallery-container">
  <div class="gallery-grid">
    {% assign all_images = site.static_files | where_exp: 'file', 'file.path contains "assets/images"' %}
    
    {% for file in all_images %}
      {% assign ext = file.name | split: '.' | last | downcase %}
      {% assign is_image = false %}
      {% assign is_excluded = false %}
      
      {% comment %} Check if file extension is valid {% endcomment %}
      {% if ext == 'jpg' or ext == 'jpeg' or ext == 'png' %}
        {% assign is_image = true %}
      {% endif %}
      
      {% comment %} Check excluded files {% endcomment %}
      {% if file.name == 'zzz.png' or file.name == 'zzz2.png' or file.name == 'kika17.jpg' or file.name == '2.22.2018-Sogetsu-School.jpg' %}
        {% assign is_excluded = true %}
      {% endif %}
      
      {% comment %} Only show if: valid image + not excluded {% endcomment %}
      {% if is_image == true and is_excluded == false %}
        {% assign clean_name = file.name | replace: '.jpg', '' | replace: '.jpeg', '' | replace: '.png', '' | replace: '-', ' ' | replace: '_', ' ' %}
        <div class="gallery-item">
          <a href="{{ file.path }}" class="gallery-link" data-lightbox="gallery">
            <img src="{{ file.path }}" alt="{{ file.name }}" loading="lazy" />
            <div class="gallery-item-caption">
              <span class="gallery-item-name">{{ caption_prefix }}{{ clean_name | capitalize }}</span>
            </div>
          </a>
        </div>
      {% endif %}
    {% endfor %}
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
    padding: 1rem;
    text-align: center;
  }

  .gallery-item-name {
    font-size: 0.85rem;
    font-weight: 500;
    color: #333;
  0.9rem;
  }

  .gallery-link {
    text-decoration: none;
    color: inherit;
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
    cursor: pointer;
    z-index: 10000;
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
    lightbox.innerHTML = `
      <span class="lightbox-close">&times;</span>
      <img src="" alt="Full size image" />
    `;
    document.body.appendChild(lightbox);

    const lightboxImg = lightbox.querySelector('img');
    const lightboxClose = lightbox.querySelector('.lightbox-close');

    // Open lightbox
    galleryLinks.forEach(link => {
      link.addEventListener('click', function(e) {
        e.preventDefault();
        const imgSrc = this.querySelector('img').src;
        lightboxImg.src = imgSrc;
        lightbox.classList.add('active');
      });
    });

    // Close lightbox
    lightboxClose.addEventListener('click', function() {
      lightbox.classList.remove('active');
    });

    lightbox.addEventListener('click', function(e) {
      if (e.target === lightbox) {
        lightbox.classList.remove('active');
      }
    });

    // Close on Escape key
    document.addEventListener('keydown', function(e) {
      if (e.key === 'Escape') {
        lightbox.classList.remove('active');
      }
    });
  });
</script>