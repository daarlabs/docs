# Handler
Functions handle incoming request and outcoming response
```go
func ExampleHandler() Handler {
	return func(c hiro.Ctx) error {
	    return c.Response().Text("Hello World!")	
    } 
}
```

{{< hint info >}}
**Advice**
- Through handler factory, you can pass deps as args
```go
func ExampleHandler(exampleService example_service.ExampleService) Handler {
    return func(c hiro.Ctx) error {
        return c.Response().Text(exampleService.GetResult())
    } 
}
```
{{< /hint >}}


### Next step
[Route](/docs/packages/hiro/route/)