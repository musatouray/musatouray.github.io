---
layout: single
title: Projects
permalink: /projects/
author_profile: true
header:
  overlay_color: "#000"
  overlay_filter: "0.2"
  overlay_image: /assets/images/hero-image.jpg
excerpt: "Data engineering and analytics projects."
classes: wide
---

<p class="projects-intro">Explore my data engineering and analytics projects. Each project demonstrates my own practical learnings and reflections.</p>

<div class="projects-grid" id="projects-grid">
  {% assign all_projects = site.portfolio | sort: "date" | reverse %}
  {% for project in all_projects %}
  <div class="project-card" data-index="{{ forloop.index }}">
    <a href="{{ project.url | relative_url }}" class="project-card__image-link">
      {% if project.header.teaser %}
      <img src="{{ project.header.teaser }}" alt="{{ project.title }}" loading="lazy">
      {% else %}
      <div class="project-card__no-image"></div>
      {% endif %}
    </a>
    <div class="project-card__body">
      {% if project.tags.size > 0 %}
      <div class="project-card__tags">
        {% for tag in project.tags limit:2 %}
        <span class="project-tag">{{ tag }}</span>
        {% endfor %}
      </div>
      {% endif %}
      <h3 class="project-card__title">
        <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      </h3>
      <p class="project-card__excerpt">{{ project.excerpt | strip_html | truncate: 120 }}</p>
      <a href="{{ project.url | relative_url }}" class="project-card__link">View Project &rarr;</a>
    </div>
  </div>
  {% endfor %}
</div>

<div class="load-more-container" id="load-more-container" style="display:none;">
  <button class="btn btn--primary load-more-btn" id="load-more-btn">Load More</button>
</div>

<script>
(function () {
  var ITEMS_PER_PAGE = 12;
  var cards = Array.from(document.querySelectorAll('.project-card'));
  var loadMoreBtn = document.getElementById('load-more-btn');
  var loadMoreContainer = document.getElementById('load-more-container');
  var shown = ITEMS_PER_PAGE;

  function updateVisibility() {
    cards.forEach(function (card, i) {
      card.style.display = i < shown ? '' : 'none';
    });
    if (shown >= cards.length) {
      loadMoreContainer.style.display = 'none';
    } else {
      loadMoreContainer.style.display = 'flex';
    }
  }

  if (cards.length > ITEMS_PER_PAGE) {
    loadMoreBtn.addEventListener('click', function () {
      shown = Math.min(shown + ITEMS_PER_PAGE, cards.length);
      updateVisibility();
    });
    updateVisibility();
  }
})();
</script>
