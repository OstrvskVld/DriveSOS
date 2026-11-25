# F2-diploma-project

Кваліфікаційний проєкт бакалавра зі спеціальності **F2 Інженерія програмного забезпечення**.

**Автор:** Островський Владислав Олександрович
**Науковий керівник:** Когут Олег Васильович
**Навчальний рік:** 2026/2027

## Тема проєкту

**UA:** Проєктування та розробка хмароорієнтованої подієво-керованої платформи автомобільного маркетплейсу з форумом, платним просуванням оголошень, агрегацією зовнішніх джерел та рекомендаційною системою на основі поведінкових подій.

**EN:** Design and Development of a Cloud-Native Event-Driven Automotive Marketplace Platform with a Forum, Paid Listing Promotion, External Source Aggregation, and a Behavior-Based Recommendation System.

## Короткий опис

Розподілена мікросервісна платформа з подієво-керованою архітектурою (Event-Driven Architecture, CQRS), яка поєднує:

- каталог оголошень із повнотекстовим пошуком;
- форум із модерацією;
- платне просування оголошень із керованим ранжуванням видачі;
- агрегацію оголошень із Telegram та зовнішніх джерел;
- трекінг цільових дій користувача (перегляди, лайки, контакти з продавцем);
- рекомендаційну систему на основі поведінкових подій.

## Попередній технологічний стек

| Категорія | Технології |
|---|---|
| Мови | Python, TypeScript |
| Патерни | Event-Driven, CQRS, Transactional Outbox, Idempotent Consumer |
| Фреймворки | FastAPI, React (Next.js) |
| Брокер | Apache Kafka |
| Бази даних | PostgreSQL, Redis, Elasticsearch, ClickHouse |
| DevOps | Docker, Kubernetes, GitHub Actions, SonarQube, Trivy, k6 |
| Observability | OpenTelemetry, Prometheus, Grafana, Jaeger |

## Структура репозиторію

```
F2-diploma-project/
├── README.md        # опис проєкту
├── docs/            # документація (Documentation as Code)
│   └── proposal.md  # Thesis Proposal
└── src/             # вихідний код (буде додано на наступних етапах)
```

## Документація

- [Thesis Proposal](docs/proposal.md): назва, актуальність, архітектура, ключові функції, технологічний стек.

## Робота з репозиторієм

Коміти оформлюються за угодою [Conventional Commits](https://www.conventionalcommits.org/):

| Префікс | Призначення |
|---|---|
| `feat:` | нова функціональність |
| `fix:` | виправлення, у тому числі зауважень керівника |
| `docs:` | зміни в документації |
| `chore:` | службові зміни (структура, конфігурація) |

Приклад: `docs: add thesis proposal verified by LLM audit`.

## Статус

| Етап | Статус |
|---|---|
| Ініціалізація репозиторію та Docs-as-Code контуру | Done |
| Thesis Proposal | Done |
| Верифікація керівником | In Progress |
| System Design (C4, ADR) | To Do |
| Реалізація | To Do |
| DevSecOps, хмарне розгортання, Observability | To Do |

## Ліцензія

Проєкт розробляється в навчальних цілях.
