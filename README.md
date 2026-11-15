# Ozon Demand Service

## Структура репозитория


| Путь                                                         | Что здесь хранится                                                                                                                          |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`pyproject.toml`](pyproject.toml), `uv.lock`, `.python-version` | Общие зависимости и инструменты Python. Версия для разработки — 3.12.                                       |
| [`src/ozon_demand_service/`](src/ozon_demand_service/)           | Python-код приложения. Здесь будут FastAPI, загрузка данных, прогнозирование и оптимизация. |
| [`config/`](config/)                                             | Настройки запуска приложения и шаблон переменных окружения.                                             |
| [`frontend/`](frontend/)                                         | Веб-кабинет и конфигурация клиента.                                                                                           |
| [`data/`](data/)                                                 | Описание форматов и синтетические входные данные.                                                                |
| [`experiments/`](experiments/)                                   | Ноутбуки Jupyter для исследований и сравнения моделей.                                                              |
| [`infra/`](infra/)                                               | Конфигурация Airflow, MLflow и S3-совместимого хранилища.                                                                 |

## Запуск в контейнерах

Из корня репозитория с Docker и Docker Compose. Команды используют [`compose.yaml`](compose.yaml).

```bash
# Собрать образ приложения и проверить импорт установленного пакета.
docker compose run --build --rm python

# Собрать образ с зависимостями разработки и запустить Ruff.
docker compose run --build --rm checks
```

## Локальная настройка pre-commit

Установите [uv](https://docs.astral.sh/uv/getting-started/installation/) на рабочей машине и выполните из корня репозитория:

```bash
uv --version
uv sync --locked
uv run --locked pre-commit install
uv run --locked pre-commit run --all-files
```

При коммите запускаются проверки из [`.pre-commit-config.yaml`](.pre-commit-config.yaml).
