# Test: Static Marketing Site

**Version introduced**: v4.0
**Last verified**: v4.0
**Status**: ✅ Passing

## Scenario

User wants a simple marketing website with static pages, no blog, no dynamic features.

## User Prompt

```
I need a simple marketing website with Nue.js.

Requirements:
- 5 static pages: Home, Features, Pricing, About, Contact
- Shared header with navigation and footer
- Clean design
- No blog, no collections, just static content
```

## Success Criteria

1. ✅ Agent references content-collections.md and organizing-projects.md
2. ✅ Finds static site pattern (not defaulting to blog)
3. ✅ Creates simple structure without unnecessary complexity
4. ✅ Uses layout.html for shared header/footer
5. ✅ Does NOT create collections or site.yaml (unless needed for meta)
6. ✅ Clear explanation that this is appropriate for static content
7. ✅ Zero confusion about whether to use blog patterns

## Expected Output

**Directory structure**:
```
my-site/
├── @shared/design/
│   └── style.css
├── layout.html
├── index.html
├── features.html
├── pricing.html
├── about.html
└── contact.html
```

**layout.html**:
```html
<!doctype html>
<header>
  <nav>
    <a href="/">Home</a>
    <a href="/features">Features</a>
    <a href="/pricing">Pricing</a>
    <a href="/about">About</a>
    <a href="/contact">Contact</a>
  </nav>
</header>

<main>
  <!-- Page content -->
</main>

<footer>
  <p>&copy; 2025 My Site</p>
</footer>
```

**Pages**: Simple HTML files with content (no front matter, no collections)

## RED Phase Result (v3.0 - Use-Case Organization)

- No explicit static site guidance
- Agent defaulted to blog patterns (unnecessary)
- Created site.yaml when not needed
- Confusion about whether this was a "blog without posts"

## GREEN Phase Result (v4.0 - Feature-Based Organization)

- Immediate reference to content-collections.md and organizing-projects.md
- Found static marketing site pattern (lines 314-329)
- Created simple structure without blog overhead
- Clear statement: "No collections, no dynamic content - just pages"
- Agent quote: "Feature-based organization made it obvious where to look"

## Regression Testing

Run this test when:
- Updating content-collections.md and organizing-projects.md
- Changing examples or patterns
- Reorganizing skill structure
- Layout documentation changes

## Notes

- This test validates the v4.0 feature-based reorganization
- Previously, "blog-guide.md" was too specific
- Now content-collections.md and organizing-projects.md covers static sites, blogs, AND docs
- Tests that skill doesn't over-engineer simple use cases
