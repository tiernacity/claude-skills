# Nue 1.x vs 2.x - Version Traps

**⚠️ WARNING**: Nue 2.x differs fundamentally from 1.x. Mixing patterns causes errors.

## Quick Identification

**Nue 2.x (correct):**
- ✅ Files end in `.html`
- ✅ Uses `<!doctype dhtml>`
- ✅ Loops: `:each="item in items"`
- ✅ Events: `@click="handler"`
- ✅ State: `import { state } from 'state'`

**Nue 1.x (obsolete - DO NOT USE):**
- ❌ Files end in `.nue`
- ❌ Loops: `:for="item in items"`
- ❌ Events: `:onclick="handler"`
- ❌ Bare assignments: `count = 0` at component level

## Critical Syntax Differences

| Feature | Nue 1.x ❌ | Nue 2.x ✅ |
|---------|-----------|-----------|
| **Files** | `.nue` | `.html` with `<!doctype dhtml>` |
| **Loops** | `:for` | `:each` |
| **Events** | `:onclick` | `@click` |
| **State** | Bare vars | `state` object or `this.update()` |
| **Lifecycle** | `mounted()` in component | `mounted()` in `<body><script>` |

## Common Traps

### Trap 1: Bare Variable Assignment

```javascript
// ❌ Fails - ReferenceError in ES modules
<script>
  count = 0
</script>

// ✅ Works - use state object
<script>
  import { state } from 'state'
  state.setup({ count: 0 })
</script>
```

### Trap 2: Wrong Loop Syntax

```html
<!-- ❌ 1.x syntax -->
<div :for="item in items">{ item }</div>

<!-- ✅ 2.x syntax -->
<div :each="item in items">{ item }</div>
```

### Trap 3: Lifecycle Location

```javascript
// ❌ Fails - wrong context
<script>
  mounted() { }
</script>

// ✅ Works - inside body
<body>
  <script>
    mounted() { }
  </script>
</body>
```

## Error Diagnosis

**`ReferenceError: <var> is not defined`**
→ Using 1.x bare assignments. Use `state` object.

**`Unexpected token '{'`**
→ Using 1.x `mounted()` syntax. Move to `<body><script>`.

**`Unknown directive :for`**
→ Should be `:each` in 2.x.

## Validation Checklist

Before using external examples, verify:

1. ☐ File is `.html` (not `.nue`)
2. ☐ Has `<!doctype dhtml>`
3. ☐ Uses `:each` (not `:for`)
4. ☐ Uses `@click` (not `:onclick`)
5. ☐ Published 2024+ or mentions "Nue 2.x"

**If ANY check fails, example is 1.x - DO NOT USE.**

## Search Guidance

**✅ Search for:**
- "nue 2.x", "nue dhtml", "nuejs 2024/2025"

**❌ Distrust:**
- Blog posts from 2022-2023
- Examples with `.nue` files
- Documentation without "dhtml"

## Official Resources

- **Docs**: https://nuejs.org/docs/
- **Version**: Run `nue --version` (should be 2.0.0+)
- **SPA Guide**: https://nuejs.org/docs/spa-development
