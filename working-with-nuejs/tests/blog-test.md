# Test: Blog Site (Content Collections)

**Version introduced**: v2.0
**Last verified**: v4.0
**Status**: ✅ Passing

## Scenario

User wants to create a blog with multiple posts, RSS feed, and shared header/footer.

## User Prompt

```
I want to create a blog with Nue.js.

Requirements:
- Homepage listing all posts (newest first)
- Individual post pages from markdown
- Shared header and footer (site title, navigation)
- RSS feed for subscribers
- Posts should have title, date, description
```

## Success Criteria

1. ✅ Agent references building-sites.md (not just SKILL.md)
2. ✅ Creates correct directory structure (posts/, layout.html, site.yaml)
3. ✅ Configures collection in site.yaml with proper sort
4. ✅ Uses front matter correctly (title, date, description)
5. ✅ Enables RSS with required fields (origin, collection)
6. ✅ Implements layout.html for shared structure
7. ✅ Zero external documentation needed
8. ✅ Explains file-based routing clearly

## Expected Output

**Directory structure**:
```
my-blog/
├── @shared/design/style.css
├── layout.html
├── index.html
├── posts/
│   ├── first-post.md
│   ├── second-post.md
│   └── third-post.md
└── site.yaml
```

**site.yaml**:
```yaml
meta:
  title: My Blog
  author: Your Name

site:
  origin: https://myblog.com

rss:
  enabled: true
  title: My Blog
  description: Latest posts
  collection: posts

collections:
  posts:
    include: [ posts/ ]
    require: [ title ]
    sort: date desc
```

**posts/first-post.md**:
```markdown
---
title: First Post
date: 2025-01-15
description: My first blog post
---

# Content here
```

**index.html** (or similar listing):
```html
<div :for="post in posts">
  <h3><a :href="post.url">{ post.title }</a></h3>
  <time>{ post.date }</time>
  <p>{ post.description }</p>
</div>
```

## RED Phase Result (v1.0 - Current Skill Tested Against Blog Scenario)

- 60% of patterns missing from skill
- Agent required 30-40 minutes external research
- Confusion about site.yaml configuration
- Didn't know about collections system
- RSS configuration unclear
- Manual work to figure out structure
- **Pain point:** No guidance on content organization

## GREEN Phase Result (v2.0 - Skill Updated, Retested)

- Immediate reference to building-sites.md (new module)
- Correct structure on first try
- All required config fields present
- RSS working immediately
- Zero external research needed
- **Validated against template:** Structure matches official blog template

## REFACTOR Phase Result (v4.0 - Structure Changed, Functionality Unchanged)

- Reorganized from use-case (blog-guide.md) to feature-based (building-sites.md)
- No functional changes to blog coverage
- Retested: Still GREEN ✅ (zero regressions)
- Improvement: Now also covers static sites explicitly

## Regression Testing

Run this test when:
- Updating building-sites.md
- Changing collections documentation
- Modifying RSS guidance
- Layout system changes
- Front matter examples updated

## Notes

- Tests most complex content site scenario
- Validates entire building-sites.md module
- RSS is a good integration test (requires origin, collection, front matter)
