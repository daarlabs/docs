# View
- Used own view library <a href="/docs/packages/gox/" target="_blank">Gox</a>

## Usage with handler
```go
type Props struct {}

func ExamplePage() Node {
	return Div(Text("Example page"))
}

func ExampleHandler() hiro.Handler {
	return func(c hiro.Ctx) error {
	    return c.Response().Render(ExamplePage(c, Props{}))	
    } 
}
```

{{< hint info >}}
**Advice**
- Page should have props property
```go
func ExaplePage(c hiro.Ctx, props Props) Node {
	// ...
}
```
{{< /hint >}}