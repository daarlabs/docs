# Form
- Logic form layer

{{< hint info >}}
**Advice**
- Use import alias **import . "github.com/daarlabs/hirokit/form"**
{{< /hint >}}

## Data model
```go
type BookForm struct {
	Form
	Name        Field[string]
	Chapters    Field[int]
	Pages       Field[int]
	Read        Field[bool]
	Demo        Field[Multipart]
	Published   Field[time.Time]
}
```

## Factory
- **Name** and **Id** can be different
- Field types are named by HTML 
- Hiro form factory generates all necessary, but you can customize it with chaining methods
```go
func CreateBookForm(c hiro.Ctx) (BookForm, error) {
	f := c.Create().Form(
      Add("name").Id("name").Label("Name").With(Text(), Validate.Required()),
      Add("chapters").Id("chapters").Label("Chapters").With(Number[int](), Validate.Required()),
      Add("pages").Id("pages").Label("Pages").With(Number[int](),Validate.Required()),
      Add("read").Id("read").Label("Read").With(Checkbox()),
      Add("demo").Id("demo").Label("Demo").With(File()),
      Add("published").Id("published").Label("Published").With(Time()),
    )
	return Build[BookForm](f)
}
```

{{< hint info >}}
**Advice**
- You can create **Must** func
```go
func MustCreateBookForm(c hiro.Ctx) BookForm {
	f, err := CreateBook(c)
	if err != nil {
		panic(err)
	}
	return f
}
```
{{< /hint >}}

## Default value
```go
// Let's say, you have some select and
// you need number value and string text
// on detail page
Add("name").
	Id("name").
	Label("Name").
	Text("some text").
	With(
		Number[int](someValue), 
		Validate.Required(),
    )
```


[//]: # ()
[//]: # (## Field)

[//]: # ()
[//]: # (### Hidden)

[//]: # ()
[//]: # (### Text)

[//]: # ()
[//]: # (### Number)

[//]: # ()
[//]: # (### Checkbox)

[//]: # ()
[//]: # (### Email)

[//]: # ()
[//]: # (### Password)


[//]: # ()
[//]: # (### Radio)

[//]: # ()
[//]: # (### File)

[//]: # ()
[//]: # (### Date)

[//]: # ()
[//]: # (### DateTimeLocal)

[//]: # ()
[//]: # (### Month)

[//]: # ()
[//]: # (### Week)

[//]: # ()
[//]: # (### Range)

[//]: # ()
[//]: # (### Reset)

[//]: # ()
[//]: # (### Color)

[//]: # ()
[//]: # (### Button)

[//]: # ()
[//]: # (### Image)



{{< hint info >}}
**Advice**
- For simplify view layer, create field props factory
```go
type TextFieldProps struct {
	Id          string
	Name        string
	Label       string
	Value       string
	Placeholder string
	Messages    []string
	Autofocus   bool
	Disabled    bool
	Required    bool
}


func CreateProps(field form.Field[string]) TextFieldProps {
    return TextFieldProps{
        Id:        field.Id,
        Name:      field.Name,
        Label:     field.Label,
        Value:     field.Value,
        Messages:  field.Messages,
        Disabled:  field.Disabled,
        Autofocus: field.Autofocus,
        Required:  field.Required,
    }
}
```
{{< /hint >}}
