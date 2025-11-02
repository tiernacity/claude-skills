# Adding Reactivity to Nue.js

Reactive components, state, events, and forms.

## Basic Reactive Component

```html
<!doctype dhtml>

<button @click="count++">{ count }</button>
<script>count = 0</script>
```

**Syntax:**
- `<!doctype dhtml>` = reactive mode
- `@click` = event handler
- `{ }` = data binding
- `<script>` = state initialization

## Syntax Quick Reference

| Feature | Syntax |
|---------|--------|
| Event | `@click="count++"` |
| Binding | `{ count }` |
| Conditional | `:if="count > 0"` |
| Loop | `:for="item in items"` |
| Two-way | `value="{ name }" @input="name = $event.target.value"` |

## State Management

**Simple:**
```javascript
isOpen = false
count = 0
```

**Complex:**
```javascript
state = { user: null, cart: [] }

function login(user) {
  state.user = user
}
```

## Forms

```html
<script>
  formData = { name: '', email: '' }
  errors = {}
  submitting = false

  function validate() {
    errors = {}
    if (!formData.name) errors.name = 'Required'
    if (!formData.email.includes('@')) errors.email = 'Invalid email'
    return Object.keys(errors).length === 0
  }

  async function handleSubmit(e) {
    e.preventDefault()
    if (!validate()) return

    submitting = true
    try {
      await fetch('/api/users', {
        method: 'POST',
        body: JSON.stringify(formData)
      })
    } finally {
      submitting = false
    }
  }
</script>

<form @submit.prevent="handleSubmit($event)">
  <input
    value="{ formData.name }"
    @input="formData.name = $event.target.value"
  />
  <span :if="errors.name" class="error">{ errors.name }</span>

  <input
    value="{ formData.email }"
    @input="formData.email = $event.target.value"
  />
  <span :if="errors.email" class="error">{ errors.email }</span>

  <button :disabled="submitting">
    { submitting ? 'Submitting...' : 'Submit' }
  </button>
</form>
```

## Lifecycle

```javascript
mounted() {
  // Initialize, fetch data
}

unmounted() {
  // Cleanup
}
```

## Common Patterns

**Tabs:**
```javascript
activeTab = 'tab1'
```
```html
<div :if="activeTab === 'tab1'">Content</div>
```

**Search:**
```javascript
searchQuery = ''
get filtered() {
  return items.filter(i => i.name.includes(searchQuery))
}
```

## When You Need More

**SPAs with routing** → building-spas.md
**Full HTML syntax** → reference-guide.md
