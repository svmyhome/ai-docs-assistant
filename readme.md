# Запуск локальной модели

В терминале проекта запусти сервер Qwen3.5-9B-MLX-4bit:

```bash
HF_HUB_OFFLINE=1 uv run --locked python -m mlx_lm server \
  --model "$HOME/.cache/local-models/Qwen3.5-9B-MLX-4bit" \
  --host 127.0.0.1 --port 8000 --max-tokens 1024 \
  --temp 0.7 --top-p 0.8 --top-k 20 \
  --chat-template-args '{"enable_thinking":false}' \
  --prefill-step-size 128 --decode-concurrency 1 \
  --prompt-concurrency 1 --prompt-cache-size 1
```

Оставь терминал открытым. В PyCharm создай новый AI Chat и выбери агента **Qwen Coder Local**. Чтобы остановить сервер, нажми `Ctrl+C` в его терминале.

Проверить, что сервер доступен:

```bash
curl --fail-with-body http://127.0.0.1:8000/v1/models
```

Модель запускается через MLX и не отображается в `ollama list`. Подробнее о настройках и проверке — в [LOCAL_MODEL.md](LOCAL_MODEL.md).
