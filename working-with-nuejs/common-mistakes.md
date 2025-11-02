# Common Mistakes in Nue.js

Frequent errors that waste hours. Check this list first.

## Critical Naming Issues

**@shared/ directory name is FIXED**
- ❌ `shared/`, `@global/`, `@common/`
- ✅ `@shared/` (at-sign required)

Nue won't recognize other names. CSS won't auto-load, components won't be available.

## Doctype Confusion

**Using wrong doctype for reactive components**
- ❌ `<!doctype html>` for interactive apps
- ✅ `<!doctype dhtml>` for reactivity

Regular `html` = server-side rendering (static). Use `dhtml` for client-side reactivity.

## Version-Specific Issues

**Creating .nue component files**
- ❌ `.nue` files (Nue 1.0 pattern)
- ✅ HTML files with `<!doctype dhtml>` (Nue 2.0)

If you see old examples with `.nue` extensions, they're outdated.

## React Developer Mistakes

**Looking for imports/mounting**
- ❌ `import Component from './component'`
- ✅ No imports needed - Nue auto-compiles

**Using hooks and JSX**
- ❌ `useState()`, `useEffect()`, JSX syntax
- ✅ Direct variables: `count = 0`

## Build Tool Confusion

**Adding webpack/vite**
- ❌ Installing build tools manually
- ✅ `nue serve` has zero-config dev server built-in

## CSS Mistakes

**Manually linking CSS**
- ❌ `<link rel="stylesheet" href="style.css">`
- ✅ CSS in `@shared/design/` auto-loads (no tags needed)

## When You Need More

**Debugging tips** → Check console for Nue errors
**Official docs** → reference-guide.md
