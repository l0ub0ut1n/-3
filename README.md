# HW3: Docker

Брусенцев Роман, БПМИ2613

Код приложения в `app/` остался без изменений.
## Файлы
- `Dockerfile`- образ приложения
- `compose.yaml` - app + redis
- `.env.example` -настройки
- `REPORT.md` - отчёт с результатами

## Настройки

В `.env` (образец в `.env.example`):

- `GREETING` -приветствие в ответе приложения
- `HOST_PORT` - порт на хосте, по умолчанию 8080

## Запуск

```sh
cp .env.example .env
docker compose up -d --build
curl http://127.0.0.1:8080/
curl -X POST http://127.0.0.1:8080/increment
docker compose down
```

