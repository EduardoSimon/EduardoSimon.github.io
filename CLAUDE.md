# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Static personal site for Eduardo Simón Picón. Plain HTML/CSS, no JavaScript, no build step. Hosted on GitHub Pages with automatic deployment via GitHub Actions.

- **Domain**: edusimon.dev
- **Repo**: EduardoSimon/EduardoSimon.github.io

## Local Development

```bash
python3 -m http.server 8000
```

Open `http://localhost:8000`. A local server is needed for correct path resolution between pages.

## Infrastructure

- **Domain registrar + DNS**: Cloudflare (`edusimon.dev`)
- **Hosting**: GitHub Pages (CDN + SSL via Let's Encrypt)
- **Email**: Cloudflare Email Routing → `edusimonpicon@gmail.com`
- **DNS setup**: CNAME `@` → `EduardoSimon.github.io` (DNS only, no Cloudflare proxy — GitHub needs direct connection for SSL cert provisioning)
- **CNAME file**: Must exist in repo root to persist the custom domain across deploys. Do not delete it.

## Deployment

Push to `main` triggers `.github/workflows/deploy.yml`, which deploys to GitHub Pages automatically. No build step — files are served as-is.

## Architecture

- **No frameworks, no JS** — pure static HTML/CSS
- All pages share a single stylesheet: `css/style.css`
- Dark/light mode via `prefers-color-scheme` CSS media query (no toggle)
- Design tokens defined as CSS custom properties in `:root`
- Navigation between pages uses relative HTML links (e.g., `blog.html`, `index.html`)
- Blog posts go in `blog/` as individual HTML files
- Images go in `images/`

## Design System (`css/style.css`)

- System serif font stack: `Georgia, 'Times New Roman', serif`
- Content max-width: `640px`, centered
- Light: `#fff` bg, `#171717` text, `#525252` secondary
- Dark: `#0a0a0a` bg, `#ededed` text, `#a3a3a3` secondary
- Mobile-first, `768px` breakpoint for desktop top padding

## Adding Content

### Adding a Blog Post

1. Create `blog/post-slug.html` with this structure:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Post Title — Eduardo Simón Picón</title>
  <link rel="stylesheet" href="../css/style.css">
</head>
<body>
  <h1>post title</h1>
  <!-- post content here -->
  <a href="../blog.html" class="back-link">← back to blog</a>
</body>
</html>
```
2. Add an entry to `blog.html` before the back-link, using `.post-list`:
```html
<ul class="post-list">
  <li><time>February 15, 2026</time><a href="blog/post-slug.html">Post Title</a></li>
</ul>
```
3. Remove the "No posts yet" placeholder when adding the first post.

### Adding a Talk

Add a new `<div class="talk">` block in `talks.html`, **before** the existing talks (newest first). Insert it after the `<h1>` and before the first existing `<div class="talk">`.

Template:
```html
<div class="talk">
  <h3>Talk Title</h3>
  <p class="meta">Date · Venue/Event</p>
  <p class="links"><a href="URL">Repo</a> · <a href="URL">Slides</a> · <a href="URL">Video</a></p>
</div>
```

- Date format: `Month Day, Year` (e.g., `February 19, 2026`) or just `Year` if no exact date
- The `<p class="links">` line is optional — only include it if there are associated links
- Separate multiple links with ` · `
- Link types used so far: `Repo`, `Slides`, `Video`

### Page Conventions

- Page titles follow the pattern: `Page Name — Eduardo Simón Picón` (home page is just `Eduardo Simón Picón`)
- `<h1>` text is lowercase (e.g., `blog`, `talks`)
- All sub-pages include a `← back to home` link with class `back-link`
- No shared header/nav component — each page is self-contained HTML
