# Test: REST API Endpoints (Beta)

**Version introduced**: v3.0
**Last verified**: v4.0
**Status**: ⚠️ Passing (with documented beta limitations)

## Scenario

User wants to create REST API endpoints with Nueserver.

## User Prompt

```
I want to create a REST API in Nue.js.

Requirements:
- GET /api/users (list all)
- GET /api/users/:id (single user)
- POST /api/users (create)
- DELETE /api/users/:id (delete)
- Return JSON responses
```

## Success Criteria

1. ✅ Agent references creating-apis.md
2. ✅ Creates server/index.js with route handlers
3. ✅ Uses global functions (get, post, del) correctly
4. ✅ Uses context object properly (c.json, c.req.param)
5. ✅ Configures site.yaml with server settings
6. ✅ **Provides beta warning** about limitations
7. ✅ Suggests alternatives if features don't work
8. ⚠️ Routes may not work (beta issue) but patterns are correct

## Expected Output

**site.yaml**:
```yaml
server:
  dir: server
  reload: true
```

**server/index.js**:
```javascript
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

## RED Phase Result (v2.0 - Without Server Coverage)

- 0% coverage for server-side APIs
- No guidance on Nueserver patterns
- Confusion about routing approach
- No context object documentation
- Unclear how to structure server code

## GREEN Phase Result (v3.0 - With creating-apis.md)

- Immediate reference to creating-apis.md
- Correct pattern usage (get, post, del, c.json, c.req.param)
- Proper file structure (server/index.js)
- **Agent provided beta warnings** (as documented)
- Suggested external backend alternatives when routes didn't work

## Known Issues

⚠️ **Beta Limitations (2.0.0-beta.2)**:
- Server routes may not work properly
- JSON responses might render as HTML
- Features under active development

**This is expected and documented** - skill correctly warns about beta status.

## Regression Testing

Run this test when:
- Updating creating-apis.md
- Nue.js releases new server features
- Context object API changes
- Beta status changes (when stable)

## Notes

- Tests creating-apis.md coverage
- **Success includes providing beta warnings** (not just working code)
- Patterns are correct even if implementation is beta
- Good test of "document what should work" vs "what does work"
- When Nue.js server goes stable, update beta warnings
