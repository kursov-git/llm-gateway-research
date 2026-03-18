# Dry Run Report

Срез: 2026-03-18

## Контекст запуска

Dry run выполнялся в этой среде без Docker.

Доступная среда:

- `Node.js v22.22.1`
- `Python 3.12.3`
- `npm 10.9.4`
- Docker отсутствует

Это важно: сравнение ниже отражает именно **native/local startup profile**, а не Docker-first experience.

## Короткий вывод

Лучший dry-run результат показал `LiteLLM`.

`OmniRoute` оказался вторым: он успешно поднялся и выглядит сильным по operational surface, но путь запуска из исходников тяжелее и менее прямой.

`uni-api` заработал, но только после ручного обхода нескольких проблем, поэтому на пустой машине его onboarding в dry run заметно хуже.

## Результаты по кандидатам

### OmniRoute

Что делал:

- `npm ci` в `/tmp/omniroute`
- попытка запуска через `npx omniroute --no-open --port 20128`
- затем запуск через source-mode:
  - `npm run dev`
  - `DATA_DIR=/tmp/omniroute-data`
  - `INITIAL_PASSWORD=dryrun-password`
  - `JWT_SECRET`, `API_KEY_SECRET`, `STORAGE_ENCRYPTION_KEY`
  - `PORT=20128`
  - `DASHBOARD_PORT=20129`

Что получилось:

- `npx omniroute` из исходного клона не стартовал: ожидался собранный `app/server.js`.
- `npm run dev` поднялся успешно.
- Next.js dashboard стартовал на `20129`.
- API bridge слушал `127.0.0.1:20128`.
- SQLite и миграции поднялись автоматически.
- login flow работал.
- protected endpoints после логина отвечали корректно.
- `/api/monitoring/health` вернул `healthy`.

Полезные наблюдения:

- Установка зависимостей прошла успешно, но surface area большой.
- `localhost` и `127.0.0.1` в auth-flow вели себя по-разному из-за cookie-domain/IPv6, поэтому для dry run лучше использовать `127.0.0.1`.
- Без ручного подключения провайдеров `/v1/models` всё равно отвечал, но это ещё не означает готовность реального routing.

Итог:

- Сильный operational profile.
- Хороший dry-run старт после правильного выбора режима запуска.
- Более тяжелый и менее прямой onboarding, чем у `LiteLLM`.

### LiteLLM

Что делал:

- создал venv `/tmp/litellm-venv`
- `pip install '.[proxy]'`
- запуск:
  - `OPENAI_API_KEY=dryrun-dummy-key`
  - `litellm --model openai/gpt-4o-mini --host 127.0.0.1 --port 4000`

Что получилось:

- proxy поднялся без DB и без master key.
- `/health/liveliness` ответил `"I'm alive!"`
- `/health/readiness` вернул `healthy`, `db: Not connected`
- `/v1/models` отдал модель `openai/gpt-4o-mini`
- первый вызов `/v1/chat/completions` вернул прозрачную upstream `401` ошибку с понятным текстом

Полезные наблюдения:

- Для первого dry run DB не нужна.
- Для smoke-теста достаточно health/readiness.
- Ошибки upstream пробрасываются понятно и быстро.
- Самый предсказуемый startup path из трёх кандидатов.

Итог:

- Лучший time-to-first-working-proxy.
- Самый чистый dry-run onboarding.
- Сразу видно, как этот сервис будет жить как основной gateway слой.

### uni-api

Что делал:

- создал минимальный `api.yaml`
- создал venv `/tmp/uni-api-venv`
- попытка `pip install -e .`
- затем ручная установка зависимостей из `pyproject.toml`
- попытка запуска через `uvicorn main:app`
- затем `git submodule update --init --recursive`
- повторный запуск:
  - `DISABLE_DATABASE=true`
  - `PORT=8000`
  - `uvicorn main:app --host 127.0.0.1 --port 8000`

Что получилось:

- `pip install -e .` упал на setuptools package discovery.
- первый runtime упал с `ModuleNotFoundError: core.log_config`
- причина: git submodule `core` не был инициализирован
- после `submodule update --init --recursive` сервис стартовал успешно
- `/v1/models` с bearer token работал
- первый `/v1/chat/completions` вернул понятную upstream `401` ошибку

Полезные наблюдения:

- Для рабочего source dry run нужен не просто clone, а clone + submodules.
- install path менее “plug-and-play”, чем выглядит по README.
- `/docs` и `/` в этом режиме возвращали `403`, что делает readiness менее очевидным.
- Простая декларативная модель конфигурации через `api.yaml` остаётся сильной стороной.

Итог:

- Работает, но onboarding хрупкий.
- Хороший file-first подход.
- Из трёх кандидатов дал самый шероховатый first-run.

## Scorecard

Оценка по 5-балльной шкале для этого dry run:

| Критерий | Вес | LiteLLM | OmniRoute | uni-api |
|---|---:|---:|---:|---:|
| Time-to-first-request | 15% | 5 | 3 | 2 |
| Provider/config startup clarity | 20% | 5 | 4 | 3 |
| Fallback/routing surface | 20% | 4 | 5 | 4 |
| Operability / logs / health | 20% | 5 | 5 | 2 |
| Reproducible config | 15% | 4 | 4 | 5 |
| Security / sane defaults in dry run | 10% | 4 | 3 | 3 |

## Предварительный рейтинг

1. `LiteLLM`
2. `OmniRoute`
3. `uni-api`

## Практический вывод

Если выбирать одного кандидата для следующего шага пилота без реальных провайдерских ключей, выбирать `LiteLLM`.

Если нужен developer-first self-hosted control plane и есть готовность принять более тяжелый startup profile, держать `OmniRoute` как сильного второго кандидата.

Если критична именно file-first конфигурация и команда готова терпеть более шероховатый bootstrap, `uni-api` остаётся живой альтернативой, но не лидером по first-run experience.

## Что проверить следующим этапом

Следующий полезный уровень проверки уже требует реальных провайдерских ключей:

- 3 провайдера на одном gateway
- fallback по ошибке и по `429`
- логирование реальных upstream failures
- поведение при смене конфигурации
- удобство сопровождения после первого запуска
