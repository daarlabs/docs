# Route
- Functions handle incoming request and outcoming response
- Route uses handler
```go
func ExampleRoute(app hiro.Hiro) {
    app.Route(
        "/example",
        example_handler.Example(),
        Method(http.MethodGet, http.MethodPost),
        Name("example"),
    )
}
```

## Config

### Method
- Route is created with all http methods by default
- You can override defaults and define custom http methods
```go
Method(http.MethodGet, http.MethodPost)
```
### Name
For routes matching and generating links
```go
Name("example")
```

### Layout
- Layouts are defined in app **main.go** file
- Override **main** layout
```go
Layout("custom_layout")
```