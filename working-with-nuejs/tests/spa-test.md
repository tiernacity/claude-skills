# Test: SPA with Client Routing

**Version introduced**: v3.0
**Last verified**: v4.0
**Status**: ✅ Passing

## Scenario

User wants to build a single-page application with client-side routing and data fetching from an API.

## User Prompt

```
I want to create a single-page app in Nue.js with client-side routing.

Requirements:
- Routes: / (home), /users (list), /users/:id (detail)
- Fetch user data from /api/users
- No full page reloads when navigating
- Loading and error states
- Back/forward button support
```

## Success Criteria

1. ✅ Agent references adding-reactivity.md and building-spas.md for routing
2. ✅ Implements History API pattern correctly
3. ✅ Uses `window.history.pushState()` for navigation
4. ✅ Listens to `popstate` for back/forward
5. ✅ Implements proper loading/error state pattern
6. ✅ Uses `mounted()` lifecycle for initialization
7. ✅ Data fetching with try/catch/finally
8. ✅ Conditional rendering based on currentView
9. ✅ Zero external research needed

## Expected Output

**Key patterns in code**:

```html
<!doctype dhtml>
<script>
  // Routing state
  currentView = 'home'
  currentParams = {}

  // Navigation
  function navigate(path) {
    currentView = pathToView(path)
    window.history.pushState({}, '', path)
  }

  // Back/forward support
  window.addEventListener('popstate', () => {
    currentView = pathToView(window.location.pathname)
  })

  // Data fetching
  users = []
  loading = false
  error = null

  async function fetchUsers() {
    loading = true
    error = null
    try {
      const response = await fetch('/api/users')
      if (!response.ok) throw new Error(`HTTP ${response.status}`)
      users = await response.json()
    } catch (e) {
      error = e.message
    } finally {
      loading = false
    }
  }

  // Initialize
  mounted() {
    currentView = pathToView(window.location.pathname)
    await fetchUsers()
  }
</script>

<!-- Navigation -->
<nav>
  <a href="/" @click.prevent="navigate('/')">Home</a>
  <a href="/users" @click.prevent="navigate('/users')">Users</a>
</nav>

<!-- Views -->
<div :if="currentView === 'home'">Home</div>
<div :if="currentView === 'users'">
  <div :if="loading">Loading...</div>
  <div :if="error">Error: { error }</div>
  <div :if="!loading && !error">
    <!-- User list -->
  </div>
</div>
```

## RED Phase Result (v2.0 - Without SPA Coverage)

- 0% coverage for client-side routing
- Required 30-40 minutes of research
- Confusion about routing approach
- No guidance on state management patterns
- Data fetching patterns unclear
- Trial and error with lifecycle methods

## GREEN Phase Result (v3.0 - With adding-reactivity.md and building-spas.md)

- Immediate reference to client routing section
- Correct History API usage on first try
- Proper loading/error/success state pattern
- All patterns working without external docs
- Clean implementation matching examples

## Regression Testing

Run this test when:
- Updating adding-reactivity.md and building-spas.md
- Changing routing examples
- Modifying state management guidance
- Data fetching patterns updated
- Lifecycle documentation changes

## Notes

- Tests most complex interactive scenario
- Validates entire adding-reactivity.md and building-spas.md module
- Good test for state management patterns
- History API integration is subtle - easy to get wrong
