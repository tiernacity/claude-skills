# Nue.js Blog & Content Sites Guide

Complete guide for creating blogs and content-focused websites with Nue.js.

## Content Collections

Markdown files in a directory automatically form a collection:

```
blog/
├── index.html           # Lists all posts
├── first-post.md        # Becomes /blog/first-post
└── second-post.md       # Becomes /blog/second-post
```

## Front Matter (YAML Metadata)

Add metadata at the top of `.md` files:

```markdown
---
title: My Blog Post
date: 2025-01-15
description: A short summary for listings and RSS
tags: [javascript, nue]
---

# Post content starts here...
```

**Required fields:** `title`, `date` (YYYY-MM-DD format)
**Recommended:** `description` (for RSS and listings)
**Posts auto-sort** by date descending (newest first)

## Listing Posts

**Option 1:** Built-in component (simple)
```html
<page-list/>
```

**Option 2:** Custom loop (more control)
```html
<div :for="post in posts">
  <h3><a :href="post.url">{ post.title }</a></h3>
  <time>{ post.date }</time>
  <p>{ post.description }</p>
</div>
```

**Available post properties:** `title`, `date`, `description`, `url`, `tags`, `author`

## site.yaml Configuration

Blogs need config in `site.yaml`:

```yaml
meta:
  title: My Blog
  title_template: %s / My Blog
  author: Your Name

site:
  origin: https://yourblog.com
  view_transitions: true

rss:
  enabled: true
  title: My Blog
  description: Latest posts on web development
  collection: posts

design:
  inline_css: true

collections:
  posts:
    include: [ posts/ ]
    require: [ title ]
    sort: date desc
```

**Key sections:**
- `meta:` - SEO metadata (title templates, author)
- `site:` - Site-wide settings (origin required for RSS)
- `rss:` - Auto-generates `/feed.xml` when enabled
- `collections:` - Define content collections with sorting
- `design:` - Styling options (inline_css for performance)

## RSS Feed Generation

With correct `site.yaml` (see above), Nue auto-generates RSS:

1. Set `rss.enabled: true`
2. Specify `rss.collection: posts` (matches your directory)
3. Provide `site.origin` (full URL with https://)
4. Ensure posts have `title`, `date`, `description`

RSS available at `/feed.xml` automatically.

## Common Patterns

### Blog with Multiple Categories

```
my-blog/
├── @shared/design/style.css
├── layout.html
├── index.html
├── posts/
│   ├── tech/
│   │   ├── post-1.md
│   │   └── post-2.md
│   └── life/
│       ├── post-3.md
│       └── post-4.md
└── site.yaml
```

Configure multiple collections:
```yaml
collections:
  tech:
    include: [ posts/tech/ ]
    sort: date desc
  life:
    include: [ posts/life/ ]
    sort: date desc
```

### Date Formatting

In templates:
```html
<time>{ new Date(post.date).toLocaleDateString() }</time>
```

### Tags and Categories

Access tags in templates:
```html
<div :for="tag in post.tags">
  <span class="tag">{ tag }</span>
</div>
```
