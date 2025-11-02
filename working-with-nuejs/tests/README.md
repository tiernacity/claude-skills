# Tests for working-with-nuejs Skill

This directory documents the test scenarios used to validate the skill. These serve as regression tests when iterating the skill.

## Testing Methodology

**TDD for Skills**: RED-GREEN-REFACTOR cycle applied to documentation
- **RED**: Test scenario WITH current skill, identify gaps/failures/pain points
- **GREEN**: Update skill to address gaps, retest until passing. Compare output with official templates to validate patterns match.
- **REFACTOR**: Improve skill structure/clarity with no functional changes, verify still GREEN (no regressions)

## Test Scenarios

### 1. Counter App (React Developer Transitioning)

**Scenario**: React developer wants to create a simple counter app in Nue.js
**Purpose**: Validates getting started, reactive components, React transition guidance
**Expected Behavior**:
- Agent uses working-with-nuejs skill immediately
- Creates single HTML file with `<!doctype dhtml>`
- Uses `@click` and `{ }` syntax correctly
- No need for external research or trial-and-error

**Test File**: `counter-test.md`
**Introduced**: v1.0
**Status**: ✅ Passing (v4.0)

---

### 2. Blog Site (Content Collections)

**Scenario**: User wants to create a blog with multiple posts, RSS feed, and shared layout
**Purpose**: Validates building-sites.md - file routing, layouts, collections, front matter, RSS
**Expected Behavior**:
- Agent references building-sites.md for content patterns
- Creates correct `site.yaml` with collections config
- Uses proper front matter (title, date, description)
- Implements site-wide layout correctly
- Generates RSS feed with proper config
- Zero external research needed

**Test File**: `blog-test.md`
**Introduced**: v2.0
**Status**: ✅ Passing (v4.0)

---

### 3. SPA with Client Routing

**Scenario**: User wants a single-page app with client-side routing and API data fetching
**Purpose**: Validates adding-interactivity.md - client routing, state management, data fetching
**Expected Behavior**:
- Agent references adding-interactivity.md for SPA patterns
- Implements History API-based routing correctly
- Uses proper loading/error state patterns
- Handles navigation and popstate events
- All patterns working without external docs

**Test File**: `spa-test.md`
**Introduced**: v3.0
**Status**: ✅ Passing (v4.0)

---

### 4. Static Marketing Site

**Scenario**: User wants simple marketing website (5 static pages, shared header/footer, no blog)
**Purpose**: Validates building-sites.md covers non-blog static sites explicitly
**Expected Behavior**:
- Agent finds guidance immediately (doesn't default to blog patterns)
- Creates simple page structure with layout
- No unnecessary collections or dynamic features
- Clear that this is the right approach for static content

**Test File**: `static-site-test.md`
**Introduced**: v4.0
**Status**: ✅ Passing (v4.0)

---

### 5. REST API Endpoints (Beta)

**Scenario**: User wants to create REST API with Nueserver
**Purpose**: Validates server-side.md - API endpoints, context object, beta warnings
**Expected Behavior**:
- Agent references server-side.md for patterns
- Uses global route functions (get, post, del) correctly
- Understands context object (c.json, c.req.param)
- Provides beta warnings and limitations
- Suggests alternatives if features don't work

**Test File**: `api-test.md`
**Introduced**: v3.0
**Status**: ⚠️ Passing with warnings (beta limitations documented)

---

## Running Tests

### Manual Testing (Current)

Tests are run by spawning subagents with the pressure scenario:

```
I'm going to test whether the working-with-nuejs skill prevents mistakes.

User request: [scenario description]

Instructions:
1. Use the working-with-nuejs skill
2. Complete the task
3. Report any gaps or external research needed

Success criteria:
- Zero external documentation needed
- Correct patterns on first try
- No trial-and-error
```

### Automated Testing (Future)

Consider automating with:
- Subagent scripts that validate outputs
- File structure assertions
- Pattern matching in generated code
- Build/serve verification

## Test Coverage

| Feature | Covered | Test |
|---------|---------|------|
| Getting started | ✅ | Counter |
| Reactive components | ✅ | Counter |
| React transition | ✅ | Counter |
| File-based routing | ✅ | Blog, Static |
| Layouts (site-wide) | ✅ | Blog, Static |
| Layouts (section) | ✅ | Blog |
| Markdown content | ✅ | Blog |
| Front matter | ✅ | Blog |
| Content collections | ✅ | Blog |
| RSS generation | ✅ | Blog |
| Client-side routing | ✅ | SPA |
| State management | ✅ | SPA |
| Data fetching | ✅ | SPA |
| Forms | ⚠️ | (Partially in SPA) |
| REST APIs | ⚠️ | API (beta) |
| Static sites | ✅ | Static |

**Coverage estimate**: ~85% of common use cases

**Not covered**:
- Complex forms with validation
- Middleware/authentication
- Production deployment
- Performance optimization
- Testing strategies
- Component extraction patterns

## Version History

- **v1.0**: Counter test only (basic reactivity)
- **v2.0**: Added blog test (collections, RSS, layouts)
- **v3.0**: Added SPA + API tests, modular structure
- **v4.0**: Added static site test, feature-based organization

## Adding New Tests

When iterating the skill:

1. **Identify gap**: What use case fails with current skill?
2. **Create test scenario**: Document in `tests/[name]-test.md`
3. **RED**: Run test WITH current skill, document failures
4. **GREEN**: Update skill to address gaps
5. **Verify**: Retest scenario until passing
6. **REFACTOR**: Compare with official docs/templates
7. **Regression**: Rerun all previous tests to ensure no breaks
8. **Document**: Update this README with new test

## Regression Testing

Before committing skill changes:

1. Run all test scenarios (Counter, Blog, SPA, Static, API)
2. Verify zero regressions (previously passing tests still pass)
3. Document any intentional breaking changes
4. Update version number if breaking

## Notes

- Tests validate PREVENTION, not just correctness
- Success = agent doesn't need external research
- Failure = agent struggles, searches docs, trial-and-error
- Beta features (APIs) may have documented limitations - that's OK
