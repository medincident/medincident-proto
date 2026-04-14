# Agent Instructions — medincident-proto

Единый источник правды для всех protobuf-контрактов системы medincident. Downstream-сервисы генерят Go у себя через `buf generate`, за одним исключением: в репе собирается Go-пакет HTTP→gRPC gateway'я (`gen/medincident/service/**`) и машиночитаемая OpenAPI v2 спека (`api/openapi/medincident.swagger.json`). См. раздел "Gateway codegen" ниже.

---

## Раскладка

```
api/proto/
├── medincident/                             — first-party доменные контракты
│   ├── event/                                — события (по одному leaf-пакету на агрегат)
│   └── service/                              — command-side gRPC сервисы с google.api.http аннотациями
└── zitadel/                                 — 1:1 passthrough зеркала Zitadel Actions v2 webhook'ов
    ├── events/v1/envelope.proto              — Zitadel webhook envelope
    ├── sessions/v1/{events,types}.proto      — session события + типы
    └── users/v1/{events,types}.proto         — user события + типы

api/openapi/
└── medincident.swagger.json                  — сгенеренная OpenAPI v2 спека (merged, только service/**)

gen/
└── medincident/service/                      — сгенеренный Go-код для HTTP→gRPC gateway
    └── **                                    — *.pb.go + *_grpc.pb.go + *.pb.gw.go

buf.yaml                                     — buf v2 module config (lint STANDARD, breaking FILE)
buf.gen.docs.yaml                            — protoc-gen-doc template для docs/
buf.gen.gateway.yaml                         — gRPC/grpc-gateway/openapiv2 template для gen/ и api/openapi/
Taskfile.yml                                 — task runner (fmt/lint/breaking/gen)
.pre-commit-config.yaml                      — fmt:check / lint / breaking / gen:check хуки
docs/                                        — сгенерированные Markdown-доки, коммитятся в репо
```

Каждая leaf-директория = один proto-пакет, один для всего `.proto` или несколько файлов с общим `package <tree>.<domain>.v1;`.

---

## Два дерева, две зоны ответственности

- **`api/proto/medincident/`** — first-party доменные контракты (события, агрегаты, shared value-типы). Это CQRS-доменные события, которые emitит `medincident-command-service`. Описывают *что произошло в домене*.
- **`api/proto/zitadel/`** — 1:1 passthrough зеркала payload'ов Zitadel Actions v2 webhook'ов. Владеет ими gateway-сервис `medincident-zitadel-actions`, который принимает Zitadel webhook'и и ре-публикует их в NATS JetStream. Описывают *что Zitadel нам рассказал* и намеренно сохраняют Zitadel-нативную форму (instance_id, sequence, resource_owner), чтобы консьюмеры могли читать их без трансляции.

**Инвариант:** два дерева никогда не ссылаются друг на друга. `medincident.*` не импортирует `zitadel.*` и наоборот. Смысл разделения — Zitadel может менять форму своих webhook'ов, не ломая доменные контракты; рефакторинг домена не трогает passthrough-зеркала.

---

## Gateway codegen

В репе собирается один Go-пакет, который служит основой для будущего HTTP→gRPC gateway бинаря. Его scope — **только** `api/proto/medincident/service/**`. Доменные events, query-side протоколы и zitadel passthrough **не** попадают в `gen/`: их Go-кодом владеют downstream-сервисы.

- **`buf.gen.gateway.yaml`** — buf template с фильтром `paths: [api/proto/medincident/service]`, managed mode (`go_package_prefix = github.com/medincident/medincident-proto/gen`) и четырьмя плагинами: `protoc-gen-go`, `protoc-gen-go-grpc`, `protoc-gen-grpc-gateway`, `protoc-gen-openapiv2`. Managed mode инжектит `option go_package` в генерацию — сами `.proto` файлы остаются repo-agnostic, downstream ставит свой prefix в своём `buf.gen.yaml`.
- **`gen/medincident/service/**/`** — `*.pb.go` + `*_grpc.pb.go` + `*.pb.gw.go`. Эти три файла на каждый `.proto` — самодостаточный Go-пакет, который компилируется сам по себе и импортится будущим `cmd/gateway`. Downstream-сервисы сюда **не** смотрят — они продолжают генерить своё у себя.
- **`api/openapi/medincident.swagger.json`** — единый мёрженный OpenAPI v2 файл (один документ на всё `service/**` дерево). Плагин `protoc-gen-openapiv2` конфигурируется через `allow_merge=true` + `merge_file_name=medincident` + `strategy: all` (без `strategy: all` buf дёргает плагин per-directory и merge не срабатывает). Коммитится в репу как ещё один machine-readable contract, рядом с `docs/` Markdown. Используется фронтендом, Postman'ом и будущим API-портальным UI.

Почему код живёт здесь, а не в downstream: HTTP gateway — инфраструктурный кусок, независимый от бизнес-логики command-service. Ему нужны Go-стабы прото-контрактов, и держать их рядом с proto источником устраняет лишний слой generation drift.

Почему scope узкий: event-протоколы описывают wire-формат публикуемых NATS-сообщений, у них нет HTTP-интерфейса и нет смысла генерить для них gateway-код. То же для zitadel passthrough — это inbound webhook payloads, не outbound HTTP API.

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
task gen           # зонтичная точка входа: параллельно запускает gen:docs и gen:gateway
task gen:docs      # перегенерировать Markdown-доки в docs/
task gen:gateway   # перегенерировать gen/medincident/service/** и api/openapi/medincident.swagger.json
task gen:check     # CI-gate: fail если `task gen` породил бы дифф в docs/, gen/ или api/openapi/
```

`task gen` — единая точка входа для всех кодгенераторов. Обе подтаски (`gen:docs` и `gen:gateway`) выполняются параллельно через Task `deps:`. Любой будущий генератор подцепляется под `gen` тем же способом.

---

## Pre-commit хуки

Конфиг в [`.pre-commit-config.yaml`](.pre-commit-config.yaml). При изменении любого `*.proto` запускаются:

| Хук | Что делает |
|---|---|
| `task-fmt-check` | `task fmt:check` — падает, если какой-то proto не отформатирован. Чинится через `task fmt`. |
| `task-lint` | `task lint` — buf style lint. |
| `task-breaking` | `task breaking` — `buf breaking` против `origin/main`. Падает, если коммит удаляет/ренамит поле на существующем пакете. |
| `task-gen-check` | `task gen:check` — падает, если изменения в `.proto` файлах не сопровождаются регенерацией `docs/`, `gen/` или `api/openapi/`. Чинится через `task gen`. |

Pre-commit `task-gen-check` хук запускает `task gen:check`, который сам вызывает `task gen` и регенерирует `docs/`, `gen/`, `api/openapi/` при изменении `.proto`. Если хук падает на рассинхроне — `git add` обновлённые артефакты и коммить заново. CI дублирует эту же проверку через `task gen:check`.

Общие hygiene-хуки (trailing whitespace, EOF newline, YAML validation, large-file guard, merge-conflict markers) запускаются на каждый коммит независимо от типа файла.

---

## Редактирование протоколов

### Добавить поле в существующее сообщение

1. Отредактируй `.proto` файл. Используй следующий свободный номер поля — **никогда не переиспользуй старый**.
2. `task fmt` — нормализовать форматирование.
3. `task lint` — проверить style.
4. `task breaking` — убедиться, что изменение wire-compatible.
5. `task gen` — перегенерировать все артефакты (`docs/`, `gen/`, `api/openapi/`).
6. Коммить `.proto` и все изменения в `docs/`, `gen/`, `api/openapi/` **одним коммитом**. Pre-commit перепрогонит fmt/lint/breaking/gen:check.

### Добавить новый пакет

1. Создай директорию под `api/proto/<tree>/<domain>/v1/`, где `<tree>` = `medincident` или `zitadel`.
2. Добавь `.proto` с `package <tree>.<domain>.v1;`, соответствующим пути директории — это навязывается `buf lint`.
3. `task gen` автоматически подхватит новый пакет:
   - `gen:docs` генерит Markdown для любого нового leaf-пакета.
   - `gen:gateway` подхватит новый пакет **только** если он под `medincident/service/**`; любые другие деревья не попадают в `gen/` и `api/openapi/`.

### Версионирование

- Breaking changes бампят версию пакета: `v1` → `v2`. Старый пакет остаётся живым, пока все консьюмеры не переедут.
- `task breaking` + pre-commit hook `task-breaking` валят коммит, который удаляет или ренамит поле на существующем пакете. Если breaking change реально нужен — заводи новую директорию `v2`, не правь `v1`.

---

## Генерация артефактов

Репа генерит три вида артефактов, все три коммитятся в репу:

**1. Markdown-доки** (`docs/**/README.md`) — `task gen:docs` использует [`protoc-gen-doc`](https://github.com/pseudomuto/protoc-gen-doc) (vendored как `go tool` в `go.mod`) и итерируется по каждой leaf-директории с `.proto` файлами. Для каждой генерится один Markdown-файл `docs/<tree>/<domain>/v1/README.md`. Выход пост-процессится через `awk` (срезается trailing whitespace, схлопываются хвостовые пустые строки) для идемпотентности — иначе pre-commit whitespace-хуки падают. Taskfile чистит `docs/` через `rm -rf docs` перед регенерацией, чтобы переименованные или удалённые пакеты не оставляли stale-файлов.

**2. Gateway Go-пакет** (`gen/medincident/service/**`) — `task gen:gateway` использует `protoc-gen-go`, `protoc-gen-go-grpc`, `protoc-gen-grpc-gateway` из `buf.gen.gateway.yaml`. Inputs фильтруется через `paths: [api/proto/medincident/service]`, так что ничего из `event/**` или `zitadel/**` в `gen/` не попадает. Taskfile чистит `gen/` через `rm -rf gen` перед регенерацией.

**3. OpenAPI v2 спека** (`api/openapi/medincident.swagger.json`) — тот же `buf.gen.gateway.yaml`, плагин `protoc-gen-openapiv2` с `allow_merge=true`, `merge_file_name=medincident` и `strategy: all`. На выходе — один мёрженный JSON-файл на всё `service/**` дерево.

CI-gate `task gen:check` валит билд, если любой из трёх артефактов дрейфует от источника. Не добавляй ручных правок в `docs/`, `gen/` или `api/openapi/` — они потеряются.

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

**Важно: downstream не импортит из `github.com/medincident/medincident-proto/gen/`.** Пакет `gen/` существует только ради будущего `cmd/gateway` бинаря (или отдельной gateway-репы, которая подключит proto-репу как Go-модуль). Command-service и query-service продолжают генерить свои Go-типы у себя, через sibling-checkout или git_repo паттерн выше. Это даёт им независимый контроль над `paths:` фильтром, плагинами и выходом — им не нужны gateway-стабы, им нужны серверные gRPC стабы плюс их собственные proto-типы. Managed mode в downstream `buf.gen.yaml` ставит свой `go_package_prefix` (например, `github.com/ulbwa/medincident-command-service/gen`), так что сгенеренные Go-файлы не конфликтуют с `gen/` из этой репы.

---

## buf config

- `buf.yaml` v2, один модуль `api/proto`.
- Deps: `buf.build/protocolbuffers/wellknowntypes`, `buf.build/googleapis/googleapis`, `buf.build/grpc-ecosystem/grpc-gateway` (последняя нужна для `google.api.http` и `openapiv2_*` аннотаций в `service/**` протоколах).
- Lint: `STANDARD` минус `FIELD_LOWER_SNAKE_CASE` (исключение нужно для passthrough zitadel-полей, которые повторяют форму Zitadel 1:1).
- Breaking: `FILE` — отслеживаем wire-compatibility на уровне файла.

---

## Что НЕ надо делать

- Не правь файлы в `docs/`, `gen/` или `api/openapi/` руками — они регенерируются `task gen`.
- Не переиспользуй номера полей, даже если поле удалили. Используй следующий свободный.
- Не создавай cross-tree импорты между `medincident/` и `zitadel/`.
- Не коммить `.proto` без соответствующего `task gen` — CI упадёт на `task gen:check`, pre-commit упадёт на `task-gen-check`.
- Не расширяй `paths:` в `buf.gen.gateway.yaml` за пределы `medincident/service`. Events, query-side, zitadel — всё это не входит в gateway-пакет by design.
- Не объявляй `option go_package` в самих `.proto` файлах — proto файлы repo-agnostic, go_package инжектится через managed mode в buf.gen.yaml каждого потребителя (включая локальный `buf.gen.gateway.yaml`).
- Не импортируй `github.com/medincident/medincident-proto/gen/` из `medincident-command-service` / `medincident-query-service` / `medincident-zitadel-actions`. Они генерят своё сами.
- Не используй `fmt.Errorf`-style правки в schema комментариях: доки — это просто Markdown, рендерятся protoc-gen-doc'ом из proto comments.
- Не бампай `v1` → `v2` ради ненужного рефакторинга. Бампается только при реальном breaking change, когда нельзя обойтись добавлением нового поля.
- Не добавляй `Makefile` — все команды живут в `Taskfile.yml`.

---

## CI

- `.github/` содержит workflow'ы для proto format + lint + breaking checks и Dependabot для Go modules + GitHub Actions.
- CI-gate `task gen:check` запускает `task gen` (обе подтаски параллельно), затем валит билд, если `git diff --exit-code -- docs/ gen/ api/openapi/` находит tracked-изменения или `git ls-files --others --exclude-standard -- docs/ gen/ api/openapi/` находит новые untracked-файлы. Это способ поймать забытую регенерацию любого из трёх артефактов.
