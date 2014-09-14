Docker + Serf + HAProxy
====

## Setup

### Build Haproxy-serf image

```bash
$ cd haproxy
$ docker build -t rds13/debian-serf-haproxy .
```

### Build Nginx-serf image

```bash
$ cd web
$ echo '<h1>web1</h1>' > index.html
$ docker build -t rds13/debian-serf-web1 .
$ echo '<h1>web2</h1>' > index.html
$ docker build -t rds13/debian-serf-web2 .
```

## Run

Run haproxy container

```bash
$ docker run -d -p 8080:80 -p 7946 --name proxy rds13/debian-serf-haproxy
```

Run web container

```bash
$ docker run -d --link proxy:serf rds13/debian-serf-web1
$ docker run -d --link proxy:serf rds13/debian-serf-web2
```

Access it

```bash
$ curl http://0.0.0.0:8080
```
or via boot2docker
```bash
$ curl http://$(boot2docker ip 2>/dev/null):8080/
```

Access haproxy stats page
```bash
$ curl http://0.0.0.0:8080/hastats
```
or via boot2docker
```bash
$ curl http://$(boot2docker ip 2>/dev/null):8080/hastats
```


Debug from host

```bash
$ serf agent -bind 127.0.0.1 -join $(docker port proxy 7946)
$ serf members
```

From OSX, serf can join proxy container, but can not send gossip to another web containers Because they send docker network IP address. This is just membership debugging.


## Reference

- [AUTO-LOADBALANCING DOCKER WITH FIG, HAPROXY AND SERF](http://www.centurylinklabs.com/auto-loadbalancing-with-fig-haproxy-and-serf/)
- Based on work from [Taichi Nakashima] (see http://github.com/tcnksm/docker-serf-haproxy).

