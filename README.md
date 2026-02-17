# Musa Touray Portfolio - Data Analytics & Power BI

This is the portfolio website of Musa Touray, Microsoft Certified Power BI and PowerApps developer.

## Theme

This site uses the [Minimal Mistakes](https://github.com/mmistakes/minimal-mistakes) Jekyll theme.

## Local Development

### Prerequisites
- Ruby 2.7+
- Bundler
- Jekyll

### Setup
```bash
bundle install
bundle exec jekyll serve --watch
```

Visit `http://localhost:4000` to preview.

## Site Structure

- `_posts/` - Blog posts (Power BI projects, analytics insights)
- `_pages/` - Static pages (About, Contact, Portfolio, Skills, Resume, etc.)
- `_portfolio/` - Portfolio collection items
- `_data/navigation.yml` - Main navigation menu
- `assets/images/` - Images, certification badges, project screenshots
- `assets/css/main.scss` - Custom styling

## Features

- Power BI embedded dashboards
- Microsoft certification showcase
- Disqus comments
- Google Analytics
- Responsive design
- SEO optimized

## Content Management

### Adding a Blog Post
Create file in `_posts/` with format: `YYYY-MM-DD-title.md`

```yaml
---
layout: single
title: "Post Title"
excerpt: "Brief description"
categories:
  - Power BI
tags:
  - Tag1
  - Tag2
header:
  teaser: /assets/images/image.jpg
  overlay_image: /assets/images/image.jpg
  overlay_filter: 0.5
---
```

### Adding a Portfolio Item
Create file in `_portfolio/` with front matter:

```yaml
---
layout: single
title: "Project Name"
excerpt: "Project description"
header:
  teaser: /assets/images/project.jpg
---
```

## Configuration

Main configuration in `_config.yml`:
- Site metadata
- Author information
- Social links
- Analytics
- Comments
- Theme settings

## License

Content © Musa Touray. All rights reserved.
Theme: MIT License (Minimal Mistakes)
