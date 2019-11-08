# tyk-integration

Simple integration golang custom plugin to Tyk API Gateway

## Getting started

This project requires Go to be installed. On OS X with Homebrew you can just run `brew install go`.

You can edit API Gateway configuration on `tyk.standalone.conf`.

You can add or modify (I suggest to add new file for each API) API specification file under folder `apps` (e.g. app2.json, app3.json and so on).

This example add custom header on API response using custom plugin written in Go.

### Instalation

For setting up the gateway, you should run Tyk gateway on your local (using docker). You can pull the docker image with this `docker pull tykio/tyk-gateway` and create network for Tyk with this `docker network create tyk`.
You can run the gateway with this:

```
docker run -d \
  --name tyk_gateway \
  --network tyk \
  -p 8080:8080 \
  -v $(pwd)/tyk.standalone.conf:/opt/tyk-gateway/tyk.conf \
  -v $(pwd)/apps:/opt/tyk-gateway/apps \
  -v $(pwd)/plugins:/opt/tyk-gateway/plugins \
  tykio/tyk-gateway:latest
```

or you can refer to this link: `https://hub.docker.com/r/tykio/tyk-gateway/`

After that, you need to compile your custom plugins with Tyk plugin compiler. You can using this:

```
docker run -v `pwd`:/go/src/plugin-build tykio/tyk-plugin-compiler addCustomHeader.so
```

after you compile the plugins, just restart your gateway.

You can try on your Postman, visit this link `http://localhost:8080/httpbin/` with method GET. You can get the result with an extra header `"Foo": "Bar"` like this:

```
{
  "args": {},
  "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Cache-Control": "no-cache",
    "Foo": "Bar",
    "Host": "httpbin.org",
    "Postman-Token": "72ad71d7-16f9-46d9-aa65-ac240d837559",
    "User-Agent": "PostmanRuntime/7.19.0"
  },
  "origin": "172.20.0.1, 103.86.156.227, 172.20.0.1",
  "url": "https://httpbin.org/get"
}
```
