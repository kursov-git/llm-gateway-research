# Comparison Matrix

Срез: 2026-03-20

## Матрица

| Проект | Класс | Установка | Настройка | Переключение / routing | Сильные стороны | Слабые стороны | Когда выбирать |
|---|---|---|---|---|---|---|---|
| `CLIProxyAPI` | CLI/OAuth-first proxy gateway | Source как Go service, Docker, `docker-compose`, companion tools | `config.yaml`, auth dir, management API, optional external dashboards | Multi-account `round-robin`, `fill-first`, quota switching, model mapping, upstream provider routing | Лучший direct fit для `Codex` / `Claude Code` / `Gemini` / `Qwen` / `iFlow`, сильная traction и зрелость | Control plane менее цельный, чем у OmniRoute; многое завязано на YAML, docs и companion-проекты | Когда нужен инструмент именно для AI coding и экстремального вайбкодинга |
| `OmniRoute` | Self-hosted AI gateway с UI | `npm install -g omniroute`, source, `docker run`, `docker compose` | `.env` + dashboard | `combos`, fallback chains, `round-robin`, `p2c`, `least-used`, `cost-optimized`, multi-account | Отличен для coding-agent и CLI сценариев, сильный UI, богатый self-hosted control plane | Более хрупкий профиль, выше ops-сложность, нужен hardening | Когда нужен gateway для developer workflow |
| `LiteLLM` | Универсальный OSS gateway/platform layer | `pip install litellm`, `pip install 'litellm[proxy]'`, Docker, Compose | `config.yaml` + `.env`, часто с DB/UI | Routing, retries, load balancing, virtual keys, spend/cost controls | Самый зрелый универсальный кандидат, очень широкий охват провайдеров | Python-first, может быть тяжелее, чем нужен для простого proxy | Когда нужен основной multi-provider gateway |
| `Portkey Gateway` | Gateway с routing + guardrails | `npx @portkey-ai/gateway`, Docker, Compose, Bun/Node, Workers | Console + configs/policies | Fallbacks, load balancing, provider optimization | Сильный production/governance уклон, хорошие guardrails | Часть восприятия продукта завязана на hosted/cloud экосистему | Когда важны policy, reliability и guardrails |
| `One API` | API management / redistribution hub | Docker, `docker-compose` | Env vars + web UI (`Channels`, `Tokens`) | Load balancing по каналам, channel selection через token suffix | Практичный self-hosted hub, понятный UI | Больше про channel management, чем про интеллектуальный routing | Когда нужен единый API hub с UI |
| `uni-api` | Легкий backend-only router | Docker, Compose, `.pex` binary | `api.yaml` или `CONFIG_URL` | `round_robin`, `weighted_round_robin`, `random`, `fixed_priority`, `smart_round_robin`, auto retry | Очень сильный file-first подход, легко хранить конфиг в git | Меньше control plane и observability из коробки | Когда нужен легкий config-as-code router |
| `Helicone AI Gateway` | Легкий performance-oriented gateway | Cloud-hosted, self-host через Docker/deployment guides | UI wizard для cloud, config guide для self-host | Smart routing, cache, rate limits, tracing | Быстрый, легкий, сильный observability-профиль | Менее очевиден как основной self-host standard choice | Когда важны latency и tracing |
| `RouteLLM` | Intelligent router framework | `pip install \"routellm[serve,eval]\"`, source | Python controller + config | Learned routing strong/weak model pair | Сильный cost/quality routing и eval-first подход | Не полноценный gateway/control plane | Когда нужен именно интеллектуальный выбор модели |

## Способы установки и настройки

### CLIProxyAPI

- Установка:
  - запуск из source как Go service
  - Docker
  - `docker-compose`
- Настройка:
  - основной файл `config.yaml`
  - отдельная директория auth state
  - management API
  - при необходимости companion dashboards вместо ручного YAML-only режима
- Практический профиль:
  - CLI/OAuth-first
  - очень силен для локального AI coding и multi-account subscriptions
  - ближе к proxy core + ecosystem, чем к единому встроенному control plane

### OmniRoute

- Установка:
  - `npm install -g omniroute`
  - запуск из source
  - Docker image
  - Docker Compose профили `base` и `cli`
- Настройка:
  - secrets, ports и runtime через `.env`
  - провайдеры, endpoint keys и combos через dashboard
- Практический профиль:
  - UI-first с env-подкладкой
  - хорош для ручного администрирования

### LiteLLM

- Установка:
  - `pip install litellm`
  - `pip install 'litellm[proxy]'`
  - Docker
  - Compose
- Настройка:
  - `config.yaml`
  - `.env`
  - при расширенном использовании обычно DB + dashboard/admin layer
- Практический профиль:
  - config-first
  - лучший кандидат для reproducible deployment

### Portkey Gateway

- Установка:
  - `npx @portkey-ai/gateway`
  - Docker
  - Docker Compose
  - Bun / Node.js deploy
  - Cloudflare Workers
- Настройка:
  - через gateway Console
  - через `config`/policy подход в интеграции
- Практический профиль:
  - policy-first
  - удобен для production routing rules и guardrails

### One API

- Установка:
  - Docker
  - `docker-compose`
- Настройка:
  - env vars и CLI параметры
  - после запуска настройка через UI
  - сущности: `Channels`, `Tokens`
- Практический профиль:
  - UI-first
  - ближе к API distribution/admin hub

### uni-api

- Установка:
  - Docker
  - Docker Compose
  - `.pex` binary
- Настройка:
  - обязательно `api.yaml`
  - либо удаленный `CONFIG_URL`
- Практический профиль:
  - file-first
  - удобно версионировать и деплоить как config-as-code

### Helicone AI Gateway

- Установка:
  - cloud-hosted вариант
  - self-host через Docker и deployment guides
- Настройка:
  - cloud UI wizard
  - self-host config guide
- Практический профиль:
  - observability/performance-first

### RouteLLM

- Установка:
  - `pip install "routellm[serve,eval]"`
  - install from source
- Настройка:
  - Python controller
  - model pair + threshold + eval config
- Практический профиль:
  - research/router-first
  - не замена полноценному API gateway

## Рекомендация

### Если нужен один основной кандидат

Выбирать `LiteLLM`, если задача инфраструктурная и не завязана в первую очередь на coding CLI.

### Если основной сценарий это разработчики, локальные CLI и coding agents

- Выбирать `CLIProxyAPI`, если главный приоритет это максимально прямой fit под OAuth/CLI и повседневный AI coding.
- Выбирать `OmniRoute`, если нужен тот же сегмент, но с более цельным встроенным UI, dashboard и control plane.

### Если нужна более легкая и декларативная конфигурация

Выбирать `uni-api`.

### Если нужен governance/policy layer

Выбирать `Portkey Gateway`.
