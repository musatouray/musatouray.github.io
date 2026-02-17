# Project Blueprint: Musa Touray Portfolio

## Project Overview

This is a professional portfolio website for Musa Touray, a Microsoft Certified Power BI and PowerApps developer. The site showcases data analytics projects, technical skills, and professional certifications.

**Live URL:** https://musatouray.github.io
**Repository:** https://github.com/musatouray/musatouray.github.io
**Tech Stack:** Jekyll, Minimal Mistakes theme, GitHub Pages

## Architecture

### Theme
- **Framework:** Jekyll static site generator
- **Theme:** Minimal Mistakes v4.24.0 (remote_theme)
- **Skin:** Default (professional white/gray)
- **Deployment:** GitHub Pages

### Key Features
- Power BI embedded dashboards in blog posts
- Microsoft certification badges showcase
- Disqus comments integration
- Google Analytics tracking
- Formspree contact form
- Portfolio collection for projects
- Responsive design
- SEO optimized

## Directory Structure

```
.
├── _config.yml              # Main configuration file (CRITICAL)
├── _data/
│   └── navigation.yml       # Site navigation menu
├── _pages/                  # Static pages
│   ├── about.md
│   ├── contact.md
│   ├── privacy-policy.md
│   ├── categories.md
│   ├── tags.md
│   ├── portfolio.md
│   ├── skills.md
│   └── resume.md
├── _posts/                  # Blog posts (YYYY-MM-DD-title.md)
│   ├── 2021-12-04-real-time-live-tv-presidential-elections-report.md
│   ├── 2022-04-10-the-gambia-national-assembly-parliamentary-elections-live-dashbard.md
│   └── 2024-03-24-senegal-presidential-elections-analysis.md
├── _portfolio/              # Portfolio collection items (empty, ready for use)
├── assets/
│   ├── css/
│   │   └── main.scss        # Custom styling
│   └── images/              # All images, badges, logos
├── index.md                 # Home page with featured projects
├── Gemfile                  # Ruby dependencies
└── README.md                # Documentation
```

## Content Management

### Adding a Blog Post

1. **Create file** in `_posts/` with format: `YYYY-MM-DD-title.md`
2. **Use this front matter template:**

```yaml
---
layout: single
title: "Your Post Title"
excerpt: "Brief 1-2 sentence description for previews"
header:
  teaser: /assets/images/your-image.jpg
  overlay_image: /assets/images/your-image.jpg
  overlay_filter: 0.5
categories:
  - Power BI
  - Data Visualization
tags:
  - Power BI
  - PowerApps
  - Azure SQL
author_profile: true
comments: true
share: true
related: true
---

Your content here...
```

3. **For Power BI embeds**, use standard HTML:

```html
<p>
    <iframe style="width:100%;" height="383"
        src="YOUR_POWERBI_EMBED_URL"
        frameborder="0" allowFullScreen="true">
    </iframe>
</p>
```

### Adding a Portfolio Item

1. **Create file** in `_portfolio/` directory
2. **Use this template:**

```yaml
---
layout: single
title: "Project Name"
excerpt: "Project description"
header:
  teaser: /assets/images/project-image.jpg
  overlay_image: /assets/images/project-image.jpg
  overlay_filter: 0.5
sidebar:
  - title: "Technologies"
    text: "Power BI, PowerApps, Azure SQL"
  - title: "Year"
    text: "2024"
---

Full project description and details...
```

### Updating Pages

Pages are in `_pages/` directory. All use `layout: single`. Common front matter options:

```yaml
---
layout: single
title: "Page Title"
permalink: /page-url/
author_profile: true  # Show author bio sidebar
toc: true            # Table of contents
toc_label: "Contents"
header:
  overlay_image: /assets/images/header.jpg
  overlay_filter: 0.3
---
```

## Critical Configuration Files

### _config.yml

**DO NOT MODIFY these without testing:**
- `remote_theme`: Must stay `mmistakes/minimal-mistakes@4.24.0`
- `permalink`: Must stay `/:title/` (SEO - existing URLs depend on this)
- `url`: Must be `https://musatouray.github.io`

**Safe to modify:**
- `minimal_mistakes_skin`: Can change to "air", "contrast", "dark", "mint", etc.
- Navigation, author info, social links
- Analytics and comment settings
- Sidebar defaults

**Important settings:**
```yaml
# Disqus comments
comments:
  provider: "disqus"
  disqus:
    shortname: "portfoliowebsite"

# Google Analytics
analytics:
  provider: "google-gtag"
  google:
    tracking_id: "UA-113961478-1"
```

### Navigation (_data/navigation.yml)

Main menu items. Format:
```yaml
main:
  - title: "Display Name"
    url: /url-path/
```

## Development Workflow

### Local Development

1. **Install dependencies:**
   ```bash
   bundle install
   ```

2. **Run local server:**
   ```bash
   bundle exec jekyll serve --watch
   ```

3. **Preview:** http://localhost:4000

4. **Live reload:** Server auto-reloads on file changes

### Testing Checklist

Before deploying changes:
- [ ] All pages load without errors
- [ ] Power BI iframes display correctly
- [ ] Images load (use absolute paths: `/assets/images/...`)
- [ ] Navigation menu works
- [ ] Responsive design (test 375px, 768px, 1024px widths)
- [ ] No broken links
- [ ] Comments appear on posts
- [ ] Contact form submits successfully

### Git Workflow

**Branch Strategy:**
- `main`: Production (live site)
- `dev`: Development/testing
- Feature branches: For major changes

**Standard workflow:**
```bash
# Create feature branch from dev
git checkout dev
git pull origin dev
git checkout -b feature/your-feature

# Make changes, test locally
# Commit and push
git add .
git commit -m "Description of changes"
git push -u origin feature/your-feature

# Merge to dev for testing
git checkout dev
git merge feature/your-feature
git push origin dev

# After testing on dev, merge to main
git checkout main
git merge dev
git push origin main
```

## Deployment

### GitHub Pages Setup

**Settings → Pages:**
- Source: Deploy from branch
- Branch: `main` (or `dev` for testing)
- Folder: `/ (root)`
- Custom domain: (if applicable)
- Enforce HTTPS: ✓

**Build Status:** Check Actions tab for deployment status

### Build Time

- Typical build: 1-2 minutes
- After push, wait ~2-5 minutes for live site update
- Check: https://github.com/musatouray/musatouray.github.io/actions

## Customization

### Changing Color Scheme

**Option 1: Use different skin** (easiest)

In `_config.yml`:
```yaml
minimal_mistakes_skin: "mint"  # or "air", "contrast", "dark", etc.
```

**Option 2: Custom colors** (more control)

Edit `assets/css/main.scss`:
```scss
$primary-color: #0066cc;  # Your brand color
$link-color: #0066cc;
```

### Adding Custom CSS

Add to `assets/css/main.scss` after the imports:
```scss
/* Your custom styles */
.your-custom-class {
  /* styles */
}
```

### Certification Badges

Configured in `_config.yml` under `defaults` → `posts` → `sidebar`.

**To update badge links:**
1. Get Credly badge URL
2. Update in `_config.yml`
3. Ensure badge image exists in `/assets/images/`

Current badges:
- PL-300: Power BI Data Analyst
- PL-100: Power Platform App Maker
- AZ-900: Azure Fundamentals
- PL-900: Power Platform Fundamentals

## Common Tasks

### Update Author Bio

Edit `_config.yml` under `author:` section

### Change Site Title

Edit `_config.yml`:
```yaml
title: "Your New Title"
subtitle: "Your Subtitle"
```

### Add Social Links

Edit `_config.yml` under `author: links:` and `footer: links:`

### Update Contact Form Email

The Formspree form uses `{{site.email}}` which is set in `_config.yml`:
```yaml
email: amigomusa@amigomusa.com
```

### Enable Search (Optional)

In `_config.yml`:
```yaml
search: true
search_full_content: true
```

## Troubleshooting

### Build Fails

**Check GitHub Actions:**
1. Go to repository → Actions tab
2. Click failed build
3. Read error logs

**Common issues:**
- YAML syntax error in front matter
- Missing closing quotes in config
- Invalid liquid template syntax

### Images Not Displaying

**Checklist:**
- Use absolute paths: `/assets/images/file.jpg` (NOT `assets/images/file.jpg`)
- Check file extension matches (case-sensitive)
- Ensure image exists in `assets/images/`
- Check for typos in filename

### Power BI iframes Not Loading

**Embed URL format:**
```html
<iframe style="width:100%;" height="383"
    src="https://app.powerbi.com/view?r=YOUR_EMBED_CODE"
    frameborder="0" allowFullScreen="true">
</iframe>
```

**Common issues:**
- Missing `https://`
- Incorrect embed permissions in Power BI
- Ad blockers may block iframes (user-side issue)

### Disqus Comments Not Showing

**Verify:**
1. `comments: true` in post front matter
2. Correct shortname in `_config.yml`
3. Post is published (not in future date)
4. Site URL matches Disqus settings

### Local Build Works but GitHub Pages Fails

**Likely causes:**
- Using plugins not supported by GitHub Pages
- Gemfile includes gems GitHub Pages doesn't allow
- Check `_config.yml` plugins list matches GitHub Pages whitelist

## Important Notes

### URL Structure

**Never change** `permalink: /:title/` in `_config.yml`:
- Existing URLs depend on this format
- Search engines have indexed current URLs
- External links point to current structure
- Breaking URLs hurts SEO

### Image Paths

**Always use absolute paths** in front matter:
```yaml
# Correct:
image: /assets/images/photo.jpg

# Wrong:
image: assets/images/photo.jpg
```

### Author Configuration

**Site uses single author** defined in `_config.yml` under `author:`.

**Do not add** `author: john` or any other author reference in post front matter - it will cause issues. All posts automatically use the site-wide author.

### Bootstrap Removed

This site **no longer uses Bootstrap**. Minimal Mistakes has its own CSS framework. Don't add Bootstrap classes to new content.

## Resources

- [Minimal Mistakes Documentation](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Credly Badge Management](https://www.credly.com/)

## Support

For theme issues:
- [Minimal Mistakes GitHub Issues](https://github.com/mmistakes/minimal-mistakes/issues)

For Jekyll issues:
- [Jekyll Talk Forum](https://talk.jekyllrb.com/)

For deployment issues:
- Check GitHub Actions logs
- [GitHub Pages Troubleshooting](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/troubleshooting-jekyll-build-errors-for-github-pages-sites)

---

**Last Updated:** 2026-02-16
**Theme Version:** Minimal Mistakes 4.24.0
**Jekyll Version:** Via GitHub Pages gem
