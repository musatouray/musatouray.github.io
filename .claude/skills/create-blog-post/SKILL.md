---
name: create-blog-post
description: Create a new Jekyll blog post for musatouray.github.io from provided text content. Handles front matter generation, file naming, slug creation, and correct image path conventions.
argument-hint: "[post title or paste your content]"
---

# Create Blog Post

You are creating a new Musings post for musatouray.github.io — a Jekyll site using Minimal Mistakes v4.24.0.

## Step 1 — Gather information

If the user has not already provided the following, ask before writing anything:

- **Post title** — if not given, derive one from the content
- **Teaser image** — ask if they have an image to add to `assets/images/`. If not, the post will use a gradient placeholder; confirm this is OK.
- **Categories and tags** — suggest sensible defaults from the content (Power BI, DAX, Fabric, Data Engineering, etc.) and ask the user to confirm or adjust.

Do NOT ask about layout, author_profile, permalink structure, or date — these are handled automatically per the rules below.

## Step 2 — Generate the front matter

Use this exact template:

```yaml
---
layout: single
title: "TITLE"
excerpt: "EXCERPT — 1-2 sentences, written for a preview card. No markdown."
date: YYYY-MM-DD
permalink: /musings/SLUG/
header:
  teaser: /assets/images/FILENAME.png
  overlay_image: /assets/images/FILENAME.png
  overlay_filter: 0.65
categories:
  - Musings
  - CATEGORY
tags:
  - TAG1
  - TAG2
author_profile: true
share: true
related: true
---
```

**Rules:**
- `date` — use today's date (available in MEMORY.md as `currentDate`)
- `permalink` — always `/musings/SLUG/` where SLUG is the title lowercased, spaces replaced with hyphens, special characters removed
- `excerpt` — plain text only, no markdown, max 2 sentences, written to entice clicks
- `overlay_filter` — use `0.65` as default
- `categories` — always include `Musings` as the first category
- If no image is provided, omit the `header` block entirely (MM will use a gradient)

## Step 3 — Generate the filename

Format: `_posts/YYYY-MM-DD-slug-title.md`

Example: `_posts/2025-10-08-dynamic-column-headers-power-bi-matrix.md`

## Step 4 — Write the post content

- Use the text provided by the user as the basis
- Structure with `##` headings (H2) and `###` subheadings (H3) — never H1 (the title is already the H1)
- Keep the author's voice — do not rewrite to sound generic
- For DAX or code samples, use fenced code blocks with the language identifier:
  ````
  ```dax
  MEASURE = CALCULATE(...)
  ```
  ````
- For Power BI embed iframes use:
  ```html
  <p>
      <iframe style="width:100%;" height="383"
          src="YOUR_POWERBI_EMBED_URL"
          frameborder="0" allowFullScreen="true">
      </iframe>
  </p>
  ```
- Image references must use absolute paths: `/assets/images/filename.png`
- Do NOT include an inline image that duplicates the header overlay — the page header already shows it

## Step 5 — Create the file

Write the complete file to `_posts/YYYY-MM-DD-slug.md` using the Write tool.

Then tell the user:
1. The filename created
2. If they provided an image: remind them to ensure `assets/images/FILENAME.png` is saved and committed (`git add assets/images/FILENAME.png`)
3. The next step: commit and push to dev, then merge to main

## Image conventions (critical)

- All images live in `assets/images/` and are committed to git
- Always use `.png` for new post images (user preference)
- Never reference GitHub user-attachments URLs (`https://github.com/user-attachments/...`) — these expire and break
- Absolute paths only: `/assets/images/filename.png` NOT `assets/images/filename.png`

## What NOT to do

- Do not add `atom_feed`, `comments`, or `author:` to front matter — these are set globally
- Do not use Bootstrap classes
- Do not add `toc: true` unless the post is long and technical (>5 sections)
- Do not push to main directly — always commit to dev first, then merge --no-ff to main
