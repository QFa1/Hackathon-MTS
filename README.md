# CompactFactExtraction · B26 DSAI

Компактная LLM для извлечения фактов из разговоров технической поддержки: проблемы, действия, состояния и договорённости с привязкой к исходным репликам. Проект B26 DSAI по кейсу МТС MWS.

## Технологии

- Python, PyTorch
- Qwen3-0.6B, Hugging Face Transformers, PEFT / LoRA
- bitsandbytes, Accelerate, Safetensors
- FastAPI, Pydantic, Uvicorn
- HTML / CSS / JavaScript
- Docker, Google Colab, pytest, Matplotlib

## Структура

- `app/` — API, интерфейс и интеграция Qwen.
- `training_and_evaluation/` — код обучения, оценки и экспериментов.
- `reports/` — метрики, ресурсные измерения и ограничения.
- `presentation/` — презентация команды.
- `SOURCE_MANIFEST.json` — происхождение и контрольные суммы исходников.

## Запуск

Готовое приложение, веса, Docker-образ и видео передаются в [Releases](https://github.com/QFa1/Hackathon-MTS/releases). После скачивания полного комплекта распакуйте его, запустите Docker Desktop и `START_WINDOWS.cmd`, затем откройте **http://127.0.0.1:8000**. Для остановки — `STOP_WINDOWS.cmd`. Требуются Linux-контейнеры; на Windows — WSL2. CPU-обработка разговора может занимать несколько минут.

Для сборки из исходников нужны `models/base` и `models/export` из полного комплекта:

```sh
docker compose build
docker compose run --rm --no-deps extractor --check-only
docker compose up -d --no-build --wait --wait-timeout 300
```

Корневой Compose использует `app/Dockerfile.release`; обычный `app/Dockerfile` — историческая mock-сборка. `/extract` возвращает факты и метаданные, `/health` — готовность, `/model/info` — сведения о модели.

## Результаты

В архиве есть полный прогон 383 официальных разговоров и сравнение FP16 / INT8 / INT4 на четырёх одинаковых диагностических звонках, а также CPU-замер. Подробности и непройденные temporal/state-пороги — в `reports/`. Презентация команды и архивные отчёты описывают разные варианты обработки; их метрики разделены, не объявлены взаимозаменяемыми.

Веса, исходные датасеты организаторов и локальные секреты в Git не включаются. Для повторения обучения потребуются соответствующие подготовленные данные. Нового обучения при публикации не проводилось.

## Команда

Жигалов Андрей, Садыков Эдуард, Кулаков Александр. Исходная командная разработка: [innoglobalhack-compact-fact-extraction](https://github.com/fullfels/innoglobalhack-compact-fact-extraction).
