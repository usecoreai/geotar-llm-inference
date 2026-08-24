# Qwen3.8-27B-FP8 on H100

Text-only inference `Qwen/Qwen3.8-27B-FP8` с reasoning и tool calling через
OpenAI-совместимый API vLLM.

## Требования

- NVIDIA H100;
- Docker Compose;
- NVIDIA Container Toolkit.

## Запуск

```bash
docker compose up -d
docker compose logs -f vllm
```

По умолчанию используется GPU 0. Выбор другой карты:

```bash
GPU_DEVICE_ID=2 docker compose up -d
```

Веса и compile cache сохраняются в `./cache` и повторно не скачиваются.

## Проверка

```bash
curl http://localhost:8000/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "Qwen/Qwen3.8-27B-FP8",
    "messages": [{"role": "user", "content": "Привет!"}],
    "max_tokens": 256
  }'
```

API не имеет аутентификации — ограничьте доступ с помощью firewall или
reverse proxy.
