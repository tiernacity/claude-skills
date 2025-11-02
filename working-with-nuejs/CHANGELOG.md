# Changelog

All notable changes to the working-with-nuejs skill.

## [v6.1] - 2025-11-02

**Status:** Current version
**Word count:** 2,322 words (41% reduction from v4.0)
**Structure:** 8 atomic workflow guides + reference signpost

### Added
- `common-mistakes.md` (207 words) - Critical debugging guidance
  - @shared/ naming requirement
  - Doctype confusion (dhtml vs html)
  - .nue file warnings
  - React developer gotchas
- Form validation pattern in `adding-reactivity.md` (+46 words)
- CORS middleware example in `creating-apis.md` (+60 words)
- PUT endpoint example in `creating-apis.md` (+60 words)
- Collection data access explanation in `content-collections.md` (+48 words)

### Changed
- Strengthened doctype guidance in `getting-started.md` (+23 words)
- Updated SKILL.md to reference common-mistakes.md

### Fixed
- Test file references (broken after v6.0 refactor)
  - `building-sites.md` → `content-collections.md` and `organizing-projects.md`
  - `adding-interactivity.md` → `adding-reactivity.md` and `building-spas.md`
  - `server-side.md` → `creating-apis.md`

### Validation
- All GREEN regression tests passed (Counter, Blog, SPA, Static Site)
- Zero external research needed during testing
- 100% test pass rate

---

## [v6.0] - 2025-11-02

**Word count:** 1,931 words (51% reduction from v4.0)
**Major restructure:** Feature-based → Atomic workflow

### Breaking Changes
- Deleted `building-sites.md` (958 words)
- Deleted `adding-interactivity.md` (1,203 words)
- Deleted `server-side.md` (626 words)

### Added
- `getting-started.md` (207 words) - Install, first app, commands
- `organizing-projects.md` (229 words) - File routing, layouts, @shared/
- `adding-reactivity.md` (249 words) - Components, state, events
- `building-spas.md` (346 words) - Client routing, data fetching
- `creating-apis.md` (340 words) - REST endpoints (beta)
- `content-collections.md` (219 words) - Blogs, docs, RSS
- `reference-guide.md` (161 words) - Official docs signposts

### Rationale
Agents load ONLY what they need for current workflow step, not entire feature domains. Sequential loading pattern: getting-started → organizing → reactivity → SPAs → APIs → collections.

---

## [v5.0] - 2025-11-02 (NOT IMPLEMENTED)

**Proposed but rejected**
**Reason:** User feedback "Go harder" - word count reduction (22%) insufficient

User feedback: "think about agents and users using the skill first - what use cases they will have, how an agent will use the skill to design and plan, how it will use sub-agents to implement."

Led to complete rethink → v6.0 atomic workflow structure.

---

## [v4.0] - 2025-11-02

**Word count:** 3,965 words
**Structure:** Feature-based organization

### Files
- `building-sites.md` (958 words) - Static sites, blogs, docs
- `adding-interactivity.md` (1,203 words) - Reactive components, SPAs, routing
- `server-side.md` (626 words) - REST APIs

### Added
- Regression test documentation in `tests/`
  - `counter-test.md` - React transition scenario
  - `blog-test.md` - Collections and RSS
  - `spa-test.md` - Client routing
  - `static-site-test.md` - Static marketing site
  - `api-test.md` - REST endpoints
  - `README.md` - TDD methodology

### Rationale
Feature-based organization seemed logical but didn't optimize for agent workflow patterns.

---

## [v3.0] - Earlier

**Structure:** Modular guides (blog, SPA, server-API)

Split monolithic SKILL.md into focused guides. Added SPA and server-side coverage.

---

## [v2.0] - Earlier

**Addition:** Blog and content site support

---

## [v1.0] - Earlier

**Initial version:** Basic Nue.js 2.0 guidance

---

## Key Lessons Learned

1. **Agent workflows ≠ human mental models** - Organize by sequential workflow steps, not logical feature categories
2. **"Go harder" is critical feedback** - First word count reduction attempt (22%) was insufficient, needed 51% to be effective
3. **Competitive review finds everything** - Two competing subagents found issues missed by single review
4. **Reference signposting > duplication** - Point to official docs instead of duplicating comprehensive API specs
5. **Word count as forcing function** - Brutal constraints force brutal clarity
6. **Test references must stay in sync** - When refactoring file structure, grep test files immediately
7. **Common mistakes = high ROI** - Debugging guidance prevents hours of wasted time

## Development Process

### RED-GREEN-REFACTOR for Skills

**RED:** Test with subagent using current skill, identify gaps
**GREEN:** Update skill, retest until passing, validate against templates
**REFACTOR:** Improve structure with no functional changes, verify no regressions

### Quality Gates

1. Competitive subagent review
2. Comparison against previous version
3. GREEN regression tests (all scenarios)
4. Word count validation
5. Git commit with comprehensive message

---

**Next iteration:** TBD - Skill is considered complete for now (2025-11-02)
