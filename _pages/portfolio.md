---
layout: single
title: Projects
permalink: /projects/
author_profile: true
breadcrumbs: false
header:
  overlay_color: "#f0f4f8"
  overlay_filter: "1"
excerpt: "Explore my data engineering and analytics projects. Each project demonstrates my own practical learnings and reflections."
classes: wide light-hero
---

<!-- Filter bar -->
<div class="projects-filter" id="projects-filter">
  <button class="filter-btn active" data-filter="all">All</button>
  <button class="filter-btn" data-filter="power-bi">
    <img src="/assets/images/power_bi.png" alt="">Power BI
  </button>
  <button class="filter-btn" data-filter="fabric">
    <img src="/assets/images/fabric.png" alt="">Fabric
  </button>
  <button class="filter-btn" data-filter="lakehouse">
    <img src="/assets/images/lakehouse.png" alt="">Lakehouse
  </button>
  <button class="filter-btn" data-filter="pipeline">
    <img src="/assets/images/pipeline.png" alt="">Pipeline
  </button>
  <button class="filter-btn" data-filter="notebook">
    <img src="/assets/images/notebook.png" alt="">Notebook
  </button>
  <button class="filter-btn" data-filter="semantic-model">
    <img src="/assets/images/semantic_model.png" alt="">Semantic Model
  </button>
  <button class="filter-btn" data-filter="report">
    <img src="/assets/images/report.png" alt="">Report
  </button>
</div>

<div class="projects-grid" id="projects-grid">
  {% assign all_projects = site.portfolio | sort: "date" | reverse %}
  {% for project in all_projects %}
  {% assign tag_slugs = "" %}
  {% for tag in project.tags %}
    {% assign slug = tag | downcase | replace: " ", "-" | replace: "_", "-" %}
    {% assign tag_slugs = tag_slugs | append: slug | append: " " %}
  {% endfor %}
  <div class="project-card" data-index="{{ forloop.index }}" data-tags="{{ tag_slugs | strip }}">
    <a href="{{ project.url | relative_url }}" class="project-card__image-link">
      {% if project.header.teaser %}
      <img src="{{ project.header.teaser }}" alt="{{ project.title }}" loading="lazy">
      {% else %}
      <div class="project-card__no-image"></div>
      {% endif %}
    </a>
    <div class="project-card__body">
      <h3 class="project-card__title">
        <a href="{{ project.url | relative_url }}">{{ project.title }}</a>
      </h3>
      <p class="project-card__excerpt">{{ project.excerpt | strip_html | truncate: 90 }}</p>
      <a href="{{ project.url | relative_url }}" class="project-card__link">View Project &rarr;</a>
    </div>
  </div>
  {% endfor %}
  <p class="no-projects-message" id="no-projects-msg" style="display:none;">No projects found for this filter.</p>
</div>

<div class="load-more-container" id="load-more-container" style="display:none;">
  <button class="btn btn--primary load-more-btn" id="load-more-btn">Load More</button>
</div>

<script>
(function () {
  var ITEMS_PER_PAGE = 12;
  var currentFilter = 'all';
  var cards = Array.from(document.querySelectorAll('.project-card'));
  var filterBtns = Array.from(document.querySelectorAll('.filter-btn'));
  var loadMoreBtn = document.getElementById('load-more-btn');
  var loadMoreContainer = document.getElementById('load-more-container');
  var noMsg = document.getElementById('no-projects-msg');
  var shown = ITEMS_PER_PAGE;

  function getFiltered() {
    if (currentFilter === 'all') return cards;
    return cards.filter(function (c) {
      var tags = (c.getAttribute('data-tags') || '').split(' ');
      return tags.indexOf(currentFilter) !== -1;
    });
  }

  function render() {
    var filtered = getFiltered();
    cards.forEach(function (c) { c.style.display = 'none'; });
    var visible = filtered.slice(0, shown);
    visible.forEach(function (c) { c.style.display = ''; });
    noMsg.style.display = filtered.length === 0 ? '' : 'none';
    if (shown >= filtered.length) {
      loadMoreContainer.style.display = 'none';
    } else {
      loadMoreContainer.style.display = 'flex';
    }
  }

  filterBtns.forEach(function (btn) {
    btn.addEventListener('click', function () {
      currentFilter = btn.getAttribute('data-filter');
      shown = ITEMS_PER_PAGE;
      filterBtns.forEach(function (b) { b.classList.remove('active'); });
      btn.classList.add('active');
      render();
    });
  });

  if (loadMoreBtn) {
    loadMoreBtn.addEventListener('click', function () {
      shown += ITEMS_PER_PAGE;
      render();
    });
  }

  render();
})();
</script>
