---
layout: single
title: "Musings"
permalink: /musings/
author_profile: true
breadcrumbs: false
header:
  overlay_color: "#f0f4f8"
  overlay_filter: "1"
excerpt: "Data and AI reflections, techniques, and lessons learned."
classes: wide light-hero
---

<!-- Filter bar -->
<div class="projects-filter" id="musings-filter">
  <button class="filter-btn active" data-filter="all">All</button>
  <button class="filter-btn" data-filter="power-bi">
    <img src="/assets/images/power_bi.png" alt="">Power BI
  </button>
  <button class="filter-btn" data-filter="fabric">
    <img src="/assets/images/fabric.png" alt="">Fabric
  </button>
  <button class="filter-btn" data-filter="notebook">
    <img src="/assets/images/notebook.png" alt="">Notebook
  </button>
  <button class="filter-btn" data-filter="dax">DAX</button>
  <button class="filter-btn" data-filter="python">Python</button>
</div>

<div class="musings-grid" id="musings-grid">
  {% assign musings_posts = site.posts | where_exp: "post", "post.categories contains 'Musings'" %}
  {% for post in musings_posts %}
  {% assign words = post.content | number_of_words %}
  {% assign mins = words | divided_by: 200 %}
  {% if mins < 1 %}{% assign mins = 1 %}{% endif %}
  {% assign tag_slugs = "" %}
  {% for cat in post.categories %}
    {% unless cat == "Musings" %}
    {% assign slug = cat | downcase | replace: " ", "-" | replace: "_", "-" %}
    {% assign tag_slugs = tag_slugs | append: slug | append: " " %}
    {% endunless %}
  {% endfor %}
  {% for tag in post.tags %}
    {% assign slug = tag | downcase | replace: " ", "-" | replace: "_", "-" %}
    {% assign tag_slugs = tag_slugs | append: slug | append: " " %}
  {% endfor %}
  <div class="musing-card" data-tags="{{ tag_slugs | strip }}">
    <a href="{{ post.url | relative_url }}" class="musing-card__image-link">
      {% if post.header.teaser %}
      <img src="{{ post.header.teaser }}" alt="{{ post.title }}" loading="lazy">
      {% else %}
      <div class="musing-card__no-image"></div>
      {% endif %}
    </a>
    <div class="musing-card__body">
      <h3 class="musing-card__title">
        <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
      <p class="musing-card__meta">
        <span>{{ post.date | date: "%b %d, %Y" }}</span>
        <span>&bull; {{ mins }} min read</span>
      </p>
      <p class="musing-card__excerpt">{{ post.excerpt | strip_html | truncate: 130 }}</p>
    </div>
  </div>
  {% endfor %}
  <p class="no-projects-message" id="no-musings-msg" style="display:none;">No articles found for this filter.</p>
</div>

<div class="load-more-container" id="musings-load-more-container" style="display:none;">
  <button class="btn btn--primary load-more-btn" id="musings-load-more-btn">Load More</button>
</div>

<script>
(function () {
  var ITEMS_PER_PAGE = 12;
  var currentFilter = 'all';
  var cards = Array.from(document.querySelectorAll('.musing-card'));
  var filterBtns = Array.from(document.querySelectorAll('#musings-filter .filter-btn'));
  var loadMoreBtn = document.getElementById('musings-load-more-btn');
  var loadMoreContainer = document.getElementById('musings-load-more-container');
  var noMsg = document.getElementById('no-musings-msg');
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
    filtered.slice(0, shown).forEach(function (c) { c.style.display = ''; });
    noMsg.style.display = filtered.length === 0 ? '' : 'none';
    loadMoreContainer.style.display = shown < filtered.length ? 'flex' : 'none';
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
