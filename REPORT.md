# Отчёт по проверке

## Уровень 4: один контейнер

```
$ docker build -t hw3-app .
$ docker run -d --name hw3 -p 8080:8000 -e GREETING="Hello from docker" hw3-app

$ curl http://127.0.0.1:8080/
{"greeting": "Hello from docker", "count": 0, "hostname": "96a57d352628", "storage": "sqlite"}
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from docker", "count": 1, "hostname": "96a57d352628", "storage": "sqlite"}
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from docker", "count": 2, "hostname": "96a57d352628", "storage": "sqlite"}
$ curl http://127.0.0.1:8080/
{"greeting": "Hello from docker", "count": 2, "hostname": "96a57d352628", "storage": "sqlite"}

$ docker logs hw3
2026-11-23 09:54:18,830 listening port=8000 storage=sqlite
2026-11-23 09:55:23,301 http 192.168.65.1 "GET / HTTP/1.1" 200 -
2026-11-23 09:55:23,317 http 192.168.65.1 "POST /increment HTTP/1.1" 200 -
2026-11-23 09:55:23,330 http 192.168.65.1 "POST /increment HTTP/1.1" 200 -
2026-11-23 09:55:23,338 http 192.168.65.1 "GET / HTTP/1.1" 200 -
```

## Уровень 6: Compose и том

```
$ cp .env.example .env
$ docker compose up -d --build
 ✔ Volume hw3_counter-data Created
 ✔ Container hw3-app-1     Started
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from compose", "count": 1, "hostname": "21043bf9486f", "storage": "sqlite"}
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from compose", "count": 2, "hostname": "21043bf9486f", "storage": "sqlite"}
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from compose", "count": 3, "hostname": "21043bf9486f", "storage": "sqlite"}

$ docker compose down
 ✔ Container hw3-app-1 Removed
$ docker compose up -d
 ✔ Container hw3-app-1 Started
$ curl http://127.0.0.1:8080/
{"greeting": "Hello from compose", "count": 3, "hostname": "93a3ae699536", "storage": "sqlite"}
```


## Уровень 8: приложение + Redis

```
$ docker compose up -d --build
$ docker compose ps
NAME           IMAGE              SERVICE   STATUS                    PORTS
hw3-app-1      hw3-app            app       Up (health: starting)     0.0.0.0:8080->8000/tcp
hw3-redis-1    redis:7.4-alpine   redis     Up 5 seconds (healthy)    6379/tcp
$ curl -X POST http://127.0.0.1:8080/increment   # x3
{"greeting": "Hello from compose", "count": 1, "hostname": "1024f800c85c", "storage": "redis"}
{"greeting": "Hello from compose", "count": 2, "hostname": "1024f800c85c", "storage": "redis"}
{"greeting": "Hello from compose", "count": 3, "hostname": "1024f800c85c", "storage": "redis"}
```


### down и up без -v

```
$ docker compose down
$ docker compose up -d
$ curl http://127.0.0.1:8080/
{"greeting": "Hello from compose", "count": 3, "hostname": "ecb227d3cf61", "storage": "redis"}
```

### Остановка Redis

```
$ docker compose stop redis
healthz: 200
readyz: 503
increment: 503
```

### Запуск Redis без перезапуска app

```
$ docker compose start redis
readyz: 200
$ curl -X POST http://127.0.0.1:8080/increment
{"greeting": "Hello from compose", "count": 4, "hostname": "ecb227d3cf61", "storage": "redis"}
$ docker compose ps
hw3-app-1     app     Up About a minute (healthy)   0.0.0.0:8080->8000/tcp
hw3-redis-1   redis   Up 5 seconds (healthy)        6379/tcp
```


