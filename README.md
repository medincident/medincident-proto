# medincident-proto

Единый источник правды для всех protobuf-контрактов системы medincident.

В репозитории живут два независимых дерева:

- **`api/proto/medincident/`** — доменные контракты первого порядка. События, идентификаторы агрегатов и shared value-типы, которые использует CQRS command/query. Они описывают *что произошло в домене*.
- **`api/proto/zitadel/`** — 1:1 зеркала payload'ов Zitadel Actions v2 webhook'ов, владеет ими gateway-сервис `medincident-zitadel-actions`. Они описывают *что Zitadel рассказал нам* и намеренно сохраняют форму самого Zitadel, чтобы консьюмеры, которым важны Zitadel-нативные метаданные (instance id, sequence, resource owner), могли читать их без трансляции.

Два дерева никогда не ссылаются друг на друга. Изменения формы Zitadel webhook'ов не должны протекать в доменные контракты, а рефакторинг домена не должен трогать passthrough-зеркала.

## Документация

Reference-документация по каждому proto-пакету живёт в [`docs/`](docs/) и кодогенерируется из самих `.proto` файлов (см. [CONTRIBUTING.md](CONTRIBUTING.md#генерация-документации)). Один файл на пакет, структура повторяет `api/proto/`:

- [`docs/medincident/events/v1/`](docs/medincident/events/v1/README.md) — доменный envelope
- [`docs/medincident/orgstructure/v1/`](docs/medincident/orgstructure/v1/README.md) — события агрегата organization
- [`docs/medincident/shared/geo/v1/`](docs/medincident/shared/geo/v1/README.md) — общие гео value-типы
- [`docs/zitadel/events/v1/`](docs/zitadel/events/v1/README.md) — Zitadel webhook envelope
- [`docs/zitadel/sessions/v1/`](docs/zitadel/sessions/v1/README.md) — Zitadel session events + types
- [`docs/zitadel/users/v1/`](docs/zitadel/users/v1/README.md) — Zitadel user events + types

Доки можно читать прямо на GitHub без установки buf/protoc.

## Раскладка

```
api/proto/
├── medincident/
│   ├── events/v1/            — доменный event envelope
│   ├── orgstructure/v1/      — события агрегата organization
│   └── shared/geo/v1/        — общие geo value-типы
└── zitadel/
    ├── events/v1/            — Zitadel webhook envelope
    ├── sessions/v1/          — Zitadel session events + types
    └── users/v1/             — Zitadel user events + types
```

Каждая leaf-директория соответствует одному proto-пакету.

## Генерация Go-кода в downstream-сервисах

Каждый консьюмер держит свой `buf.gen.yaml` и генерит Go через `buf generate`. Есть два поддерживаемых стиля input'а:

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

Консьюмерам следует фильтровать `paths:` только до нужного поддерева — не надо тянуть `zitadel/`, если сервис работает только с доменными событиями. Живой пример `git_repo:` формы — `medincident-zitadel-actions/buf.gen.yaml`.

## Работа в репозитории

См. [CONTRIBUTING.md](CONTRIBUTING.md) — prereqs, настройка pre-commit, правила редактирования proto (версионирование, отслеживание breaking changes, раскладка пакетов).

Быстрая справка:

```bash
task fmt           # отформатировать все proto
task lint          # buf lint + buf breaking против origin/main
task gen:docs      # перегенерировать Markdown-доки в docs/
task gen           # зонтичная генерация (сейчас: gen:docs)
```
