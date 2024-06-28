# Config



## App

### Plugin
Plugins like Farah UI, based on Mirage, must be registered, they need set the field to true
```go
Plugin: true
```

## Cache
- Cache by default uses memory package, but recommended way is to use **Redis** (or Redis-compatible database)
- <a href="https://github.com/redis/go-redis" target="_blank">Redis client</a>
```go
Cache: config.Cache{
    // Custom memory dir, which persist cache state
	memory: memory.New("./custom-backup-dir"),
	redis:  redis.NewClient(
        &redis.Options{
            Addr:     "starter-redis:6379",
            DB:       1,
        },
    )
}
```

## Database
- Supports multi-database system
- Main database is default
- Provided naming convention constant
```go
Database: map[string]*quirk.DB{
    mirage.Main:      MainDatabase(),
    "some-database":  SomeAnotherDatabase(),
},
```

## Form
- Limit incoming data size in MBs
```go
Form: form.Config{
    Limit: 256
}
```

## Filesystem

## Localization

## Parser

## Router

## Security

## Smtp


{{< columns >}}

### Previous step
[Get started](/docs/get-started/)

<--->

### Next step
[Router](/docs/packages/mirage/router/)

{{< /columns >}}