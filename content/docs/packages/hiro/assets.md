# Assets

## Static (public) files
- For public (static) files, use Hiro Static handler, which use two arguments (**url path**, **dir**)

```go
app.Static("/public/", "./public/")
```

## Styles
- Handled by <a href="/docs/packages/tempest/" target="_blank">Tempest</a>

### Built-in
- "https://cdnjs.cloudflare.com/ajax/libs/modern-normalize/2.0.0/modern-normalize.min.css"

## Scripts
- Handled by <a href="/docs/packages/tempest/" target="_blank">Tempest</a>

### Built-in
- "https://cdn.jsdelivr.net/npm/alpinejs@3.x.x/dist/cdn.min.js"
- "https://unpkg.com/htmx.org@1.9.12"