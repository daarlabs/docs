# Devtool
Utility tool shows informations about current page / action
- Status code
- Path
- Route name
- Render time
- Custom debug logs
- Query parameters
- Path values
- Database queries
- Current user session

## Run devtool with CLI
```bash
hrx dev
```

## Use devtool
### Config
```bash
Dev: config.Dev{
  LiveReload: env.Development(),
  Tool:       env.Development(),
}
```