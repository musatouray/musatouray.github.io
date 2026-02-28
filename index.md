---
layout: single
title: "Musa Touray"
classes: wide home-page
author_profile: false
---

<section class="home-hero">

  <!-- Centred profile content -->
  <div class="home-hero__inner">
    <h1 class="home-hero__name">Hi, I'm <span>Musa</span></h1>
    <img src="/assets/images/profile-image.jpg" alt="Musa Touray" class="home-hero__avatar">
    <div class="home-hero__roles">
      <span>Data Engineer</span>
      <span>Power BI Developer</span>
      <span>Microsoft Fabric</span>
    </div>
    <p class="home-hero__bio">Data Engineer &amp; Analytics Engineer. Microsoft Certified Fabric and Power BI specialist building enterprise data solutions across the Azure ecosystem.</p>
    <p class="home-hero__tagline">Learn, Apply, Repeat.</p>
    <div class="home-hero__divider"></div>
    <div class="home-hero__location">
      <i class="fas fa-map-marker-alt" aria-hidden="true"></i>
      New York, USA
    </div>
    <div class="home-hero__social">
      <a href="https://www.linkedin.com/in/musa-touray/" class="home-hero__social-link" target="_blank" rel="noopener noreferrer">
        <i class="fab fa-linkedin" aria-hidden="true"></i><span>LinkedIn</span>
      </a>
      <a href="https://github.com/musatouray" class="home-hero__social-link" target="_blank" rel="noopener noreferrer">
        <i class="fab fa-github" aria-hidden="true"></i><span>GitHub</span>
      </a>
      <a href="https://twitter.com/mosesgerrard" class="home-hero__social-link" target="_blank" rel="noopener noreferrer">
        <i class="fab fa-twitter" aria-hidden="true"></i><span>Twitter</span>
      </a>
    </div>
  </div>

  <!-- Nav at the bottom border of the hero -->
  <nav class="home-hero__nav" aria-label="Main navigation">
    <a href="/">Home</a>
    <a href="/musings/">Musings</a>
    <a href="/projects/">Projects</a>
    <a href="/resume/">Resume</a>
  </nav>

</section>

## Featured Projects

<div class="projects-grid home-projects-grid">
  {% assign featured = site.portfolio | sort: "date" | reverse %}
  {% for project in featured %}
  {% assign tag_slugs = "" %}
  {% for tag in project.tags %}
    {% assign slug = tag | downcase | replace: " ", "-" | replace: "_", "-" %}
    {% assign tag_slugs = tag_slugs | append: slug | append: " " %}
  {% endfor %}
  <div class="project-card" data-tags="{{ tag_slugs | strip }}">
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
      <p class="project-card__excerpt">{{ project.excerpt | strip_html | truncate: 90 }}</p>
      <a href="{{ project.url | relative_url }}" class="project-card__link">View Project &rarr;</a>
    </div>
  </div>
  {% endfor %}
</div>

<div style="text-align:center; margin: -1rem 0 2.5rem;">
  <a href="/projects/" class="btn btn--primary">View All Projects</a>
</div>
