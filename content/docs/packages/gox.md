# Gox

- Composable pure function view layer
- You can use any HTML element as Go func

{{< hint info >}}
**Advice**  
- Use import alias **import . "github.com/daarlabs/arcanum/gox"**
- Be careful with naming convention to prevent HTML nodes names collisions
{{< /hint >}}

## Sample code
```go
package app

import (
	. "github.com/daarlabs/arcanum/gox"
)

func Page() string {
	return Render(
		Html(
			Lang("en"),
			Head(
				Title(Text("Example app")),
			),
			Body(
				H1(Text("Example page")),
				P(Text("Example paragraph")),
			),
		),
	)
}

```

## Render
Render function transform Gox nodes to string
```go
Render(
	Div(Text("Gox"))
)
```

## Text
For the composition feature, we need to use text node, instead of primitive string
```go
H1(Text("Headline 1"))

P(Text("Paragraph"))
```

## Elements
```go
Div()

H1()

P()
```

## Attributes
```go
Button(
    Type("submit"),
    CustomData("track", "submit-button"),
)

A(
	Href("/"),
	Text("Go to homepage"),
)
```

## Shared
- Some nodes have shared name (label, style, title, ...)
- Modifiers **Element()**, **Attribute()** will change node type
```go
Label(Text("E-mail"))
Style(Element(), Raw("body{background:red;}"))
```

## Fragment
Wrapper, when you need group nodes to single node
```go
Fragment(
  Div(),
  Span(),
  H1(),
)
```

## Write
If you have **io.Writer**, like **http.ResponseWriter**, you can use **Write(w io.Writer, nodes ...Node)** function
```go
Write(w, Div(Text("this is the response")))
```

## Append
After node initialization, you can append nodes to the node
```go
Append(
  Div(),
  Text("Appended node"),
)
```

## Raw
Pure HTML can be used with **Raw()** node
```go
Raw(`<div>Raw element</div>`)
```

## Factory
If some element, attribute missing, or you have some custom features, you can create you own Gox nodes
```go
CustomElement := CreateElement("my-custom-element")
CustomAttribute := CreateAttribute[string]("my-custom-attribute")

CustomElement(
	CustomAttribute("some-value"),
)
```

## Plugin
You can also create custom nodes, as struct, with only one required method **Node() Node**
```go
type MyPlugin struct{
	SomeValue string
}

func (p MyPlugin) Node() Node {
	return Div(
		Text(p.SomeValue)
    )
}
```

## Clsx
Built-in plugin to conditional classes
```go
Div(
	Clsx{
      "class-1 class-2": true,
      "class-3": false,
    }
)
```


## Component
Gox offers compose power to create stateless UI components
```go
func MainButton(content string) Node {
    return Button(
        Type("button"),
        CustomData("test", "test-id"),
        Text(content),
    )
}
```