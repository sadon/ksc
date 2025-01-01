# Docker containers

Docker image consist of LAYERS

Original
```commandline
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go
CMD ["./app"]
```

Multistage build:
https://docs.docker.com/build/building/multi-stage/
````commandline
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM apline
COP --from=0 /app .
CMD ["./app"]
````
---

1. use specific versions for guarantee to run it at the any time 
FROM ubuntu:22.04
FROM apline:3.12.1
golang-go:1.1.1

2. Don't run as root
```commandline
#RUN addgroup -S appgroup
USER appuser #use in cert
```

3. Run RO filesystem
```commandline
RUN  chmod a-w /etc
```

4. Remove shell exec
```commandline
RUN rm -rf /bin/*
```

