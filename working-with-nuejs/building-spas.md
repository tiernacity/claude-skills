# Building SPAs with Nue.js

Client-side routing and data fetching for single-page applications.

## Client-Side Routing

```html
<!doctype dhtml>

<script>
  currentView = 'home'
  currentParams = {}

  function navigate(path) {
    currentView = pathToView(path)
    window.history.pushState({}, '', path)
  }

  function pathToView(path) {
    if (path === '/') return 'home'
    if (path === '/users') return 'users'
    if (path.startsWith('/users/')) {
      currentParams.id = path.split('/')[2]
      return 'user-detail'
    }
    return '404'
  }

  window.addEventListener('popstate', () => {
    currentView = pathToView(window.location.pathname)
  })

  mounted() {
    currentView = pathToView(window.location.pathname)
  }
</script>

<nav>
  <a href="/" @click.prevent="navigate('/')">Home</a>
  <a href="/users" @click.prevent="navigate('/users')">Users</a>
</nav>

<main>
  <div :if="currentView === 'home'">Home</div>
  <div :if="currentView === 'users'">Users</div>
  <div :if="currentView === 'user-detail'">
    User { currentParams.id }
  </div>
</main>
```

**Key points:**
- `@click.prevent` = prevent default navigation
- `pushState` = update URL
- `popstate` = handle back/forward
- Initialize in `mounted()`

## Data Fetching Pattern

```html
<script>
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

  async mounted() {
    await fetchUsers()
  }
</script>

<div>
  <div :if="loading">Loading...</div>
  <div :if="error">
    Error: { error }
    <button @click="fetchUsers()">Retry</button>
  </div>
  <div :if="!loading && !error">
    <div :for="user in users">{ user.name }</div>
  </div>
</div>
```

**Standard pattern:** State (`data`, `loading`, `error`) + try/catch/finally + conditional rendering

## Combining Routing + Data

```javascript
function navigate(path) {
  currentView = pathToView(path)
  window.history.pushState({}, '', path)

  if (currentView === 'users') fetchUsers()
  else if (currentView === 'user-detail') fetchUser(currentParams.id)
}
```

## Common SPA Patterns

**Pagination:**
```javascript
currentPage = 1
pageSize = 10

get paginatedData() {
  const start = (currentPage - 1) * pageSize
  return allData.slice(start, start + pageSize)
}
```

**Search with debounce:**
```javascript
let timeout
function handleSearch(value) {
  clearTimeout(timeout)
  timeout = setTimeout(() => {
    searchQuery = value
    fetchResults()
  }, 300)
}
```

## When to Use SPAs

**Good for:** Interactive dashboards, apps with complex state, real-time tools
**Consider multi-page (organizing-projects.md) for:** Content sites, marketing, SEO-critical pages

## When You Need More

**Backend APIs** → creating-apis.md
**Advanced state** → reference-guide.md
