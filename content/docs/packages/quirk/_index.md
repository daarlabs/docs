# Quirk
- Database abstraction layer
- Only supports **Postgres**, right now

## Connection
Use with <a href="/docs/packages/mirage/config/#database" target="_blank">Config</a>
```go
func Postgres() *quirk.DB {
    return quirk.MustConnect(
        quirk.WithPostgres(),
        quirk.WithHost("localhost"),
        quirk.WithPort(5432),
        quirk.WithDbname("postgres"),
        quirk.WithUser("postgres"),
        quirk.WithPassword("postgres"),
        quirk.WithSslDisable(),
        quirk.WithLog(true),
    )
}
```

## Data model
- By default, Quirk uses fields camelCase name transformed to snake_case
- Field name can be overriden with **db tag**
- Nullable fields need **sql.Null[T]** type
- For filling nullable fields, you can use **quirk.Nullable[T any](value T)**
```go
type Example struct {
	Id          int
	FooBar      string            `db:"foo_bar"`
	Nullable    sql.Null[string]
}

var data = Example{
    Nullable: quirk.Nullable("foo"),
}
```

{{< hint info >}}
**Advice**
- If you have some relationship to another table with some id in your data model and it can be **NULL**, you have to use **sql.Null[int64]**
```go
RelationshipId  sql.Null[int64]  `db:"relationship_id"`
```
{{< /hint >}}

## Query
- Query can be splitted in parts
- Each part can have own params
```go
func MustGetAll(db *quirk.DB) []model.Example {
	result := make([]model.Example, 0)
	db.Q(`SELECT *`).
		Q(`FROM examples`).
		Q(`WHERE category = @category`, quirk.Param{"category": "foo"}).
		Q(`LIMIT 20`).
		MustExec(&result)
	return result
}
```

## If
- Query part  will be used if condition is true
```go
func MustGetAll(db *quirk.DB, category string) []model.Example {
	result := make([]model.Example, 0)
	db.Q(`SELECT *`).
		Q(`FROM examples`).
		If(
          len(category) > 0, 
          `WHERE category = @category`, quirk.Param{"category": category}, 
        ).
		Q(`LIMIT 20`).
		MustExec(&result)
	return result
}
```

## Factory
- Utils, which can help you to create **INSERT** or **UPDATE** fields and placeholders strings

### Create Insert
```go
data := quirk.Param{
    "first_name": "Foo",
    "last_name":  "Bar",
    "age":        30,
}
fields, placeholders := CreateInsert(data)

var id int
db.Q(`INSERT INTO examples`).
	Q("(" + fields + ")").
	Q("VALUES (" + placeholders + ")", data).
	Q("RETURNING id").
	MustExec(&id)
```

### Create Update
```go
data := quirk.Param{
    "first_name": "Bar",
    "last_name":  "Foo",
    "age":        31,
}

db.Q(`UPDATE examples`).
	Q("SET " + CreateUpdate(data), data).
    Q("WHERE id = @id", quirk.Map{"id": 1}).
	MustExec(&id)
```

## In
- Arrays are automatically recognized, no need of any additional operation
- **IN** is automatically transformed to **ANY**
```go
Where(`id IN (@ids)`, quirk.Param{"ids": []int{1,2,3}})
```

## Transactions
```go
tx := db.MustBegin()
if someError {
	tx.MustRollback()
	return
}
tx.MustCommit()
```

## Postgres Type

### Jsonb
- Quirk custom type
- You can transform map to jsonb with **MapToJsonb**
```go
type Example struct {
	SomeMap Jsonb[string]
}

var data = Example{
	SomeMap: quirk.MapToJsonb(map[string]string{
	  "foo": "bar",	
    })
}
```

## Postgres Utils

### CreateTsVector
Create tsvector values from any primitive type
```go
Where(
	"INSERT INTO examples (vectors) VALUES (to_tsvector(@vectors))",
    quirk.Param{"vectors": quirk.CreateTsVector("foo", "bar")},
)
```

### CreateTsQuery
Create tsquery values from any primitive type
```go
Where(
	"vectors @@ to_tsquery(@tsquery)", 
	quirk.Param{"tsquery": quirk.CreateTsQuery("foo", "bar")},
)
```