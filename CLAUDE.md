# Agent Instructions — medincident-proto

Единый источник правды для всех protobuf-контрактов системы medincident. Schema-only репо: здесь нет runtime-кода, downstream-сервисы генерят Go у себя через `buf generate`.

---

## Раскладка

```
api/proto/
├── medincident/                             — first-party доменные контракты
│   ├── events/v1/envelope.proto              — доменный event envelope
│   ├── orgstructure/v1/events.proto          — события агрегата organization
│   └── shared/geo/v1/types.proto             — общие geo value-типы
└── zitadel/                                 — 1:1 passthrough зеркала Zitadel Actions v2 webhook'ов
    ├── events/v1/envelope.proto              — Zitadel webhook envelope
    ├── sessions/v1/{events,types}.proto      — session события + типы
    └── users/v1/{events,types}.proto         — user события + типы

buf.yaml                                     — buf v2 module config (lint STANDARD, breaking FILE)
buf.gen.docs.yaml                            — protoc-gen-doc template для docs/
Taskfile.yml                                 — task runner (fmt/lint/breaking/gen)
.pre-commit-config.yaml                      — fmt:check / lint / breaking хуки
docs/                                        — сгенерированные Markdown-доки, коммитятся в репо
api/openapi/                                 — зарезервировано под будущие OpenAPI контракты
```

Каждая leaf-директория = один proto-пакет, один для всего `.proto` или несколько файлов с общим `package <tree>.<domain>.v1;`.

---

## Два дерева, две зоны ответственности

- **`api/proto/medincident/`** — first-party доменные контракты (события, агрегаты, shared value-типы). Это CQRS-доменные события, которые emitит `medincident-command-service`. Описывают *что произошло в домене*.
- **`api/proto/zitadel/`** — 1:1 passthrough зеркала payload'ов Zitadel Actions v2 webhook'ов. Владеет ими gateway-сервис `medincident-zitadel-actions`, который принимает Zitadel webhook'и и ре-публикует их в NATS JetStream. Описывают *что Zitadel нам рассказал* и намеренно сохраняют Zitadel-нативную форму (instance_id, sequence, resource_owner), чтобы консьюмеры могли читать их без трансляции.

**Инвариант:** два дерева никогда не ссылаются друг на друга. `medincident.*` не импортирует `zitadel.*` и наоборот. Смысл разделения — Zitadel может менять форму своих webhook'ов, не ломая доменные контракты; рефакторинг домена не трогает passthrough-зеркала.

---

## Тулчейн

Ни Docker, ни C-toolchain, ни отдельная установка `protoc` не нужны. Всё идёт через `go tool`:

| Инструмент | Источник |
|---|---|
| `buf` | `go tool buf` (см. `go.mod`) |
| `protoc-gen-doc` | `go tool protoc-gen-doc` |
| `golangci-lint` | `go tool` (зарезервировано) |

Требования: **Go 1.26+**, [Task](https://taskfile.dev), [pre-commit](https://pre-commit.com).

---

## Частые команды

```bash
task fmt           # отформатировать все proto in place
task fmt:check     # проверка формата (dry-run, без модификаций)
task lint          # buf lint (STANDARD, без FIELD_LOWER_SNAKE_CASE)
task breaking      # buf breaking против origin/main (fallback на local main)
task gen           # зонтичная точка входа для всех кодгенов (сейчас: gen:docs)
task gen:docs      # перегенерировать Markdown-доки в docs/
task gen:check     # CI-gate: fail если `task gen` породил бы дифф в docs/
```

`task gen` — единая точка входа для всех кодгенераторов. Сегодня дёргает только `gen:docs`; любой будущий генератор (validation rules, OpenAPI, client stubs) добавляется как новая `gen:*` таска и подцепляется под `gen`.

---

## Pre-commit хуки

Конфиг в [`.pre-commit-config.yaml`](.pre-commit-config.yaml). При изменении любого `*.proto` запускаются:

| Хук | Что делает |
|---|---|
| `task-fmt-check` | `task fmt:check` — падает, если какой-то proto не отформатирован. Чинится через `task fmt`. |
| `task-lint` | `task lint` — buf style lint. |
| `task-breaking` | `task breaking` — `buf breaking` против `origin/main`. Падает, если коммит удаляет/ренамит поле на существующем пакете. |

Pre-commit **не** регенерирует доки автоматически — это оставлено разработчику, чтобы хуки были быстрыми. Рассинхрон `docs/` и `.proto` ловит CI через `task gen:check`. Перед push'ем запускай `task gen:docs` руками, если правил схемы.

Общие hygiene-хуки (trailing whitespace, EOF newline, YAML validation, large-file guard, merge-conflict markers) запускаются на каждый коммит независимо от типа файла.

---

## Редактирование протоколов

### Добавить поле в существующее сообщение

1. Отредактируй `.proto` файл. Используй следующий свободный номер поля — **никогда не переиспользуй старый**.
2. `task fmt` — нормализовать форматирование.
3. `task lint` — проверить style.
4. `task breaking` — убедиться, что изменение wire-compatible.
5. `task gen:docs` — обновить Markdown-доки.
6. Коммить `.proto` и `docs/` **одним коммитом**. Pre-commit перепрогонит fmt/lint/breaking; `gen:check` запускается только в CI.

### Добавить новый пакет

1. Создай директорию под `api/proto/<tree>/<domain>/v1/`, где `<tree>` = `medincident` или `zitadel`.
2. Добавь `.proto` с `package <tree>.<domain>.v1;`, соответствующим пути директории — это навязывается `buf lint`.
3. `task gen:docs` автоматически подхватит новый пакет — генератор итерируется по каждой leaf-директории с `.proto` файлами.

### Версионирование

- Breaking changes бампят версию пакета: `v1` → `v2`. Старый пакет остаётся живым, пока все консьюмеры не переедут.
- `task breaking` + pre-commit hook `task-breaking` валят коммит, который удаляет или ренамит поле на существующем пакете. Если breaking change реально нужен — заводи новую директорию `v2`, не правь `v1`.

---

## Генерация документации

`task gen:docs` использует [`protoc-gen-doc`](https://github.com/pseudomuto/protoc-gen-doc) (vendored как `go tool` в `go.mod`) и итерируется по каждой leaf-директории с `.proto` файлами. Для каждой генерится один Markdown-файл `docs/<tree>/<domain>/v1/README.md`.

Выход пост-процессится через `awk`: срезается trailing whitespace и схлопываются хвостовые пустые строки, чтобы регенерация была идемпотентной (иначе pre-commit whitespace-хуки падают).

`docs/` закоммичены в репу — их видно прямо на GitHub, без установки buf.

Taskfile чистит `docs/` через `rm -rf docs` перед регенерацией — это нужно, чтобы переименованные или удалённые пакеты не оставляли stale-файлов. Не добавляй ручных правок в `docs/` — они потеряются.

---

## Downstream-консьюмеры

Downstream-сервисы (`medincident-command-service`, `medincident-query-service`, `medincident-zitadel-actions`) генерят Go из этой репы через свой `buf.gen.yaml`. Два поддерживаемых паттерна:

**Локальная разработка (sibling checkout):**
```yaml
inputs:
  - directory: ../medincident-proto/api/proto
    paths:
      - medincident   # или: zitadel
```

**Стабильное потребление (pinned tag):**
```yaml
inputs:
  - git_repo: https://github.com/medincident/medincident-proto.git
    tag: vX.Y.Z
    subdir: api/proto
    paths:
      - medincident
```

Канонический пример `git_repo:` формы — `medincident-zitadel-actions/buf.gen.yaml`.

**Консьюмеры всегда фильтруют `paths:`** только до нужного поддерева. Не тяни `zitadel/`, если сервис работает только с доменными событиями, и наоборот.

---

## buf config

- `buf.yaml` v2, один модуль `api/proto`.
- Dep: `buf.build/protocolbuffers/wellknowntypes` (через `buf.lock`).
- Lint: `STANDARD` минус `FIELD_LOWER_SNAKE_CASE` (исключение нужно для passthrough zitadel-полей, которые повторяют форму Zitadel 1:1).
- Breaking: `FILE` — отслеживаем wire-compatibility на уровне файла.

---

## Что НЕ надо делать

- Не добавляй Go-код в репу. Это schema-only, downstream генерит сам.
- Не добавляй `Makefile` — все команды живут в `Taskfile.yml`.
- Не правь файлы в `docs/` руками — они регенерируются `task gen:docs`.
- Не переиспользуй номера полей, даже если поле удалили. Используй следующий свободный.
- Не создавай cross-tree импорты между `medincident/` и `zitadel/`.
- Не коммить `.proto` без соответствующего обновления `docs/` — CI упадёт на `task gen:check`.
- Не используй `fmt.Errorf`-style правки в schema комментариях: доки — это просто Markdown, рендерятся protoc-gen-doc'ом из proto comments.
- Не бампай `v1` → `v2` ради ненужного рефакторинга. Бампается только при реальном breaking change, когда нельзя обойтись добавлением нового поля.

---

## CI

- `.github/` содержит workflow'ы для proto format + lint + breaking checks и Dependabot для Go modules + GitHub Actions.
- CI-gate `task gen:check` запускает `task gen` и валит билд, если `git diff -- docs/` непустой. Это единственный способ поймать забытую регенерацию доков.
