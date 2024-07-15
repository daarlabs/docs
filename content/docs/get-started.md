# Get started

Requirements:
- [Docker](https://www.docker.com/)
- [Go](https://go.dev/) 1.22+

Optional:
- [Tilt](https://tilt.dev/)

## Install CLI
```bash
go install -v github.com/daarlabs/hrx
```

## Create a new app
```bash
hrx new app_name && cd app_name
```


## Start the app
```bash
tilt up

# OR

docker compose up
```

Your app will be running on [localhost](http://localhost)

{{< columns >}}

## Next step
[Config](/docs/packages/hiro/config/)

{{< /columns >}}