# Project Blueprint: Musa Touray Portfolio

## Project Overview

This is a professional portfolio website for Musa Touray, a Data Engineer, Analytics Engineer, and Microsoft Certified Fabric and Power BI specialist. The site showcases data analytics projects, technical skills, and professional certifications.

**Live URL:** https://musatouray.github.io
**Repository:** https://github.com/musatouray/musatouray.github.io
**Tech Stack:** Jekyll, Minimal Mistakes theme, GitHub Pages

## Architecture

### Theme
- **Framework:** Jekyll static site generator
- **Theme:** Minimal Mistakes v4.24.0 (remote_theme)
- **Skin:** Default (professional white/gray)
- **Deployment:** GitHub Pages — Deploy from branch (`main`)

### Key Features
- Custom full-bleed hero on home page (replaces MM sidebar author profile)
- Power BI embedded dashboards in blog posts
- Microsoft certification badges in post/page sidebar
- Featured Projects + Latest Musings grids on home page
- Portfolio collection for projects
- Musings (blog posts) with filter buttons and load-more
- Responsive design, SEO optimised

## Directory Structure

```
.
├── _config.yml              # Main configuration file (CRITICAL)
├── _data/
│   └── navigation.yml       # Site navigation menu
├── _pages/                  # Static pages
│   ├── contact.md
│   ├── privacy-policy.md
│   ├── categories.md
│   ├── tags.md
│   ├── portfolio.md         # Projects listing page
│   ├── musings.md           # Musings/blog listing page
│   └── resume.md
├── _posts/                  # Blog posts (YYYY-MM-DD-title.md)
│   ├── 2021-12-04-real-time-live-tv-presidential-elections-report.md
│   ├── 2022-04-10-the-gambia-national-assembly-parliamentary-elections-live-dashbard.md
│   ├── 2024-03-24-senegal-presidential-elections-analysis.md
│   └── 2025-10-08-dynamic-column-headers-power-bi-matrix.md
├── _portfolio/              # Portfolio collection items
├── .github/
│   └── workflows/
│       └── jekyll.yml       # GitHub Actions workflow (user-managed)
├── assets/
│   ├── css/
│   │   └── main.scss        # All custom styling (do not split)
│   └── images/              # All images, badges, logos
├── index.md                 # Home page (custom hero + project/musings grids)
├── Gemfile                  # Ruby dependencies
└── README.md                # Documentation
```

## Home Page Architecture

The home page (`index.md`) uses a fully custom layout — **not** the standard MM overlay hero or sidebar.

**Front matter:**
```yaml
---
layout: single
title: "Musa Touray"
classes: wide home-page
author_profile: false
---
```

**Page structure:**
1. `<section class="home-hero">` — full-bleed profile card
   - `.home-hero__inner` — centred column (name, avatar, bio, tagline, divider, location, social links)
   - `.home-hero__nav` — pill-style nav bar at the bottom border of the hero
2. `## Featured Projects` — `.projects-grid home-projects-grid` (3-col, portfolio items)
3. `## Latest Musings` — `.projects-grid home-musings-grid` (3-col, 3 most recent posts)

**Key CSS behaviour (`.home-page` body class):**
- Masthead hidden entirely: `.home-page .masthead { display: none !important }`
- All MM layout wrappers reset to `width: 100%; float: none; margin: 0; padding: 0`
- `overflow-x: clip` (NOT `hidden`) to prevent horizontal scroll without creating scroll container
- Content below hero re-padded: `.page__content > *:not(.home-hero) { padding-left: 2rem; padding-right: 2rem }`
- Both home grids share the 3-column rule: `.home-projects-grid, .home-musings-grid { grid-template-columns: repeat(3, 1fr) }`

## Design System (`assets/css/main.scss`)

### Brand Tokens
```scss
$primary-color:  #FF6634;   // set before @import "minimal-mistakes"
$link-color:     #FF6634;

:root {
  --color-primary:    #FF6634;
  --color-nav-bg:     #f0f4f8;
  --color-text:       #0F1729;
  --color-muted:      #6B7280;
  --color-border:     #e2e8f0;
  --font-sans:        'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}
```

### Typography
- All `h1–h6` locked to **18px** via `!important` to prevent MM's responsive root scaling
- Body font: Inter via `--font-sans`

### Key MM Overrides
- Masthead nav underline bar: `.visible-links a::before { display: none !important }`
- Masthead nav hover: pill style (faint orange bg, orange text) — matches hero nav
- Site title text hidden (`font-size: 0`) but logo image still shows
- Subtitle hidden globally: `.site-subtitle { display: none !important }`
- RSS Feed removed from footer: `atom_feed: hide: true` in `_config.yml`
- Twitter removed from `author.links` and `footer.links` in `_config.yml`

### Card Components
- `.project-card` — used for portfolio page AND home page Featured Projects and Latest Musings
- `.musing-card` — used on the Musings listing page only
- `.load-more-btn` requires `color: #fff !important` (MM cascade overrides btn--primary color)

## Critical Configuration Files

### _config.yml

**DO NOT MODIFY these without testing:**
- `remote_theme`: Must stay `mmistakes/minimal-mistakes@4.24.0`
- `permalink`: Must stay `/:title/` (SEO — existing URLs depend on this)
- `url`: Must be `https://musatouray.github.io`

**Current important settings:**
```yaml
atom_feed:
  hide: true          # removes FEED link from footer

# Google Analytics
analytics:
  provider: "google-gtag"
  google:
    tracking_id: "UA-113961478-1"
```

**Social links:** Only LinkedIn and GitHub are active. Twitter has been removed from `author.links` and `footer.links`.

### Gemfile

```ruby
gem "github-pages", group: :jekyll_plugins
# Do NOT add jekyll-include-cache separately — it's bundled inside github-pages
```

### Navigation (_data/navigation.yml)

Current nav items: Home (`/`), Musings (`/musings/`), Projects (`/projects/`), Resume (`/resume/`)

## Posts

| File | Teaser Image |
|------|-------------|
| `2021-12-04-real-time-live-tv-presidential-elections-report.md` | `gambia-presidential-elections.png` |
| `2022-04-10-...-parliamentary-elections-live-dashbard.md` | `gambia-parliamentary-elections.jpg` |
| `2024-03-24-senegal-presidential-elections-analysis.md` | `senegal_elections_analysis.png` |
| `2025-10-08-dynamic-column-headers-power-bi-matrix.md` | `matrix_table_with_dynamic_column_headers.png` |

**Note:** Election post images are intentionally `.png` (except parliamentary which is `.jpg`). Do not change extensions without checking.

### Adding a Blog Post

1. Create file in `_posts/` with format: `YYYY-MM-DD-title.md`
2. Use this front matter template:

```yaml
---
layout: single
title: "Your Post Title"
excerpt: "Brief 1-2 sentence description for previews"
date: YYYY-MM-DD
permalink: /musings/your-post-slug/
header:
  teaser: /assets/images/your-image.png
  overlay_image: /assets/images/your-image.png
  overlay_filter: 0.65
categories:
  - Musings
  - Power BI
tags:
  - Power BI
  - DAX
author_profile: true
share: true
related: true
---

Post content here...
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

Create file in `_portfolio/` directory:

```yaml
---
layout: single
title: "Project Name"
excerpt: "Project description"
header:
  teaser: /assets/images/project-image.png
  overlay_image: /assets/images/project-image.png
  overlay_filter: 0.5
tags:
  - Power BI
  - Azure
---

Full project description and details...
```

## Certification Badges

Configured in `_config.yml` under `defaults` → `posts` → `sidebar`. Current badges (all `.png`):
- **DP-700**: Fabric Analytics Engineer Associate
- **DP-600**: Fabric Analytics Engineer
- **PL-300**: Power BI Data Analyst Associate
- **PL-100**: Power Platform App Maker Associate
- **DP-900**: Azure Data Fundamentals
- **PL-900**: Power Platform Fundamentals

## Development Workflow

### Local Development

```bash
bundle install
bundle exec jekyll serve --watch
# Preview at http://localhost:4000
```

### Git Workflow

**Branch strategy:**
- `main` — Production (live site, GitHub Pages serves from here)
- `dev` — Development/staging

**Standard workflow:**
```bash
# Work on dev
git checkout dev
# ... make changes ...
git add <files>
git commit -m "Description"
git push origin dev

# Merge to main (always use --no-ff)
git checkout main
git merge --no-ff dev -m "Merge dev: description of changes"
git push origin main

# Sync dev back to avoid "X commits behind" warning
git checkout dev
git merge --no-ff main -m "Sync dev with main merge commits"
git push origin dev
git checkout main
```

**Force a rebuild without content changes:**
```bash
git commit --allow-empty -m "Trigger GitHub Pages rebuild"
git push origin main
```

**Important:** Always use `git merge --no-ff` (not fast-forward) when merging dev → main.

## Deployment

### GitHub Pages Setup

**Settings → Pages:**
- Source: **Deploy from a branch**
- Branch: `main` / `/ (root)`
- Enforce HTTPS: ✓

**Do NOT use GitHub Actions deployment** — the branch-based builder is simpler and works correctly with the `github-pages` gem. A `jekyll.yml` Actions workflow exists in `.github/workflows/` but deployment is handled by the branch-based builder, not Actions.

### Build Time
- Typical build: 1–2 minutes after push
- CDN propagation: up to 5–10 minutes
- Always test in **incognito mode** to bypass browser cache
- Hard refresh: `Ctrl+Shift+R`

## Important Notes

### Image Paths
**Always use absolute paths** in front matter and content:
```yaml
# Correct:
teaser: /assets/images/photo.png

# Wrong:
teaser: assets/images/photo.png
```

**Never reference GitHub user-attachments URLs** (e.g. `https://github.com/user-attachments/assets/...`) — these are private/temporary and return 404 when embedded. Always save images to `assets/images/` and commit them.

### URL Structure
**Never change** `permalink: /:title/` in `_config.yml` — search engines have indexed existing URLs.

### Author Configuration
Site uses a single author defined in `_config.yml` under `author:`. Do not add `author:` to individual post front matter — it causes issues. All posts automatically use the site-wide author.

### Bootstrap
This site does **not** use Bootstrap. Minimal Mistakes has its own CSS framework. Do not add Bootstrap classes.

## Troubleshooting

### Build Fails
1. Go to repo → **Actions** tab → click failed build → read error logs
2. Common causes: YAML syntax error, missing quotes, invalid Liquid syntax

### Images Not Displaying
- Use absolute paths: `/assets/images/file.png`
- Check filename and extension match exactly (case-sensitive on GitHub Pages)
- Ensure image is committed to git (`git status` to check for untracked files)

### Changes Not Showing on Live Site
1. Check the Pages build completed (green checkmark in Actions tab)
2. Open site in **incognito** mode to bypass browser cache
3. Wait up to 10 minutes for CDN propagation after branch switch
4. If still stale, push an empty commit to force rebuild

### Gemfile Dependency Warning
If you see "The github-pages gem can't satisfy your Gemfile's dependencies" — ensure `jekyll-include-cache` is **not** listed separately in the Gemfile; it's bundled inside `github-pages`.

### Power BI iframes Not Loading
- Verify embed URL starts with `https://`
- Check embed permissions in Power BI service
- Ad blockers may block iframes on visitor's side

## Resources

- [Minimal Mistakes Documentation](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Credly Badge Management](https://www.credly.com/)

---

**Last Updated:** 2026-03-01
**Theme Version:** Minimal Mistakes 4.24.0
**Jekyll Version:** Via GitHub Pages gem
