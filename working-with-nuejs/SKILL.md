---
name: working-with-nuejs
description: Use when working with Nue.js 2.x (HTML-first reactive framework) - atomic workflow guides for getting started, project organization, reactivity, SPAs, APIs, and content collections. Load only what you need.
---

# Working with Nue.js

Nue.js is a minimalist, HTML-first reactive framework (1MB vs 500MB node_modules). Core principle: **Direct DOM manipulation**, not virtual DOM.

**Version:** 2.0.0-beta.2
**Docs:** https://nuejs.org/docs/
**Runtime:** Bun 1.2+

## Atomic Workflow Guides

Load ONLY what you need for current step:

**@getting-started.md** - Install, first app, commands
**@organizing-projects.md** - File routing, layouts, `@shared/`
**@adding-reactivity.md** - Components, state, events, forms
**@building-spas.md** - Client routing, data fetching
**@creating-apis.md** - REST endpoints (beta)
**@content-collections.md** - Blogs, docs, portfolios
**@common-mistakes.md** - Frequent errors that waste hours
**@reference-guide.md** - When to use official Reference docs

## Quick Example

```html
<!doctype dhtml>
<button @click="count++">{ count }</button>
<script>count = 0</script>
```

`<!doctype dhtml>` = reactive, `@click` = events, `{ }` = binding, `<script>` = state

## Version Note

Nue 2.0 differs from 1.0: New `<!doctype dhtml>` for reactivity (not `.nue` files), `@click` events (not `:onclick`), automatic compilation.

## Skill Maintenance

Regression tests in `tests/` directory. See `tests/README.md` before updating.
