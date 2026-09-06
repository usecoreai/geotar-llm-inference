# bge-m3 on H100

Эмбеддинги `BAAI/bge-m3` через vLLM OpenAI-compatible API: мультиязычная модель
с контекстом 8192 токена, отдаёт dense-векторы размерности 1024.

## Требования

- NVIDIA H100;
- Docker Compose;
- NVIDIA Container Toolkit

## Запуск

```bash
docker compose up -d
docker compose logs -f embeddings
```

По умолчанию используется GPU 0. Выбор другой карты:

```bash
GPU_DEVICE_ID=2 docker compose up -d
```

Образ `vllm/vllm-openai:v0.27.1` при первом `docker compose up` качается с Docker Hub
(порядка 20 ГБ). Веса `BAAI/bge-m3` после старта контейнера кладутся в volume
`hf_cache` и при следующих запусках не скачиваются.

## Проверка

```bash
curl http://localhost:8888/v1/embeddings \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "BAAI/bge-m3",
    "input": ["Привет!"],
    "encoding_format": "float",
    "truncate_prompt_tokens": 8192
  }'
```

Живость сервиса — `GET /health`, описание API — `GET /docs`.
API не имеет аутентификации — ограничьте доступ с помощью firewall или reverse proxy.
