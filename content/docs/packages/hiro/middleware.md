# Middleware
Middlewares are just another handlers, which runs before all defined handlers
```go
app.Use(
    func(c hiro.Ctx) error {
        return c.Continue()
    },
)
```