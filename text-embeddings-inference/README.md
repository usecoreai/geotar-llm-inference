# bge-m3 on H100

Эмбеддинги `BAAI/bge-m3` через Text Embeddings Inference: мультиязычная модель
с контекстом 8192 токена, отдаёт dense-векторы размерности 1024.

## Требования

- NVIDIA H100;
- Docker Compose;
- NVIDIA Container Toolkit
- .env файл рядом с компоузом, в котором HUGGINGFACE_API_KEY

Образ привязан к архитектуре карты: `hopper-1.9` собран под compute capability
90 и на других GPU не запустится.

## Запуск

```bash
docker compose up -d
docker compose logs -f embeddings
```

По умолчанию используется GPU 0. Выбор другой карты:

```bash
GPU_DEVICE_ID=2 docker compose up -d
```

Веса модели сохраняются в volume `hf_cache` и повторно не скачиваются. В первый
запуск скачается около 2.3 ГБ.

## Проверка (вместо <ключ> нужно вручную подставить api key)

```bash
curl http://localhost:8888/embed \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer <ключ>' \
  -d '{
    "inputs": ["Привет!"],
    "normalize": true,
    "truncate": true
  }'
```

Живость сервиса — `GET /health`, описание API — `GET /docs`.

## Доступ

У API есть аутентификация, но желательно ограничьте доступ с помощью firewall или reverse proxy.