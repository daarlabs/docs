# Dyna
- Dynamic data manager
- Supports manual provided data, database data or data factory functions
- Used in dynamic components like: **autocomplete**, **datatable**
- Database query uses <a href="/docs/packages/esquel/" target="_blank">Esquel</a>

## New instance
```go
dyna.New()
```

## Query data model
```go
type Query struct {
	Table  string
    Alias  string
    Value  string
	Fields map[string]string
}
```
### Table
Database table name
### Alias
Datatable table alias
### Value
Datatable table field name, like **id**
### Fields
Datatable fields aliases, suitable for smart components
```go
Fields: map[string]string{
	"text":   "e.name",
	"value":  "e.id",
}
```


## Set DB
```go
m := dyna.New()

m.DB(db, query)
```

## Set Data
```go
m := dyna.New()

m.Data([]map[string]any{
    { "value": 1, "text": "Text 1" },
    { "value": 2, "text": "Text 2" },
})
```

## Parse params from hiro.Ctx
```go
var param dyna.Param
param = param.Parse(c)
```

## Use params on Esquel query
- Generates conditions, sorting, offseting, limiting
```go
var param dyna.Param
param = param.Parse(c)
q := db.Q(`...`)
param.Use(q)
q.MustExec()
```

## Get data
- Automatically choose data source (manual data / database), <br> if override func exists, prefers that

### Get All
```go
GetAll(param, &target)
```
### Get One
```go
GetOne("id", 1, &target)
```
### Get Many
```go
GetMany("id", []int{1,2,3}, &target)
```


## Override data get

### Get All Func
```go
GetAllFunc(func(param Param, t any) error {
	// ...
	return nil
})
```
### Get One Func
```go
GetOneFunc(func(name string, v any, t any) error {
	// ...
	return nil
})

```
### Get Many Func
```go
GetManyFunc(func(name string, v any, t any) error {
	// ...
	return nil
})

```


## Util

### Create Select
```go
 // third param is not required, because id is default value field
CreateSelect("examples", "name", "id") Query
```