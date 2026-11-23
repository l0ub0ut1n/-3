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
