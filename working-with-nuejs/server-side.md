# Nue.js Server-Side API Guide

Guide for creating REST API endpoints with Nueserver.

⚠️ **Beta Warning:** Server features are in beta in 2.0.0-beta.2. Some functionality may not work as documented. Test thoroughly and consider using a separate backend (Express, Hono) if you encounter issues.

## Server Directory Structure

API routes go in the `server/` directory:

```
my-app/
├── server/
│   ├── index.js       # API routes
│   └── data/
│       └── users.json # Mock data
├── index.html
└── site.yaml
```

## site.yaml Configuration

Enable server routes:

```yaml
server:
  dir: server        # Directory for server files
  reload: true       # Auto-reload on changes
```

## Creating API Endpoints

Nueserver provides global routing functions (no imports needed):

**server/index.js:**
```javascript
// Mock data
const users = [
  { id: '1', name: 'Alice', email: 'alice@example.com' },
  { id: '2', name: 'Bob', email: 'bob@example.com' }
]

// GET all users
get('/api/users', async (c) => {
  return c.json(users)
})

// GET single user
get('/api/users/:id', async (c) => {
  const id = c.req.param('id')
  const user = users.find(u => u.id === id)
  if (!user) return c.json({ error: 'User not found' }, 404)
  return c.json(user)
})

// POST create user
post('/api/users', async (c) => {
  const data = await c.req.json()
  const user = { id: String(users.length + 1), ...data }
  users.push(user)
  return c.json(user, 201)
})

// DELETE user
del('/api/users/:id', async (c) => {
  const id = c.req.param('id')
  const index = users.findIndex(u => u.id === id)
  if (index === -1) return c.json({ error: 'Not found' }, 404)
  users.splice(index, 1)
  return c.json({ success: true })
})
```

## Global Route Functions

Available globally in server files:

- `get(path, handler)` - Handle GET requests
- `post(path, handler)` - Handle POST requests
- `put(path, handler)` - Handle PUT requests
- `del(path, handler)` - Handle DELETE requests
- `use(path, middleware)` - Apply middleware

## Context Object (c)

The handler receives a context object with these methods:

### Response Methods
- `c.json(data, status?)` - Return JSON response
- `c.text(text, status?)` - Return plain text
- `c.html(html, status?)` - Return HTML

### Request Methods
- `c.req.param(name)` - Get URL parameter (`:id` → `c.req.param('id')`)
- `c.req.json()` - Parse JSON request body
- `c.req.header(name)` - Get request header
- `c.req.query(name)` - Get query parameter

### Environment
- `c.env` - Access environment variables and bindings (Cloudflare Workers pattern)

## Middleware Example

```javascript
// Authentication middleware
use('/api/admin/*', async (c, next) => {
  const token = c.req.header('Authorization')
  if (!isValidToken(token)) {
    return c.json({ error: 'Unauthorized' }, 401)
  }
  await next()
})

// Protected route
get('/api/admin/users', async (c) => {
  return c.json(await getAllUsers())
})
```

## Cloudflare Workers Pattern

Nueserver follows Cloudflare Workers conventions:

```javascript
get('/api/data', async (c) => {
  // Access D1 database
  const { db } = c.env
  const users = await db.prepare('SELECT * FROM users').all()

  // Access KV storage
  const { cache } = c.env
  const cached = await cache.get('users')

  return c.json(users)
})
```

## Error Handling

```javascript
get('/api/users/:id', async (c) => {
  try {
    const id = c.req.param('id')
    const user = await findUser(id)
    if (!user) {
      return c.json({ error: 'User not found' }, 404)
    }
    return c.json(user)
  } catch (error) {
    console.error('Error:', error)
    return c.json({ error: 'Internal server error' }, 500)
  }
})
```

## CORS Configuration

Enable CORS for API endpoints:

```javascript
use('/api/*', async (c, next) => {
  c.res.headers.set('Access-Control-Allow-Origin', '*')
  c.res.headers.set('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE')
  c.res.headers.set('Access-Control-Allow-Headers', 'Content-Type')
  await next()
})
```

## Known Limitations (Beta)

- Server API routes may not work properly in 2.0.0-beta.2
- JSON responses might render as HTML
- Features are under active development
- Consider using external backend (Express, Hono, Cloudflare Workers) for production
- Test thoroughly before deploying

## Alternative: External Backend

If Nueserver doesn't meet your needs, use a separate backend:

```yaml
# site.yaml
server:
  proxy:
    /api: http://localhost:3000
```

This proxies all `/api/*` requests to your external server.
