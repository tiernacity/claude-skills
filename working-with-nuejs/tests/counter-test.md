# Test: Counter App (React Developer Transitioning)

**Version introduced**: v1.0
**Last verified**: v4.0
**Status**: ✅ Passing

## Scenario

A React developer wants to create a simple counter app in Nue.js. They're familiar with `useState`, `onClick`, JSX, but have never used Nue.js.

## User Prompt

```
I'm a React developer and want to try Nue.js. Can you help me create a simple counter app?

Requirements:
- Increment and decrement buttons
- Display current count
- Start at 0
```

## Success Criteria

1. ✅ Agent uses working-with-nuejs skill immediately
2. ✅ Creates single HTML file with `<!doctype dhtml>`
3. ✅ Uses `@click` syntax (not `:onclick` or `onClick`)
4. ✅ Uses `{ count }` binding syntax
5. ✅ Initializes state in `<script>` block as `count = 0`
6. ✅ No external documentation lookup needed
7. ✅ Working on first try (no trial-and-error)
8. ✅ References React transition table in explanation

## Expected Output

**File**: `index.html`

```html
<!doctype dhtml>
<html>
<head>
  <title>Counter</title>
</head>
<body>
  <div>
    <button @click="count--">-</button>
    <b>{ count }</b>
    <button @click="count++">+</button>

    <script>
      count = 0
    </script>
  </div>
</body>
</html>
```

## RED Phase Result (v1.0 - Without Skill)

- Agent struggled with syntax (tried `:onclick` first)
- Required 20-30 minutes of documentation research
- Multiple attempts to get reactivity working
- Confusion between Nue 1.0 (.nue files) and 2.0 (HTML)

## GREEN Phase Result (v1.0 - With Skill)

- Immediate reference to working-with-nuejs skill
- Correct syntax on first try
- Mentioned React transition patterns
- Zero external research
- Working in < 2 minutes

## Regression Testing

Run this test when:
- Updating getting started section
- Changing component syntax examples
- Modifying React transition guidance
- Major version changes to Nue.js

## Notes

- This is the foundational test (validates core reactivity)
- If this fails, something is fundamentally wrong
- Tests both syntax AND concept mapping from React
