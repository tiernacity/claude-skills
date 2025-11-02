# Nue.js Single-Page Application Guide

Guide for creating SPAs with client-side routing and state management.

## Client-Side Routing

Nue.js doesn't provide a built-in router for SPAs. Use the browser History API:

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
      <!-- User list content -->
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
- Initialize view on mount

## Data Fetching

Pattern for fetching data from APIs:

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

## State Management

For complex state, use reactive objects:

```html
<script>
  // Application state
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

  function addNotification(message) {
    state.notifications = [...state.notifications, {
      id: Date.now(),
      message,
      timestamp: new Date()
    }]
  }
</script>

<div>
  <!-- Conditional rendering based on state -->
  <div :if="state.isAuthenticated">
    <p>Welcome, { state.user.name }!</p>
    <button @click="logout()">Logout</button>
  </div>

  <div :if="!state.isAuthenticated">
    <button @click="showLoginForm()">Login</button>
  </div>

  <!-- Cart count -->
  <div class="cart">
    Cart ({ state.cart.length })
  </div>

  <!-- Notifications -->
  <div :for="notif in state.notifications">
    { notif.message }
  </div>
</div>
```

## Loading States Pattern

Standard pattern for async operations:

```javascript
// State
data = null
loading = false
error = null

// Async action
async function loadData() {
  loading = true
  error = null
  try {
    data = await fetchFromAPI()
  } catch (e) {
    error = e.message
  } finally {
    loading = false
  }
}
```

Template:
```html
<div :if="loading">Loading...</div>
<div :if="error">Error: { error }</div>
<div :if="data && !loading">
  <!-- Success content -->
</div>
```

## Form Handling

```html
<script>
  formData = {
    name: '',
    email: ''
  }

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

  <button type="submit" :disabled="submitting">
    { submitting ? 'Submitting...' : 'Submit' }
  </button>

  <div :if="success">Success!</div>
</form>
```

## Component Communication

Pass data between components with props:

```html
<!-- Parent component -->
<script>
  selectedUser = null
</script>

<div>
  <user-list @select="(user) => selectedUser = user"/>
  <user-detail :if="selectedUser" :user="selectedUser"/>
</div>

<!-- user-list component -->
<div :is="user-list">
  <div :for="user in users" @click="$emit('select', user)">
    { user.name }
  </div>
</div>

<!-- user-detail component -->
<div :is="user-detail">
  <h1>{ user.name }</h1>
  <p>{ user.email }</p>
</div>
```

## Performance Tips

1. **Debounce input handlers:**
```javascript
let timeout
function handleInput(value) {
  clearTimeout(timeout)
  timeout = setTimeout(() => {
    search(value)
  }, 300)
}
```

2. **Lazy load data:**
```javascript
async mounted() {
  // Only fetch when view is mounted
  if (!this.data) {
    await this.fetchData()
  }
}
```

3. **Minimize reactivity:**
```javascript
// Use plain objects for non-reactive data
const constants = { API_URL: 'https://api.example.com' }

// Only make state reactive that needs to trigger re-renders
state = { activeView: 'home' }
```

## Common Patterns

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
