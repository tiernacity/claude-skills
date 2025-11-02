# Creating APIs with Nueserver

REST endpoints with Nueserver.

⚠️ **Beta:** Server features in 2.0.0-beta.2 may not work as documented. Test thoroughly, consider external backend (Express, Hono) if issues arise.

## Setup

```
my-app/
├── server/
│   └── index.js       # API routes
└── site.yaml
```

**site.yaml:**
```yaml
server:
  dir: server
  reload: true
```

## Creating Endpoints

Global routing functions (no imports):

```javascript
const users = [
  { id: '1', name: 'Alice' },
  { id: '2', name: 'Bob' }
]

// GET all
get('/api/users', async (c) => {
  return c.json(users)
})

// GET single
get('/api/users/:id', async (c) => {
  const id = c.req.param('id')
  const user = users.find(u => u.id === id)
  if (!user) return c.json({ error: 'Not found' }, 404)
  return c.json(user)
})

// POST create
post('/api/users', async (c) => {
  const data = await c.req.json()
  const user = { id: String(users.length + 1), ...data }
  users.push(user)
  return c.json(user, 201)
})

// PUT update
put('/api/users/:id', async (c) => {
  const id = c.req.param('id')
  const user = users.find(u => u.id === id)
  if (!user) return c.json({ error: 'Not found' }, 404)
  Object.assign(user, await c.req.json())
  return c.json(user)
})

// DELETE
del('/api/users/:id', async (c) => {
  const index = users.findIndex(u => u.id === c.req.param('id'))
  if (index === -1) return c.json({ error: 'Not found' }, 404)
  users.splice(index, 1)
  return c.json({ success: true })
})
```

## Global Functions

- `get(path, handler)`
- `post(path, handler)`
- `put(path, handler)`
- `del(path, handler)`
- `use(path, middleware)`

## Context Object (c)

**Response:**
- `c.json(data, status?)`
- `c.text(text, status?)`
- `c.html(html, status?)`

**Request:**
- `c.req.param(name)` - URL param
- `c.req.json()` - Parse body
- `c.req.header(name)` - Get header
- `c.req.query(name)` - Query param

**Environment:**
- `c.env` - Cloudflare Workers pattern

## Middleware

```javascript
// CORS for API endpoints
use('/api/*', async (c, next) => {
  c.res.headers.set('Access-Control-Allow-Origin', '*')
  c.res.headers.set('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE')
  c.res.headers.set('Access-Control-Allow-Headers', 'Content-Type')
  await next()
})

// Authentication
use('/api/admin/*', async (c, next) => {
  const token = c.req.header('Authorization')
  if (!isValidToken(token)) {
    return c.json({ error: 'Unauthorized' }, 401)
  }
  await next()
})
```

## Error Handling

```javascript
try {
  const user = await findUser(id)
  if (!user) return c.json({ error: 'Not found' }, 404)
  return c.json(user)
} catch (error) {
  return c.json({ error: 'Server error' }, 500)
}
```

## Alternative: External Backend

```yaml
# site.yaml
server:
  proxy:
    /api: http://localhost:3000
```

Proxies all `/api/*` to external server.

## When You Need More

**Full Server API** → reference-guide.md
**Client-side fetching** → building-spas.md
