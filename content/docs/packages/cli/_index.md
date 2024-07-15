# CLI

## Requirements
- [Go](https://go.dev/) 1.22+

## Get started
```bash
go install -v github.com/daarlabs/hrx
```


## Cmd
- Run `hrx help` to see all possibilities
- Generating or migrating is only possible in project root directory (**go.work**)

## Create new app
```bash
hrx new app_name

# Short form
hrx n app_name
```

## Run devtool
```bash
hrx dev

# Short form
hrx d
```

## Generate

### Generate route
Generates route, handler and page with props
```bash
hrx generate route --name route_name --app app_name

# Short form
hrx g r -n route_name -a app_name
```

### Generate handler
```bash
hrx generate handler --name handler_name --app app_name

# Short form
hrx g h -n handler_name -a app_name
```

### Generate page
Generates page with props
```bash
hrx generate page --name page_name --app app_name

# Short form
hrx g p -n page_name -a app_name
```

### Generate form
```bash
hrx generate form --name form_name --app app_name

# Short form
hrx g f -n form_name -a app_name
```

### Generate migration
- Name is not required
```bash
hrx generate migration --name migration_name

# Short form
hrx g m -n migration_name
```

## Migrator

### Init migrator
```bash
hrx migrate init

# Short form
hrx m i
```

### Migrate up
```bash
hrx migrate up

# Short form
hrx m u
```

### Migrate down
```bash
hrx migrate down

# Short form
hrx m d
```