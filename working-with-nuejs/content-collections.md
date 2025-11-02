# Content Collections in Nue.js

Blog posts, documentation, portfolios using collections.

## Collections Setup

**site.yaml:**
```yaml
collections:
  posts:
    include: [ posts/ ]
    require: [ title, date ]
    sort: date desc
```

**Directory:**
```
my-site/
├── posts/
│   ├── post-1.md
│   ├── post-2.md
│   └── post-3.md
└── site.yaml
```

## Front Matter

```markdown
---
title: My Post
description: Short excerpt
date: 2025-01-15
author: Your Name
tags: [web, tutorial]
---

# Content here
```

## Listing Collection Items

Collection data automatically available in templates. The collection name (`posts`) becomes the variable name.

```html
<!-- posts array available automatically -->
<div :for="post in posts">
  <h3><a :href="post.url">{ post.title }</a></h3>
  <time>{ post.date }</time>
  <p>{ post.description }</p>

  <div :for="tag in post.tags">
    <span>{ tag }</span>
  </div>
</div>
```

**How it works:** Nue scans directories in `include:`, extracts front matter from each file, creates array with that collection name. Access via `:for` loop.

**Available properties:** `title`, `date`, `description`, `url`, `tags`, `author` (all from front matter)

## RSS Feed

**Requirements:**
1. Collection configured
2. Set `rss.enabled: true`
3. Provide `site.origin` (full URL)
4. Posts have `title`, `date`, `description`

**site.yaml:**
```yaml
site:
  origin: https://myblog.com

rss:
  enabled: true
  title: My Blog
  description: Latest posts
  collection: posts
```

**Result:** Auto-generated `/feed.xml`

## Multiple Collections

```yaml
collections:
  posts:
    include: [ blog/ ]
    sort: date desc

  docs:
    include: [ docs/ ]
    require: [ title, order ]
    sort: order asc
```

Docs use `order` instead of `date` for manual ordering.

## Use Cases

- Blog posts
- Documentation pages
- Portfolio projects
- Case studies
- Team members

## When You Need More

**Project organization** → organizing-projects.md
**Full config options** → reference-guide.md
