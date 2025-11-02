# Organizing Nue.js Projects

File-based routing, layouts, and the `@shared/` directory.

## File-Based Routing

Directory structure = URLs:

```
my-site/
├── index.html          → /
├── about.html          → /about
└── blog/
    ├── index.html      → /blog/
    └── post.md         → /blog/post
```

No configuration needed. File extensions omitted from URLs.

## The @shared Directory

System directory for site-wide assets:

```
@shared/
├── design/          # CSS (auto-loads globally!)
├── ui/              # Shared components
├── data/            # YAML data
└── server/          # Backend routes
```

**Critical:**
- Must be named exactly `@shared/` (not `@global/`, `@common/`)
- CSS in `@shared/design/` auto-loads on all pages (no `<link>` tags)

## Layouts

Shared structure (headers, footers):

**Site-wide** - `layout.html` in root:
```html
<!doctype html>
<header><nav><!-- nav --></nav></header>
<main><!-- content --></main>
<footer><!-- footer --></footer>
```

**Section-specific** - `blog/layout.html`:
```html
<!doctype html>
<header><h1>Blog</h1></header>
<article><!-- post --></article>
```

Hierarchy: Section → Site → None

## Markdown Pages

`.md` files auto-convert to HTML:

```markdown
---
title: My Page
date: 2025-01-15
---

# Content here
```

Access in templates: `<h1>{ title }</h1>`

## Doctype Choice

**`<!doctype html>`** = Static/server-side (blogs, marketing, docs)
**`<!doctype dhtml>`** = Interactive/client-side (see adding-reactivity.md)

## Common Patterns

**Static site:**
```
my-site/
├── @shared/design/style.css
├── layout.html
├── index.html
└── about.html
```

**Multi-section:**
```
my-site/
├── @shared/design/
├── layout.html
├── blog/layout.html
└── docs/layout.html
```

## When You Need More

**Collections (blogs/docs)** → content-collections.md
**Reactivity** → adding-reactivity.md
**Full config options** → reference-guide.md
