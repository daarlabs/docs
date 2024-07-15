# Tempest
- On-request dynamic styles utility-based framework
- CSS, JS, Fonts manager
- API inspired by <a href="https://tailwindcss.com/" target="_blank">TailwindCSS</a>
- Classes and their styles are cached

## Config
- Tempest uses global config and sync.Map to store classes, so you need to configure it before app config initialization

{{< hint info >}}
**Advice**
- Init Tempest Config in same file as an app config
{{< /hint >}}

```go
func init() {
	tempest.GlobalConfig = &tempest.Config{
		// App font family
		FontFamily: "Sora, sans-serif",
		// Aditional fonts
		Font: map[string]tempest.Font{
			"sora": {
				Value: "Sora, sans-serif",
				Url:   "https://fonts.googleapis.com/css2?family=Sora:wght@100..800&display=swap",
			},
		},
		// Additional styles, which will be bundled
		Styles: []string{},
		// Scripts to bundle
		Scripts: []string{},
		// Custom colors
		Color: map[string]tempest.Color{
			palette.Primary: palette.PrimaryPallete,
		},
		// Custom shadows
		Shadow: map[string][]tempest.Shadow{
			"focus": {
				{
					Value: "0 0 0 0.25rem",
					Hex: palette.PrimaryPallete[400], 
					Opacity: 15,
				},
			},
		},
	}
	tempest.Start()
}

```

## Start
Process and prepare all necessary things to provide frontend assets

## Assets providing
By default, Hiro has routes for dynamic Tempest content, so you don't need to worry about that

## Class
- **tempest.Class()** is factory, which create **gox.Node** class element or string
- Use methods chaining to apply all classes, you want to

{{< hint warning >}}
**Warning**
- Not all classes have been implemented yet
{{< /hint >}}

```go
Div(
	tempest.Class().P(1).M(1).TextSlate(900).TextWhite(tempest.Dark())
),
```
## Component Action
- If you use **tempest.Class()** in smart component action response, you have to add **.Name(c.Request().Action())**
- Actions are dynamic and Tempest doesn't use request context, so they are stored by action name
- Not cached classes are dynamically added to response
```go
tempest.Class().Name(c.Request().Action()).BgSlate(100).P(4)
```

## Modifier
### Placeholder
```go
tempest.Class().TextSlate(900).TextSlate(400, tempest.Placeholder())
```

### Dark
Classes with **Dark()** modifier will be used, if any parent has **dark** class
```go
tempest.Class().BgWhite().BgSlate(900, tempest.Dark())
```
{{< hint info >}}
**Advice**
- You can store dark mode in cookies and use dark in body class
```go
Body(
	Clsx{
      tempest.Class().Dark(): c.Cookie.Get("X-Dark-Mode") == "true",
    }
)
```
{{< /hint >}}

### Hover
```go
tempest.Class().BgSlate(400).BgBlue(400, tempest.Hover())
```

### Focus
```go
tempest.Class().Border(1).BorderSlate(300).BorderBlue(400, tempest.Focus())
```

### Checked
```go
tempest.Class().
    BgWhite().BgBlue(400, tempest.Checked())
    Border(1).BorderSlate(300).BorderBlue(400, tempest.Checked())
```

### Peer
```go
tempest.Class().
    BgWhite().BgBlue(400, tempest.Checked(tempest.Peer))
    Border(1).BorderSlate(300).BorderBlue(400, tempest.Checked(tempest.Peer))
```

### Group
```go
tempest.Class().BgWhite().BgBlue(400, tempest.Hover(tempest.Group))
```

### Opacity
```go
tempest.Class().BgSlate(900, tempest.Opacity(80))
```

### Media query
```go
tempest.Class().
    W(4).
    W(8, tempest.Xs()).
    W(12, tempest.Sm()).
    W(16, tempest.Md()).
    W(24, tempest.Lg()).
    W(32, tempest.Xl()).
    W(64, tempest.Xxl())
```