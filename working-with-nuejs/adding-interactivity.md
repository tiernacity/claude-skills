# Adding Interactivity to Nue.js Sites

Guide for creating reactive components, managing state, and building interactive user interfaces.

## Reactive Components

Make your pages interactive with Nue.js's reactive system:

### Basic Counter Example

```html
<!doctype dhtml>

<div>
  <button @click="count--">-</button>
  <b>{ count }</b>
  <button @click="count++">+</button>

  <script>
    // Initialize state
    count = 0
  </script>
</div>
```

**Key concepts:**
- `<!doctype dhtml>` enables reactivity (client-side rendering)
- `@click` for event handlers
- `{ variable }` for data binding
- `<script>` block defines state
- State changes trigger automatic DOM updates

### Component Syntax

| Pattern | Syntax | Example |
|---------|--------|---------|
| Event handler | `@click="expression"` | `@click="count++"` |
| Data binding | `{ variable }` | `<b>{ count }</b>` |
| Initialize state | `<script>` block | `<script>count = 0</script>` |
| Conditional | `:if="condition"` | `<p :if="count > 0">Positive</p>` |
| Loop | `:for="item in items"` | `<li :for="todo in todos">{ todo }</li>` |
| Two-way binding | `value` + `@input` | `value="{ name }" @input="name = $event.target.value"` |

## State Management

### Simple State

For basic components:

```html
<script>
  // Reactive variables
  isOpen = false
  count = 0
  name = ''
</script>

<button @click="isOpen = !isOpen">Toggle</button>
<div :if="isOpen">Content</div>
```

### Complex State

For larger applications:

```html
<script>
  // State object
  state = {
    user: null,
    isAuthenticated: false,
    cart: [],
    notifications: []
  }

  // Actions
  function login(user) {
    state.user = user
    state.isAuthenticated = true
  }

  function logout() {
    state.user = null
    state.isAuthenticated = false
  }

  function addToCart(item) {
    state.cart = [...state.cart, item]
  }
</script>

<div>
  <div :if="state.isAuthenticated">
    <p>Welcome, { state.user.name }!</p>
    <button @click="logout()">Logout</button>
  </div>

  <div :if="!state.isAuthenticated">
    <button @click="showLoginForm()">Login</button>
  </div>

  <div class="cart">
    Cart ({ state.cart.length })
  </div>
</div>
```

## Client-Side Routing

For single-page applications without full page reloads:

```html
<!doctype dhtml>

<script>
  // State for current view
  currentView = 'home'
  currentParams = {}

  // Navigation function
  function navigate(path) {
    currentView = pathToView(path)
    window.history.pushState({}, '', path)
  }

  // Map paths to views
  function pathToView(path) {
    if (path === '/') return 'home'
    if (path === '/users') return 'users'
    if (path.startsWith('/users/')) {
      currentParams.id = path.split('/')[2]
      return 'user-detail'
    }
    return '404'
  }

  // Handle back/forward buttons
  window.addEventListener('popstate', () => {
    currentView = pathToView(window.location.pathname)
  })

  // Initialize on mount
  mounted() {
    currentView = pathToView(window.location.pathname)
  }
</script>

<body>
  <!-- Navigation -->
  <nav>
    <a href="/" @click.prevent="navigate('/')">Home</a>
    <a href="/users" @click.prevent="navigate('/users')">Users</a>
  </nav>

  <!-- Views -->
  <main>
    <div :if="currentView === 'home'">
      <h1>Welcome Home</h1>
    </div>

    <div :if="currentView === 'users'">
      <h1>User List</h1>
    </div>

    <div :if="currentView === 'user-detail'">
      <h1>User Details</h1>
      <p>User ID: { currentParams.id }</p>
    </div>

    <div :if="currentView === '404'">
      <h1>404 - Not Found</h1>
    </div>
  </main>
</body>
```

**Key points:**
- Use `@click.prevent` to prevent default link behavior
- Update `window.history` when navigating
- Listen to `popstate` for back/forward buttons
- Initialize view on `mounted()`

**Note:** This is client-side routing for SPAs. For multi-page sites, use file-based routing (see building-sites.md).

## Data Fetching

Pattern for loading data from APIs:

```html
<script>
  // State
  users = []
  loading = false
  error = null

  // Fetch function
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

  // Fetch on mount
  async mounted() {
    await fetchUsers()
  }
</script>

<div>
  <!-- Loading state -->
  <div :if="loading">
    <p>Loading...</p>
  </div>

  <!-- Error state -->
  <div :if="error">
    <p class="error">Error: { error }</p>
    <button @click="fetchUsers()">Retry</button>
  </div>

  <!-- Success state -->
  <div :if="!loading && !error">
    <div :for="user in users">
      <h3>{ user.name }</h3>
      <p>{ user.email }</p>
    </div>
  </div>
</div>
```

**Pattern:**
1. State: `data`, `loading`, `error`
2. Try/catch/finally for async operations
3. Conditional rendering based on state
4. Retry button in error state

## Forms

### Simple Form

```html
<script>
  formData = {
    name: '',
    email: ''
  }

  function handleSubmit(e) {
    e.preventDefault()
    console.log('Submitted:', formData)
    // Process form data
  }
</script>

<form @submit.prevent="handleSubmit($event)">
  <input
    type="text"
    value="{ formData.name }"
    @input="formData.name = $event.target.value"
    placeholder="Name"
  />

  <input
    type="email"
    value="{ formData.email }"
    @input="formData.email = $event.target.value"
    placeholder="Email"
  />

  <button type="submit">Submit</button>
</form>
```

### Form with Async Submission

```html
<script>
  formData = { name: '', email: '' }
  submitting = false
  success = false

  async function handleSubmit(e) {
    e.preventDefault()
    submitting = true
    success = false

    try {
      const response = await fetch('/api/users', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(formData)
      })

      if (!response.ok) throw new Error('Submit failed')

      success = true
      formData = { name: '', email: '' } // Reset form
    } catch (e) {
      alert('Error: ' + e.message)
    } finally {
      submitting = false
    }
  }
</script>

<form @submit.prevent="handleSubmit($event)">
  <!-- Form fields -->

  <button type="submit" :disabled="submitting">
    { submitting ? 'Submitting...' : 'Submit' }
  </button>

  <div :if="success" class="success">Success!</div>
</form>
```

## Component Lifecycle

```html
<script>
  // Called when component is mounted to DOM
  mounted() {
    console.log('Component mounted')
    // Initialize, fetch data, setup listeners
  }

  // Called before component is removed
  unmounted() {
    console.log('Component unmounted')
    // Cleanup, remove listeners
  }
</script>
```

## Component Communication

For passing data between components:

```html
<!-- Parent component -->
<script>
  selectedUser = null
</script>

<div>
  <user-list @select="(user) => selectedUser = user"/>
  <user-detail :if="selectedUser" :user="selectedUser"/>
</div>
```

## Common Patterns

### Debounced Input

```javascript
let timeout
function handleInput(value) {
  clearTimeout(timeout)
  timeout = setTimeout(() => {
    search(value)
  }, 300)
}
```

### Pagination

```javascript
currentPage = 1
pageSize = 10

get paginatedData() {
  const start = (currentPage - 1) * pageSize
  return allData.slice(start, start + pageSize)
}
```

### Search/Filter

```javascript
searchQuery = ''

get filteredUsers() {
  return users.filter(u =>
    u.name.toLowerCase().includes(searchQuery.toLowerCase())
  )
}
```

### Sorting

```javascript
sortField = 'name'
sortAsc = true

get sortedData() {
  return [...data].sort((a, b) => {
    const mult = sortAsc ? 1 : -1
    return a[sortField] > b[sortField] ? mult : -mult
  })
}
```

### Tabs/Accordion

```javascript
activeTab = 'tab1'

function switchTab(tab) {
  activeTab = tab
}
```

```html
<div class="tabs">
  <button @click="switchTab('tab1')" :class="{ active: activeTab === 'tab1' }">
    Tab 1
  </button>
  <button @click="switchTab('tab2')" :class="{ active: activeTab === 'tab2' }">
    Tab 2
  </button>
</div>

<div :if="activeTab === 'tab1'">Tab 1 content</div>
<div :if="activeTab === 'tab2'">Tab 2 content</div>
```

## Doctype: dhtml vs html

**Use `<!doctype dhtml>` when:**
- Creating interactive/reactive components
- Need client-side rendering
- Building SPAs or dynamic UIs

**Use regular `<!doctype html>` when:**
- Static pages (server-side rendering)
- Content-focused pages (blogs, docs) - see building-sites.md
- No interactivity needed

## For React Developers

If you're coming from React, unlearn these patterns:

| React Pattern | Nue.js Pattern | Why Different |
|--------------|----------------|---------------|
| `useState(0)` | `count = 0` in `<script>` | Direct variable, no hooks |
| `onClick={() => ...}` | `@click="..."` | HTML attribute, not prop |
| JSX `<MyComponent />` | Inline HTML | HTML-first, not JS-first |
| Import/mount | Automatic | No manual mounting needed |
| Virtual DOM | Direct DOM | Nue mutates DOM directly |
| useEffect | `mounted()`, `unmounted()` | Simpler lifecycle |

**Don't overcomplicate** - Nue is intentionally simpler than React. Direct variables, HTML attributes, straightforward patterns.

## When to Use Interactivity

**Good use cases:**
- Forms with validation
- Interactive dashboards
- Single-page applications
- Real-time updates
- User input handling

**Consider static approach (building-sites.md) for:**
- Blogs and content sites
- Marketing pages
- Documentation
- Portfolios

Interactivity adds complexity. Use it when it adds value.
