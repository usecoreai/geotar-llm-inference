# Qwen3.8-27B-FP8 + BGE-M3 on H100

Text-only inference `Qwen/Qwen3.8-27B-FP8` с reasoning и tool calling и
эмбеддинги `BAAI/bge-m3` через OpenAI-совместимый API vLLM. Оба сервиса
поднимаются одним Compose и шарят кэш весов в `./cache`.

## Требования

- NVIDIA H100;
- Docker Compose;
- NVIDIA Container Toolkit.

## Запуск

Индексы GPU задаются в `.env`:

```
LLM_GPU_DEVICE_ID=0
EMBEDDINGS_GPU_DEVICE_ID=1
```

Запускать нужно на разных GPU девайсах, укажите необходимые в енве, далее:

```bash
docker compose up -d
docker compose logs -f
```

Веса и compile cache сохраняются в `./cache` и повторно не скачиваются.

## Проверка

LLM (`http://localhost:8000`):

```bash
curl http://localhost:8000/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "Qwen/Qwen3.8-27B-FP8",
    "messages": [{"role": "user", "content": "Привет!"}],
    "max_tokens": 256
  }'
```

Embeddings (`http://localhost:8888`):

```bash
curl http://localhost:8888/v1/embeddings \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "BAAI/bge-m3",
    "input": "Привет!"
  }'
```

API не имеет аутентификации — ограничьте доступ с помощью firewall или
reverse proxy.