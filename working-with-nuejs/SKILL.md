---
name: working-with-nuejs
description: Use when working with Nue.js 2.x (HTML-first reactive framework) - covers getting started, component syntax, single/multi-page apps, @shared/ directory, file-based routing, layouts, blogs with content collections, front matter, RSS feeds, site.yaml configuration, and React-to-Nue transition
---

# Working with Nue.js

## Overview

Nue.js is a minimalist, HTML-first reactive framework (1MB vs typical 500MB node_modules). Core principle: **Direct DOM manipulation with HTML-centric components**, not JavaScript-centric virtual DOM.

**Current version:** 2.0.0-beta.2 (as of 2025-11-01)
**Documentation:** https://nuejs.org/docs/
**Runtime:** Bun 1.2+ (JavaScript runtime)

## Installation & Quick Start

```bash
# Install Bun (if needed)
curl -fsSL https://bun.sh/install | bash

# Install Nue.js globally
bun install --global nuekit

# Create a project (optional templates: minimal, blog, spa, full)
nue create my-app

# Or start from scratch - just create index.html and run:
nue serve
```

**Zero config required** - Nue follows UNIX philosophy: create HTML file, run `nue`.

## Minimal Counter Example

The simplest reactive component is a single HTML file:

```html
<!doctype dhtml>
<html>
<head>
  <title>Counter</title>
</head>
<body>
  <!-- Inline reactive component -->
  <div>
    <button @click="count--">-</button>
    <b>{ count }</b>
    <button @click="count++">+</button>

    <script>
      // Initialize state - that's it!
      count = 0
    </script>
  </div>
</body>
</html>
```

**Key insights:**
- `<!doctype dhtml>` enables reactive/interactive mode (client-side rendering)
- `@click` for event handlers (not `:onclick` in v2.0)
- `{ variable }` for data binding
- `<script>` block defines state inline - no imports needed
- State changes trigger automatic DOM updates

## Component Syntax Quick Reference

| Pattern | Syntax | Example |
|---------|--------|---------|
| Event handler | `@click="expression"` | `@click="count++"` |
| Data binding | `{ variable }` | `<b>{ count }</b>` |
| Initialize state | `<script>` block | `<script>count = 0</script>` |
| Conditional | `:if="condition"` | `<p :if="count > 0">Positive</p>` |
| Loop | `:for="item in items"` | `<li :for="todo in todos">{ todo }</li>` |

## Doctype Confusion: dhtml vs html

**Use `<!doctype dhtml>` when:**
- Creating interactive/reactive components
- Need client-side rendering
- Building SPAs or dynamic UIs

**Use regular `<!doctype html>` when:**
- Static pages (server-side rendering)
- Content-focused pages (blogs, docs)
- No interactivity needed

Most beginners need `dhtml` for their first experiments with reactivity.

## For React Developers

If you're coming from React, **unlearn these patterns:**

| React Pattern | Nue.js Pattern | Why Different |
|--------------|----------------|---------------|
| `useState(0)` | `count = 0` in `<script>` | Direct variable, no hooks |
| `onClick={() => ...}` | `@click="..."` | HTML attribute, not prop |
| JSX `<MyComponent />` | Inline HTML | HTML-first, not JS-first |
| Import/mount | Automatic | No manual mounting needed |
| Separate files | Inline for simple cases | Start simple, extract later |
| Virtual DOM | Direct DOM | Nue mutates DOM directly |

**Don't overcomplicate** - Nue is intentionally simpler than React. A single HTML file is often enough.

## Project Structure

### Single-Page Apps (Simple)

```
my-app/
└── index.html              # Just one file - that's it!
```

### Multi-Page Sites (Blog, Docs, Marketing)

```
my-app/
├── index.html              # Homepage
├── layout.html             # Shared header/footer (optional)
├── site.yaml               # Site config (title, nav, etc.)
├── @shared/                # System directory (auto-loaded)
│   ├── design/            # CSS (auto-loaded client-side, no imports!)
│   ├── ui/                # Shared components
│   ├── data/              # YAML data (server-side)
│   └── server/            # Backend routes
├── blog/                   # Blog section → /blog/
│   ├── index.md           # /blog/
│   ├── layout.html        # Section-specific layout (optional)
│   └── first-post.md      # /blog/first-post
└── docs/                   # Docs section → /docs/
    ├── index.html         # /docs/
    └── guide.md           # /docs/guide
```

**File-based routing:** Directory structure = URL structure
- `index.html` → `/`
- `blog/index.md` → `/blog/`
- `blog/first-post.md` → `/blog/first-post`
- `docs/guide.md` → `/docs/guide`

**`@shared/` directory (CRITICAL):**
- Fixed name - must be exactly `@shared/` (not `@global/` or anything else)
- CSS in `@shared/design/` auto-loads on ALL pages (no `<link>` needed!)
- Components in `@shared/ui/` available everywhere
- System-wide concerns, not app-specific

**Layouts:**
- `layout.html` in root = site-wide header/footer
- `layout.html` in section (e.g., `blog/layout.html`) = section-specific
- Wraps your content automatically

**Supported files:** `.html`, `.md`, `.css`, `.js`, `.ts`, `.svg`, `.yaml`

## Creating Blogs & Content Sites

### Content Collections

Markdown files in a directory automatically form a collection:

```
blog/
├── index.html           # Lists all posts
├── first-post.md        # Becomes /blog/first-post
└── second-post.md       # Becomes /blog/second-post
```

### Front Matter (YAML Metadata)

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

### Listing Posts

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

### site.yaml Configuration

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

### RSS Feed Generation

With correct `site.yaml` (see above), Nue auto-generates RSS:

1. Set `rss.enabled: true`
2. Specify `rss.collection: posts` (matches your directory)
3. Provide `site.origin` (full URL with https://)
4. Ensure posts have `title`, `date`, `description`

RSS available at `/feed.xml` automatically.

## Common Commands

```bash
nue serve                    # Dev server with hot-reload (port 4000)
nue serve --port 8080        # Custom port
nue serve --production       # Preview without hot-reload
nue build                    # Production build
nue build --dry-run          # Preview what will build
nue stats                    # Production statistics
```

**CLI documents itself:** Run `nue --help` for all options.

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using regular `<!doctype html>` for interactive components | Use `<!doctype dhtml>` for reactivity |
| Creating `.nue` component files (1.0 pattern) | Use HTML files with `<!doctype dhtml>` in 2.0 |
| Naming system directory `@global/` or `@common/` | Must be exactly `@shared/` (fixed name) |
| Manually linking CSS with `<link>` tags | CSS in `@shared/design/` auto-loads (no link needed!) |
| Creating separate component files immediately | Start with inline components, extract when needed |
| Looking for `import` statements | Nue auto-compiles/mounts - no manual imports |
| Using React patterns (useState, JSX) | Use direct variables and HTML attributes |
| Expecting virtual DOM | Nue mutates DOM directly - simpler model |
| Adding build tools | Nue has zero-config dev server built-in |

## When You Need More

**Extracting components:** Use separate HTML files or template partials (see Nuedom docs)
**Shared components:** Place in `@shared/ui/` for cross-app use
**Styling:** CSS auto-loads from same directory or `@shared/design/`
**Data:** Use `.yaml` files in `@shared/data/` for server-side data
**Deep dive:** Full docs at https://nuejs.org/docs/

**Note on `.nue` files:** These are from Nue 1.0. In 2.0, use HTML files with `<!doctype dhtml>` instead.

## Version Notes

Nue 2.0 is significantly different from 1.0:
- New `<!doctype dhtml>` requirement for reactivity
- Changed `:onclick` to `@click` for events
- Automatic compilation/mounting (no manual setup)
- Bun-first architecture

If you see older docs/examples with different syntax, verify the version.
