# Config



## App
```go
App: config.App{
    Name:           "your-app-name",
    Plugin:         true // If it's plugin, not an app
    PublicUrlPath:  "/public/",
    PublicLocalDir: "./public/",
}    
```

### Plugin
Plugins like Farah UI, based on Hiro, must be registered, they need set the field to true

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
Database: map[string]*esquel.DB{
    hiro.Main:        MainDatabase(),
    "some-database":  SomeAnotherDatabase(),
},
```

## Dev
Devtool usage configuration
```go
Dev: config.Dev{
  LiveReload: env.Development(),
  Tool:       env.Development(),
}
```

## Form
- Limit incoming form data size in MBs
```go
Form: form.Config{
    Limit: 256,
}
```

## Filesystem
- Local
- S3-compatible - <a href="https://github.com/minio/minio-go" target="_blank">Minio client</a>
```go
// Local
Filesystem: filesystem.Config{
    Driver: filesystem.Local,
    Dir:    "./files"
}

// Cloud
cloudClient, _ := minio.New(
	"your-bucket-endpoint", 
	&minio.Options{
        Creds:  credentials.NewStaticV4(
            "your-bucket-access-key", 
            "your-bucket-secret-key", 
            "", // token, if necessary
        ),
        Secure: true,
    },
)
Filesystem: filesystem.Config{
    Driver: filesystem.Cloud,
	Cloud: cloudClient,
    Name: "my-bucket-name"
}
```

## Localization
- Uses cookies by default
- Can use path prefix
```go
Localization: config.Localization{
    Enabled: true,
    // If you want have lang code as path prefix
    Path:    true,
    Languages: []config.Language{
        { Main: true, Code: "cs" },
        { Code: "en" },
    },
    Translator: translator.New(
        translator.Config{
            Dir:      "./locales",
            FileType: translator.Yaml,
        },
    ),
    Form: form.Messages{
        Email:     "error.form.field.email",
        Required:  "error.form.field.required",
        MinText:   "error.form.field.min-text",
        MaxText:   "error.form.field.max-text",
        MinNumber: "error.form.field.min-number",
        MaxNumber: "error.form.field.max-number",
    },
}
```

## Parser
- Limit incoming body size (files) in MBs, when you are not using form builder
```go
Parser: config.Parser{
	Limit: 256,
}
```

## Router
```go
Router: config.Router{
    // If you want have in all paths prefix, 
	// lang code included, but has more weight
	Prefix: "custom-prefix",
	// Automatic panic recover
	// Handled by dynamic handler
    Recover: true,
}
```

## Security
Auth and firewall are two different packages, each manage their own things, but roles must be shared
```go
roles := []auth.Role{
    {Name: "super-admin", Super: true},
}

Security: config.Security{
    Auth: auth.Config{
        Roles: roles,
    },
    Csrf: csrf.New(),
    Firewall: []firewall.Firewall{
        {
            Enabled: true,
            Name: hiro.Main,
            Roles: roles,
            Groups: []string{"super-secret-page"},
            Redirect: "login", // If forbidden, route name redirect
        },
    },
}
```

### Firewall
- Comparing names and securables

### Roles
- One has to be main
- Super has access through all firewalls, despite firewall securables

### Groups
- Universally named paths names prefixes

### CSRF
- If **CSRF** has config instance, it's auto-injected, through factory to all forms, when you are not logged in


## Smtp
```go
Smtp: mailer.Config{
    Host:     "your-host",
    Port:     25,
    User:     "username",
    Password: "password",
    From:     "\"Your Name\" <info@your-email.com>",
}
```

{{< columns >}}

### Previous step
[Get started](/docs/get-started/)

<--->

### Next step
[Router](/docs/packages/hiro/router/)

{{< /columns >}}