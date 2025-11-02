# Building Sites with Nue.js

Guide for creating content-focused websites with file-based routing, layouts, and collections.

## File-Based Routing

Directory structure maps directly to URLs:

```
my-site/
├── index.html              → /
├── about.html              → /about
├── blog/
│   ├── index.html          → /blog/
│   └── first-post.md       → /blog/first-post
└── docs/
    ├── index.html          → /docs/
    └── guide.md            → /docs/guide
```

**Key points:**
- `index.html` or `index.md` = directory entry point
- File extensions (.html, .md) omitted from URLs
- Subdirectories create URL paths
- No configuration needed

## Layouts

Layouts provide shared structure (headers, footers, navigation):

### Site-Wide Layout

**layout.html** in root applies to all pages:

```html
<!doctype html>
<header>
  <nav>
    <a href="/">Home</a>
    <a href="/about">About</a>
  </nav>
</header>

<main>
  <!-- Page content injected here -->
</main>

<footer>
  <p>&copy; 2025 My Site</p>
</footer>
```

### Section-Specific Layouts

**blog/layout.html** applies only to blog section:

```html
<!doctype html>
<header>
  <h1>Blog</h1>
</header>

<article>
  <!-- Post content injected here -->
</article>

<aside>
  <h3>Recent Posts</h3>
  <!-- Sidebar content -->
</aside>
```

**Layout hierarchy:**
1. Section layout (blog/layout.html) if exists
2. Site layout (layout.html) if exists
3. No layout (content only)

## Markdown Content

Nue.js converts `.md` files to HTML automatically:

**blog/my-post.md:**
```markdown
# Welcome to My Site

This is a paragraph with **bold** and *italic* text.

## Features

- Easy to write
- Converts to HTML
- Supports front matter
```

**Becomes:** `/blog/my-post`

## Front Matter (Metadata)

Add YAML metadata to markdown files:

```markdown
---
title: My Page Title
description: A short description
date: 2025-01-15
author: Your Name
tags: [web, tutorial]
---

# Content starts here...
```

**Common fields:**
- `title` - Page title (used in `<title>`, listings)
- `description` - Meta description, excerpts
- `date` - Publication date (YYYY-MM-DD)
- `author` - Content author
- `tags` - Array of tags/categories

**Access in templates:**
```html
<h1>{ title }</h1>
<p>{ description }</p>
<time>{ date }</time>
```

## Content Collections

Organize related content with collections:

```
my-site/
├── posts/
│   ├── post-1.md
│   ├── post-2.md
│   └── post-3.md
└── site.yaml
```

**site.yaml:**
```yaml
collections:
  posts:
    include: [ posts/ ]
    require: [ title, date ]
    sort: date desc
```

**Use cases:**
- Blog posts
- Documentation pages
- Portfolio projects
- Case studies
- Team members
- Products

## Listing Collection Items

### Built-In Component

```html
<page-list/>
```

Simple listing with default formatting.

### Custom Listing

```html
<div :for="post in posts">
  <h3><a :href="post.url">{ post.title }</a></h3>
  <time>{ post.date }</time>
  <p>{ post.description }</p>

  <div :for="tag in post.tags">
    <span class="tag">{ tag }</span>
  </div>
</div>
```

**Available properties:**
- `post.title` - From front matter
- `post.date` - From front matter
- `post.description` - From front matter
- `post.url` - Auto-generated path
- `post.tags` - Array from front matter
- `post.author` - From front matter

## site.yaml Configuration

Central configuration for your site:

```yaml
# SEO & Metadata
meta:
  title: My Site
  title_template: "%s | My Site"
  description: A description of my site
  author: Your Name

# Site Settings
site:
  origin: https://mysite.com
  view_transitions: true

# Design Options
design:
  inline_css: true

# Content Collections
collections:
  posts:
    include: [ posts/ ]
    require: [ title ]
    sort: date desc

  docs:
    include: [ docs/ ]
    require: [ title ]
    sort: order asc

# RSS Feed (optional)
rss:
  enabled: true
  title: My Site Feed
  description: Latest updates
  collection: posts
```

**Key sections:**
- `meta:` - SEO metadata, title templates
- `site:` - Site-wide settings (origin needed for RSS)
- `design:` - Styling options
- `collections:` - Define content collections
- `rss:` - Auto-generate RSS feed

## RSS Feed Generation

For sites with collections (blogs, news, updates):

**Requirements:**
1. Configure collection in `site.yaml`
2. Set `rss.enabled: true`
3. Specify `rss.collection: posts`
4. Provide `site.origin` (full URL with https://)
5. Ensure posts have `title`, `date`, `description`

**Result:** Auto-generated `/feed.xml`

**Example:**
```yaml
site:
  origin: https://myblog.com

rss:
  enabled: true
  title: My Blog
  description: Latest posts
  collection: posts

collections:
  posts:
    include: [ blog/ ]
    sort: date desc
```

## The @shared Directory

Shared assets available across your entire site:

```
my-site/
└── @shared/
    ├── design/          # CSS (auto-loaded globally!)
    │   └── style.css
    ├── ui/              # Shared components
    │   └── header.html
    ├── data/            # YAML data (server-side)
    │   └── team.yaml
    └── server/          # Backend routes (see server-side.md)
        └── api.js
```

**Key features:**
- **Must be named exactly `@shared/`** (not `@global/`, `@common/`, etc.)
- CSS in `@shared/design/` auto-loads on all pages (no `<link>` tags!)
- Components in `@shared/ui/` available everywhere
- Data in `@shared/data/` for template rendering

## Common Patterns

### Multi-Section Site

```
my-site/
├── @shared/design/style.css
├── layout.html              # Site-wide header/footer
├── index.html               # Homepage
├── about.html
├── blog/
│   ├── layout.html          # Blog-specific layout
│   ├── index.html           # Blog listing
│   └── posts/
│       ├── post-1.md
│       └── post-2.md
└── docs/
    ├── layout.html          # Docs-specific layout
    ├── index.html           # Docs home
    └── guides/
        ├── getting-started.md
        └── advanced.md
```

### Static Marketing Site

```
my-site/
├── @shared/design/
│   ├── base.css
│   └── components.css
├── layout.html
├── index.html               # Homepage
├── features.html
├── pricing.html
├── about.html
└── contact.html
```

No collections, no dynamic content - just pages with shared layout.

### Documentation Site

```
docs/
├── @shared/
│   ├── design/theme.css
│   └── ui/sidebar.html
├── layout.html
├── index.md
└── guides/
    ├── installation.md
    ├── configuration.md
    └── deployment.md
```

```yaml
# site.yaml
collections:
  guides:
    include: [ guides/ ]
    require: [ title, order ]
    sort: order asc
```

Ordered documentation pages, not date-sorted posts.

## Supported File Types

- `.html` - HTML pages
- `.md` - Markdown content
- `.css` - Stylesheets (auto-load from @shared/design/)
- `.js`, `.ts` - Scripts
- `.svg` - SVG assets
- `.yaml` - Data files

## When NOT to Use These Features

- **Interactive apps** → See adding-interactivity.md
- **REST APIs** → See server-side.md
- **Client-side routing** → See adding-interactivity.md

Building sites is for content, pages, and file-based routing. Add interactivity or server features when needed.
