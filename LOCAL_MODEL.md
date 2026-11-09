# Локальная Qwen3.5-9B для PyCharm

Модель: `mlx-community/Qwen3.5-9B-MLX-4bit`, скачана в `/Users/vladimir/.cache/local-models/Qwen3.5-9B-MLX-4bit`. Агент PyCharm называется **Qwen Coder Local**, модель в его настройках — **Qwen3.5 9B Local 4bit**. Сервер обслуживает текстовые запросы на `127.0.0.1:8000`.

## Запуск

В терминале проекта `ai-docs-assistant`:

```bash
HF_HUB_OFFLINE=1 uv run --locked python -m mlx_lm server \
  --model "$HOME/.cache/local-models/Qwen3.5-9B-MLX-4bit" \
  --host 127.0.0.1 --port 8000 --max-tokens 1024 \
  --temp 0.7 --top-p 0.8 --top-k 20 \
  --chat-template-args '{"enable_thinking":false}' \
  --prefill-step-size 128 --decode-concurrency 1 \
  --prompt-concurrency 1 --prompt-cache-size 1
```

Оставь терминал открытым. В PyCharm создай новый AI Chat и выбери агента **Qwen Coder Local**. После изменения настроек агента может потребоваться перезапуск PyCharm. Сервер завершится по `Ctrl+C`.

Флаг `enable_thinking:false` выбирает быстрые прямые ответы; это может ухудшать решение сложных задач. Модель запускается локально. Веса уже скачаны; `HF_HUB_OFFLINE=1` запрещает обращения Hugging Face Hub к сети. Лимит ответа агента — 1024 токена; большая история и инструменты увеличивают время обработки.

## Проверка

```bash
curl --fail-with-body http://127.0.0.1:8000/v1/models
```

9 ноября 2026: SHA-256 двух файлов весов совпали с метаданными Hugging Face. Прямой тест чата вернул `4` на вопрос «два плюс два»; отдельный API-запрос сформировал структурированный вызов `read_file`. Проверка Qwen Code в безопасном режиме также вернула `4`. Полная работа агента через интерфейс PyCharm и скорость на реальных проектах требуют отдельной проверки. Кеш прежней Qwen3-Coder-30B-A3B-Instruct-3bit удалён.

Конфигурация агента: `~/.jetbrains/acp.json`; конфигурация модели: `~/.qwen/settings.json`. Перед переходом созданы копии с суффиксом `.before-qwen35-20261109`.
