# LLM Gateway Research

Срез: 2026-03-20

## Цель

Выбрать open-source решение, похожее на OmniRoute, для работы с несколькими LLM-провайдерами и гибкого переключения между ними.

## Короткий вывод

Если нужен максимально универсальный и зрелый OSS gateway, основной инфраструктурный кандидат: `LiteLLM`.

Если нужен инструмент именно для coding, CLI, OAuth subscriptions и мультипровайдерной агентской разработки, новый top-tier кандидат: `CLIProxyAPI`.

Если нужен self-hosted gateway с сильным уклоном в developer workflow, coding agents, OAuth/CLI accounts и встроенным визуальным control plane, сильный кандидат: `OmniRoute`.

Если нужен более production/governance-ориентированный gateway с политиками и guardrails, сильный кандидат: `Portkey Gateway`.

Если нужен более легкий и file-first вариант без тяжелой control plane, сильный кандидат: `uni-api`.

## Shortlist

1. `router-for-me/CLIProxyAPI`
2. `diegosouzapw/OmniRoute`
3. `BerriAI/litellm`
4. `Portkey-AI/gateway`
5. `yym68686/uni-api`
6. `songquanpeng/one-api`
7. `Helicone/ai-gateway`
8. `lm-sys/RouteLLM`

## Что важно при выборе

- Насколько зрелая поддержка множества провайдеров.
- Есть ли единая OpenAI-compatible точка входа.
- Насколько гибко задаются fallback, load balancing и provider selection.
- Как устроена конфигурация: UI-first, YAML-first, env-first или policy/code-first.
- Насколько воспроизводим деплой: Docker, compose, package install, binary.
- Есть ли observability, quotas, rate limiting, ключи доступа и базовый security layer.
- Не превращается ли решение в слишком тяжелую платформу, если нужен только routing.

## Основной вывод по OmniRoute и CLIProxyAPI

`OmniRoute` и `CLIProxyAPI` теперь выглядят как финальная пара именно для developer-first и agentic coding workflow.

`CLIProxyAPI` сильнее по прямому fit, зрелости и traction в OAuth/CLI-сценариях.

`OmniRoute` сильнее как более цельный self-hosted продукт с интегрированным dashboard, health, combos и ops-поверхностью.

Сильные стороны:

- `CLIProxyAPI`: очень точное попадание в `Codex` / `Claude Code` / `Gemini` / `Qwen` / `iFlow`, multi-account и OAuth routing.
- `CLIProxyAPI`: сильные сигналы зрелости и экосистемы вокруг desktop/dashboard-обвязок.
- `OmniRoute`: богатый self-hosted UI для `Providers`, `Combos`, `Analytics`, `Health`, `Endpoints`.
- `OmniRoute`: гибкие стратегии выбора и fallback.
- Оба варианта хорошо попадают в multi-provider coding workflow.

Слабые стороны:

- Оба варианта зависят от совместимости с внешними CLI/OAuth-потоками и потому потенциально более хрупки, чем “чистый” gateway.
- `CLIProxyAPI` выглядит более config/docs-centric, а control plane более фрагментирован вокруг companion-проектов.
- `OmniRoute` шире по продуктовой поверхности, поэтому выше ops-сложность и maintenance burden.
- Перед production оба варианта требуют явного hardening конфигурации.

## Рекомендуемый порядок выбора

1. Если нужен инструмент для экстремального вайбкодинга, CLI/OAuth и агентской разработки: смотреть `CLIProxyAPI`.
2. Если нужен тот же developer-first сегмент, но с более цельным встроенным UI/control plane: смотреть `OmniRoute`.
3. Если нужен универсальный gateway для команды и приложений: смотреть `LiteLLM`.
4. Если важны policy/governance/guardrails: смотреть `Portkey Gateway`.
5. Если нужен простой file-based router: смотреть `uni-api`.

## Файлы в этой папке

- `agent-layer-decision.md` — зафиксированное решение не добавлять `sgr-agent-core` на текущем этапе.
- `comparison-matrix.md` — прикладная матрица по установке, настройке и сценариям выбора.
- `decision-memo.md` — практическая рекомендация по выбору под разные ограничения команды.
- `dry-run-report.md` — фактические результаты локального dry run по CLIProxyAPI, OmniRoute, LiteLLM и uni-api.
- `one-page-summary.md` — очень короткий итог: что выбирать и почему.
- `pilot-plan.md` — короткий план пилота для проверки финальных кандидатов на реальных сценариях.
- `research-brief.md` — исходный запрос, уточнения пользователя и рамка для повторного запуска исследования в будущем.
- `sources.md` — ссылки и краткие примечания по использованным источникам.

## Важная оговорка

`CLIProxyAPI` уже добавлен в `dry-run-report.md` как фактически прогнанный кандидат, но его dry run выполнялся через официальный Linux release binary, а не через source-build, потому что в этой среде отсутствует Go toolchain.
