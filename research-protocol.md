# Research Protocol

Срез: 2026-03-20

## Назначение

Этот документ фиксирует протокол исследования open-source решений в условиях избытка кода, репозиториев, proxy-сервисов, SDK, agent frameworks и productized forks.

Цель протокола:

- не пропускать сильные соседние классы решений
- отделять зрелые рабочие инструменты от шумных репозиториев
- быстро переходить от широкого поиска к decision-grade shortlist
- делать исследование воспроизводимым для новых агентов и новых волн рынка

Это не просто памятка “как искать на GitHub”.

Это рабочий стандарт для задач вида:

- найти лучший gateway / proxy / agent runtime
- сравнить похожие репозитории
- выбрать решение под конкретный workflow
- повторить исследование через месяц или квартал без потери качества

## Базовый принцип

В эпоху изобилия кода главная ошибка не “не дочитать README”.

Главная ошибка: **слишком рано сузить класс решений**.

Поэтому протокол строится так:

1. Сначала максимизировать `recall`.
2. Потом повышать `precision`.
3. Только после этого ранжировать.

Если сделать наоборот, легко пропустить сильный репозиторий, который лежит не в той категории, в которой он формально позиционируется.

## Основная модель исследования

Каждое исследование должно идти по четырём слоям:

1. **Problem framing**
   - Что на самом деле хочет пользователь.
   - Какой workflow нужно поддержать.
   - Где проходит граница между “похоже” и “подходит”.
2. **Discovery**
   - Максимально широкий поиск по прямым и смежным категориям.
   - Сбор длинного списка кандидатов.
3. **Validation**
   - Проверка зрелости, install path, operational profile, ecosystem signals и кода.
4. **Decision**
   - Сравнение, ранжирование, pilot recommendation, fallback plan.

## Шаг 1. Правильно зафиксировать задачу

До начала поиска нужно зафиксировать не только тему, но и рабочий контекст.

Нужно ответить на вопросы:

- Это инфраструктурная задача или developer workflow задача.
- Нужен “generic platform” или “tool for one sharp use case”.
- Важнее зрелость, скорость старта, UI, config-as-code, routing, guardrails, cost control, CLI/OAuth, local-first или hosted readiness.
- Нужен продукт для команды, для одного разработчика или для внутреннего сервиса.
- Нужна реальная эксплуатация сейчас или только ресёрч.

Если этого не сделать, discovery почти всегда уйдёт в слишком общую категорию.

## Шаг 2. Строить не один shortlist, а search matrix

Нельзя начинать с одного списка “аналогов”.

Нужно строить `search matrix`, где категории задаются отдельно.

Для типичной задачи про AI gateways / proxies / agents полезно минимум такое разбиение:

- direct category:
  - `llm gateway`
  - `ai gateway`
  - `multi-provider proxy`
- adjacent developer-first category:
  - `cli proxy`
  - `oauth proxy`
  - `claude code proxy`
  - `codex proxy`
  - `gemini cli proxy`
  - `multi-account coding proxy`
- adjacent product category:
  - `ai relay`
  - `api hub`
  - `llm router`
  - `agent backend`
- specialized adjacent category:
  - `research agent`
  - `model router`
  - `policy gateway`
  - `inference control plane`

Если кандидат найден только в соседней категории, это не причина его исключать.

Наоборот: такие проекты часто и дают лучший fit под реальный use case.

## Шаг 3. Работать в двух режимах поиска

Каждое исследование должно сочетать:

- `recall-oriented scan`
- `fit-oriented scan`

### Recall-oriented scan

Задача: не пропустить сильные классы решений.

Что делать:

- искать по прямым и смежным терминам
- смотреть GitHub topics
- смотреть “ports”, “inspired by”, “alternatives”, “related projects”
- идти по ссылкам из README, docs, community dashboards, forks, companion tools
- смотреть не только лидеров по stars, но и новых быстрорастущих кандидатов

### Fit-oriented scan

Задача: понять, что подходит именно под данный workflow.

Что делать:

- сверять описание с реальным сценарием пользователя
- проверять install path
- проверять config model
- проверять operational profile
- проверять ecosystem fit и примеры реального использования

## Шаг 4. Использовать GitHub как граф, а не как список README

GitHub нужно читать как граф связей, а не как набор страниц.

Для каждого сильного кандидата смотреть:

- `README`
- `docs/`
- `releases`
- `topics`
- `used by` / dependents, если доступны
- `network` / forks
- `issues`
- `pull requests`
- `package manifests`
- `docker-compose.yml`
- `config.example.*`
- `examples/`
- `scripts/`
- `LICENSE`
- `SECURITY.md`
- `CHANGELOG`

Особенно важно:

- есть ли официальный install path
- есть ли release binaries
- есть ли Docker path
- как выглядит минимальная конфигурация
- есть ли health/readiness story
- есть ли migration story
- есть ли signs of active maintenance

## Шаг 5. Лучшие практики поиска на GitHub

### Репозиторный поиск

Использовать не один общий запрос, а серии запросов:

- по названию класса решения
- по ключевым интеграциям
- по user workflow
- по соседним категориям

Хорошие типы запросов:

- `llm gateway openai compatible`
- `claude code proxy oauth`
- `multi account coding proxy`
- `openai compatible codex gemini claude`
- `topic:ai-gateway`
- `topic:proxy llm`
- `topic:llm-gateway`

### Code Search

GitHub Code Search полезен не только для чтения конкретного репозитория, но и для поиска паттернов по всему GitHub.

Использовать qualifiers:

- `repo:owner/name`
- `org:...`
- `user:...`
- `language:...`
- `path:...`
- `symbol:...`
- `content:...`
- `is:archived`

Полезные шаблоны:

- `repo:owner/name "OpenAI-compatible"`
- `repo:owner/name path:README.md "Claude Code"`
- `repo:owner/name path:config.example.yaml "round-robin"`
- `repo:owner/name path:internal "v1/models"`
- `repo:owner/name symbol:ChatCompletions`
- `"Claude Code" "Codex" "Gemini CLI" "OpenAI-compatible"`

### Навигация по коду

При чтении репозитория на GitHub полезно использовать:

- symbols pane
- jump to definition
- find references

Это особенно важно в больших репозиториях, где README уже оторван от реального кода.

### Insights и графы

Полезно смотреть:

- repository graphs
- network graph
- traffic, если доступен
- dependency graph
- dependents / used by

Это помогает понять:

- репозиторий реально используется или просто популярен
- есть ли экосистема вокруг него
- есть ли downstream adoption

## Шаг 6. Читать не только README, а install surface

Для кандидата надо проверять не только “что проект обещает”, но и “как его реально поднять”.

Минимальный install audit:

- есть ли release binary
- есть ли Docker image
- есть ли docker compose
- есть ли package manager install
- есть ли quick start, который реально запускается
- есть ли example config
- есть ли .env.example
- есть ли минимальный hello world / smoke test

Часто именно здесь разваливаются красивые README.

## Шаг 7. Искать сигналы реальной эксплуатации

В мире изобилия кода stars уже недостаточно.

Нужно искать признаки, что проект живёт в реальной эксплуатации:

- свежие commits
- не мёртвые issues
- активные PR
- release cadence
- официальные binaries
- Docker images
- companion tools
- dashboards / desktop apps / plugins вокруг ядра
- упоминания в “inspired by” или “built on top of”
- наличие migration / upgrade notes

Сильный сигнал:

- вокруг репозитория выросла экосистема обвязок

Слабый сигнал:

- один красивый README без operational следов

## Шаг 8. Делать code-grounded валидацию

После shortlist нельзя опираться только на README.

Нужно читать кодовые признаки:

- какие route'ы реально зарегистрированы
- есть ли config reload
- как реализован auth
- есть ли retry / circuit breaker / routing strategy
- как пробрасываются upstream ошибки
- есть ли health/readiness endpoints
- есть ли watcher'ы, storage adapters, migrations

Минимальный кодовый аудит:

- `README`
- `package.json` / `pyproject.toml` / `go.mod`
- `docker-compose.yml`
- `config.example.*`
- `cmd/` или entrypoints
- `internal/api` или `src/server`
- `providers` / `translators` / `routing`

## Шаг 9. Делать dry run как можно раньше

Dry run нужен не в конце, а как только shortlist сузился до 2-4 кандидатов.

Что проверять в dry run:

- install path
- first startup
- minimal config
- health / readiness
- `/v1/models` или аналогичный listing endpoint
- один запрос с dummy key, чтобы увидеть прозрачность upstream error
- поведение без токена
- hot reload, если это заявлено

Dry run нужен для ответа на вопрос:

> Сколько реального трения даст проект до того, как он начнёт приносить пользу?

## Шаг 10. Разделять классы качества

Нужно оценивать кандидатов по разным осям, а не сводить всё к одному “лучший”.

Минимальные оси:

- `Fit`
  - насколько точно попадает в реальный workflow
- `Maturity`
  - насколько проект зрелый и живой
- `Onboarding`
  - насколько быстро можно поднять
- `Operability`
  - логи, health, dashboard, day-2 ops
- `Config model`
  - UI-first, YAML-first, env-first, code-first
- `Risk`
  - security, fragility, external dependency risk

Одна из типичных ошибок:

- выбрать самый зрелый generic project, когда нужен узкий workflow-fit

Другая типичная ошибка:

- выбрать самый sharp tool, игнорируя ops risk

## Шаг 11. Обязательно проверять соседние классы решений

Перед финальным ranking нужно сделать отдельный checkpoint:

### Miss Prevention Checklist

- Проверены ли прямые аналоги.
- Проверены ли соседние категории.
- Проверены ли proxy/relay/CLI варианты, если задача касается developer workflow.
- Проверены ли forks, ports и inspired-by проекты.
- Проверены ли companion dashboards и ecosystem tools.
- Проверены ли проекты, которые называют себя не так, как рынок называет их класс.
- Пересобран ли shortlist после уточнения use case.

Если на любой вопрос ответ “нет”, ranking преждевременен.

## Шаг 12. Правильно использовать внешние сигналы

Нельзя опираться только на stars.

Но и нельзя игнорировать внешние сигналы.

Нужно учитывать:

- рабочие референсы от коллег
- наличие production use cases
- число stars и forks
- скорость обновлений
- репутацию maintainers
- степень экосистемной связанности

Хорошее правило:

- `colleague signal` может перевесить абстрактный market ranking, если use case совпадает

## Шаг 13. Что обязательно сохранять в артефактах исследования

Чтобы исследование можно было переиспользовать, нужно сохранять не только вывод.

Нужны отдельные артефакты:

- `comparison-matrix.md`
- `decision-memo.md`
- `dry-run-report.md`
- `research-brief.md`
- `sources.md`
- `research-protocol.md`

Особенно важно хранить:

- почему shortlist был именно таким
- какие категории были просмотрены
- какие кандидаты были исключены и почему
- что уже протестировано руками
- какие ограничения среды повлияли на вывод

## Шаг 14. Антипаттерны исследования

Ниже ошибки, которые чаще всего и ломают качество поиска.

### Антипаттерн 1. Слишком ранняя категоризация

Ошибка:

- “пользователь сказал gateway, значит ищем только gateway”

Последствие:

- пропускаются proxy / relay / CLI-centric решения

### Антипаттерн 2. Слишком ранний ranking

Ошибка:

- ранжировать после 3-5 первых сильных репозиториев

Последствие:

- shortlist закрепляется раньше, чем закончен discovery

### Антипаттерн 3. README-driven research

Ошибка:

- исследование строится вокруг маркетингового описания

Последствие:

- project surface путается с реальной operational ценностью

### Антипаттерн 4. Игнорирование ecosystem graph

Ошибка:

- не смотреть forks, network, companion tools, inspired-by

Последствие:

- теряется понимание, какие решения реально стали центром экосистемы

### Антипаттерн 5. Нет второго прохода после уточнения use case

Ошибка:

- пользователь уточнил сценарий, а shortlist остался прежним

Последствие:

- решение остаётся оптимизировано под старую формулировку задачи

## Шаг 15. Практический шаблон работы нового агента

Если новый агент повторяет исследование, он должен пройти такой порядок:

1. Зафиксировать `problem framing`.
2. Собрать `search matrix`.
3. Сделать recall-first scan по GitHub и смежным источникам.
4. Сформировать длинный список.
5. Отдельно проверить adjacent categories.
6. Сузить до shortlist.
7. Сделать code-grounded validation.
8. Провести dry run по 2-4 кандидатам.
9. Обновить ranking.
10. Сохранить decision artifacts.

## Пример запросов для задач про developer-first AI tooling

Для задач класса, похожего на это исследование, полезно использовать такие поисковые линии:

- `openai compatible proxy codex claude code gemini`
- `oauth proxy claude code codex`
- `multi account ai coding proxy`
- `agentic coding proxy`
- `llm gateway coding agents`
- `round-robin ai proxy oauth`
- `topic:ai-gateway codex`
- `"inspired by" "OmniRoute"`
- `"built on top of" "CLIProxyAPI"`
- `"Claude Code" "OpenAI-compatible API"`

## Как принимать решение в конце

Финальный вывод должен всегда быть трёхслойным:

1. `Лучший универсальный вариант`
2. `Лучший вариант под конкретный workflow пользователя`
3. `План B`

Тогда итог не ломается при смене приоритета.

## Почему этот протокол важен

Рынок движется к ситуации, где:

- хорошего кода слишком много
- форков, портов и proxy-слоёв становится всё больше
- README всё меньше означает реальное качество
- рядом с каждым зрелым проектом быстро появляется десяток productized forks

Поэтому поиск становится отдельной инженерной компетенцией.

Побеждает не тот, кто “нашёл популярный репозиторий”.

Побеждает тот, кто:

- не пропустил сильный соседний класс решений
- быстро отличил настоящую зрелость от поверхностной популярности
- проверил operational reality, а не только marketing surface
- оставил после себя воспроизводимое исследование

## Источники и опорные практики

- GitHub Docs: navigating code on GitHub
  - https://docs.github.com/en/github/managing-files-in-a-repository/navigating-code-on-github
- GitHub Docs: finding and understanding example code
  - https://docs.github.com/en/get-started/learning-to-code/finding-and-understanding-example-code
- GitHub Docs: about repository graphs
  - https://docs.github.com/articles/using-graphs
- GitHub Docs: about the dependency graph
  - https://docs.github.com/code-security/supply-chain-security/understanding-your-software-supply-chain/about-the-dependency-graph
- GitHub CLI manual: `gh search`
  - https://cli.github.com/manual/gh_search
