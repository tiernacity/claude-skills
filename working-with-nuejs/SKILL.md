---
name: working-with-nuejs
description: Use when working with Nue.js 2.x (HTML-first reactive framework) - atomic workflow guides for getting started, project organization, reactivity, SPAs, APIs, and content collections. Load only what you need.
---

# Working with Nue.js 2.x

<CRITICAL>
**This skill is for Nue.js 2.x ONLY.** Nue 1.x patterns DO NOT WORK and cause errors.

**BEFORE using ANY external example:**
1. Verify it uses `<!doctype dhtml>` (not `.nue` files)
2. Check loops use `:each` (not `:for`)
3. Confirm events use `@click` (not `:onclick`)
4. If unsure, read **@version-confusion.md** FIRST

**When searching for help:** Only use resources from 2024+ and verify they mention "Nue 2.x" or "dhtml".
</CRITICAL>

Nue.js is a minimalist, HTML-first reactive framework (1MB vs 500MB node_modules). Core principle: **Direct DOM manipulation**, not virtual DOM.

**Version:** 2.0.0-beta.2
**Docs:** https://nuejs.org/docs/
**Runtime:** Bun 1.2+

## Atomic Workflow Guides

Load ONLY what you need for current step:

**@version-confusion.md** - **READ FIRST** - Critical v1.x vs v2.x differences
**@getting-started.md** - Install, first app, commands
**@organizing-projects.md** - File routing, layouts, `@shared/`
**@adding-reactivity.md** - Components, state, events, forms
**@building-spas.md** - Client routing, data fetching
**@creating-apis.md** - REST endpoints (beta)
**@content-collections.md** - Blogs, docs, portfolios
**@common-mistakes.md** - Frequent errors that waste hours
**@reference-guide.md** - When to use official Reference docs

## Nue 2.x Quick Example

```html
<!doctype dhtml>

<body>
  <button @click="count++">{ count }</button>

  <script>
    count = 0

    mounted() {
      console.log('App mounted')
    }
  </script>
</body>
```

`<!doctype dhtml>` = reactive, `@click` = events, `{ }` = binding, `<body><script>` = lifecycle

## CRITICAL Version Requirement

**Nue 2.x ONLY.** If you encounter:
- Examples with `.nue` files → Nue 1.x, DO NOT USE
- Syntax like `:for` or `:onclick` → Nue 1.x, DO NOT USE
- Bare `count = 0` outside `<body><script>` → May fail, verify context

**Always validate** examples are Nue 2.x before using.

## Skill Maintenance

Regression tests in `tests/` directory. See `tests/README.md` before updating.
