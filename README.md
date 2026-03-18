# LLM Gateway Research

Срез: 2026-03-18

## Цель

Выбрать open-source решение, похожее на OmniRoute, для работы с несколькими LLM-провайдерами и гибкого переключения между ними.

## Короткий вывод

Если нужен максимально универсальный и зрелый OSS gateway, основной кандидат: `LiteLLM`.

Если нужен self-hosted gateway с сильным уклоном в developer workflow, coding agents, OAuth/CLI accounts и визуальное управление fallback-сценариями, сильный кандидат: `OmniRoute`.

Если нужен более production/governance-ориентированный gateway с политиками и guardrails, сильный кандидат: `Portkey Gateway`.

Если нужен более легкий и file-first вариант без тяжелой control plane, сильный кандидат: `uni-api`.

## Shortlist

1. `BerriAI/litellm`
2. `diegosouzapw/OmniRoute`
3. `Portkey-AI/gateway`
4. `yym68686/uni-api`
5. `songquanpeng/one-api`
6. `Helicone/ai-gateway`
7. `lm-sys/RouteLLM`

## Что важно при выборе

- Насколько зрелая поддержка множества провайдеров.
- Есть ли единая OpenAI-compatible точка входа.
- Насколько гибко задаются fallback, load balancing и provider selection.
- Как устроена конфигурация: UI-first, YAML-first, env-first или policy/code-first.
- Насколько воспроизводим деплой: Docker, compose, package install, binary.
- Есть ли observability, quotas, rate limiting, ключи доступа и базовый security layer.
- Не превращается ли решение в слишком тяжелую платформу, если нужен только routing.

## Основной вывод по OmniRoute

`OmniRoute` заметно отличается от части конкурентов тем, что это не просто единый proxy для BYOK API keys.

Сильные стороны:

- Сильный уклон в CLI и coding-agent use case.
- Богатый self-hosted UI для `Providers`, `Combos`, `Analytics`, `Health`, `Endpoints`.
- Гибкие стратегии выбора и fallback.
- Поддержка OAuth, API key и multi-account сценариев.
- Удобен для локального и полу-ручного использования командой разработчиков.

Слабые стороны:

- Более широкая и хрупкая поверхность интеграции, чем у “чистого” gateway.
- Сильная зависимость от совместимости с внешними CLI/OAuth-потоками.
- Выше ops-сложность и maintenance burden.
- Перед production нужен явный hardening конфигурации.

## Рекомендуемый порядок выбора

1. Если нужен универсальный gateway для команды и приложений: смотреть `LiteLLM`.
2. Если основной сценарий это coding assistants и developer tools: смотреть `OmniRoute`.
3. Если важны policy/governance/guardrails: смотреть `Portkey Gateway`.
4. Если нужен простой file-based router: смотреть `uni-api`.

## Файлы в этой папке

- `comparison-matrix.md` — прикладная матрица по установке, настройке и сценариям выбора.
- `decision-memo.md` — практическая рекомендация по выбору под разные ограничения команды.
- `dry-run-report.md` — фактические результаты локального dry run по OmniRoute, LiteLLM и uni-api.
- `one-page-summary.md` — очень короткий итог: что выбирать и почему.
- `pilot-plan.md` — короткий план пилота для проверки финальных кандидатов на реальных сценариях.
- `research-brief.md` — исходный запрос, уточнения пользователя и рамка для повторного запуска исследования в будущем.
- `sources.md` — ссылки и краткие примечания по использованным источникам.
