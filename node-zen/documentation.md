## Build

```bash
$ docker build -t ngrey5/node-zen:version
$ docker tag ngrey5/node-zen:version ngrey5/node-zen:latest
$ docker push --all-tags ngrey5/node-zen
```

### OR (SHORTHAND)

```bash
$ docker build -t ngrey5/node-zen:version -t ngrey4/node-zen:latest .
$ docker push --all-tags ngrey5/node-zen
```
