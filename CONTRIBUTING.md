# Contributing

## Требования

- [Go 1.26+](https://go.dev/dl/) — `buf`, `protoc-gen-doc` и `golangci-lint` подтянуты через `go tool`, так что для работы со схемами нужен только Go toolchain.
- [Task](https://taskfile.dev/installation/) — task runner, обёртка над всеми частыми командами.
- [pre-commit](https://pre-commit.com/#install) — git hook manager; запускает формат, lint, breaking-change и регенерацию доков на каждое изменение `.proto`.

Ни Docker, ни C-toolchain, ни отдельная установка protoc не нужны — всё идёт через `go tool buf`.

## Старт

Склонируй репу и поставь pre-commit хуки:

```bash
git clone https://github.com/medincident/medincident-proto.git
cd medincident-proto
pre-commit install
```

Проверь тулчейн:

```bash
task lint       # style + breaking-change check
task fmt:check  # проверка форматирования (dry-run)
task gen:docs   # перегенерация per-package доков в docs/
```

Все три должны завершиться с кодом 0 на чистом чекауте.

## Pre-commit хуки

Конфигурация в [`.pre-commit-config.yaml`](.pre-commit-config.yaml). При изменении любого `*.proto` файла запускаются:

| Хук | Что делает |
|---|---|
| `task-fmt-check` | `task fmt:check` — падает, если какой-то proto не отформатирован. Чинится через `task fmt`. |
| `task-lint` | `task lint` — buf style lint + `buf breaking` против `origin/main`. |
| `task-gen-docs` | `task gen:docs` — перегенерирует Markdown-доки под [`docs/`](docs/). Если какой-то файл доки изменился, pre-commit валит коммит, ты пере-стейджишь сгенерированные файлы и коммитишь заново. |

Общие hygiene-хуки (trailing whitespace, EOF newline, YAML validation, large-file guard, merge-conflict markers) запускаются на каждый коммит независимо от типа файла.

Запуск всех хуков руками:

```bash
pre-commit run --all-files
```

## Частые команды

```bash
task fmt           # отформатировать все proto in place
task fmt:check     # проверка формата (dry-run, без модификаций)
task lint          # buf lint + buf breaking (против origin/main)
task lint:breaking # только breaking-change check
task gen           # запустить все кодгенераторы (сейчас: gen:docs)
task gen:docs      # перегенерировать Markdown-доки в docs/
```

`task gen` — зонтичная точка входа для всех кодгенов. Сегодня дёргает только `gen:docs`; любой будущий генератор (validation rules, OpenAPI, client stubs) добавится как новая `gen:*` таска и подцепится под `gen`.

## Генерация документации

`task gen:docs` использует [`protoc-gen-doc`](https://github.com/pseudomuto/protoc-gen-doc) (вендорнут как `go tool` зависимость, см. `go.mod`) и итерируется по каждой leaf-директории с `.proto` файлами. Для каждой генерится один Markdown-файл `docs/<tree>/<domain>/v1/README.md`. Сгенерированные доки закоммичены в репу — так их видно прямо на GitHub, без установки buf.

Выход пост-процессится через `awk`: срезается trailing whitespace и схлопываются хвостовые пустые строки, чтобы регенерация была идемпотентной под pre-commit. Если хук `task-gen-docs` валит твой коммит — просто пере-стейдж изменённые файлы в `docs/` и коммить повторно.

## Редактирование протоколов

### Добавить поле в существующее сообщение

1. Отредактируй `.proto` файл. Используй следующий свободный номер поля — никогда не переиспользуй старый.
2. Запусти `task fmt`, чтобы нормализовать форматирование.
3. Запусти `task lint`, чтобы проверить style и что изменение wire-compatible.
4. Запусти `task gen:docs`, чтобы обновить Markdown-доки.
5. Коммить. Pre-commit хуки перепрогонят всё перечисленное выше.

### Добавить новый пакет

1. Создай директорию под `api/proto/<tree>/<domain>/v1/`, где `<tree>` — это либо `medincident` (first-party доменные контракты), либо `zitadel` (passthrough для Zitadel Actions v2).
2. Добавь `.proto` файл с `package <tree>.<domain>.v1;`, соответствующим пути директории. Это навязывается `buf lint`.
3. `task gen:docs` подхватит новый пакет автоматически — генератор итерируется по каждой leaf-директории с `.proto` файлами.

### Версионирование

- Breaking changes бампят версию пакета: `v1` → `v2`. Старый пакет остаётся живым до тех пор, пока все консьюмеры не переедут.
- `task lint` запускает `buf breaking` против `origin/main` и уронит коммит, который удаляет или ренамит поле на существующем пакете. Если breaking change реально нужен — бампай версию пакета в новую директорию, а не правь существующую.

### Два дерева, две зоны ответственности

- **`api/proto/medincident/`** — first-party доменные контракты (события, агрегаты, shared value-типы). Это CQRS-доменные события, которые emitит `medincident-command-service`.
- **`api/proto/zitadel/`** — 1:1 passthrough зеркала payload'ов Zitadel Actions v2 webhook'ов. Владеет ими `medincident-zitadel-actions`, который принимает Zitadel webhook'и и ре-публикует их в NATS JetStream, используя эти сообщения как payload-тип.

Никаких cross-reference между деревьями. Сообщение `medincident.*` не должно импортировать `zitadel.*` и наоборот — смысл разделения в том, чтобы Zitadel мог менять форму своих webhook'ов, не ломая доменные контракты.

## Downstream-консьюмеры

Downstream-сервисы генерят Go-код из этой репы через `buf.gen.yaml`. Основные паттерны:

**Локальная разработка на feature-ветке** — указывай `buf.gen.yaml` на sibling рабочую копию через `directory:` input:

```yaml
inputs:
  - directory: ../medincident-proto/api/proto
    paths:
      - medincident   # или: zitadel
```

**Стабильное потребление** — как только `medincident-proto` выкатывает тег, переключайся на `git_repo:` input, запиненный на этот тег. См. `medincident-zitadel-actions/buf.gen.yaml` — канонический пример этой формы.

В обоих случаях фильтруй `paths:` только до нужного поддерева. Не тяни `zitadel/`, если сервису нужны только доменные события.
