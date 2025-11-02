# Getting Started with Nue.js

Install and run your first Nue.js app.

## Installation

```bash
# Install Bun runtime
curl -fsSL https://bun.sh/install | bash

# Install Nue.js globally
bun install --global nuekit
```

## First App (Counter)

Create `index.html`:

```html
<!doctype dhtml>
<button @click="count++">{ count }</button>
<script>count = 0</script>
```

**Run it:**
```bash
nue serve          # Dev server on port 4000
```

**Key syntax:**
- `<!doctype dhtml>` = reactive mode (client-side) - **REQUIRED for interactivity**
- `@click` = event handler
- `{ }` = data binding
- `<script>` = state initialization

**⚠️ Critical:** Use `<!doctype dhtml>` for interactive components. Regular `<!doctype html>` = static/server-side only. See common-mistakes.md for details.

## Common Commands

```bash
nue serve                    # Dev server with hot-reload
nue serve --port 8080        # Custom port
nue build                    # Production build
nue create my-app            # Scaffold from template
nue --help                   # Full command reference
```

## Templates

```bash
nue create my-app  # Choose: minimal, blog, spa, full
```

## For React Developers

| React | Nue.js |
|-------|--------|
| `useState(0)` | `count = 0` |
| `onClick={() => ...}` | `@click="..."` |
| Virtual DOM | Direct DOM |

No hooks, no JSX, no imports. Start simple, extract components when needed.

## Next Steps

- **Organizing projects** → See organizing-projects.md
- **Adding reactivity** → See adding-reactivity.md
- **Building SPAs** → See building-spas.md
- **Blog/content sites** → See content-collections.md
