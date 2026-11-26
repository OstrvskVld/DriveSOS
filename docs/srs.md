# Software Requirements Specification: DriveSOS

Структуру адаптовано за ISO/IEC/IEEE 29148. Версія 1.0. Значення метрик є цільовими, не виміряними.

## 1. System Context

DriveSOS — платформа розміщення, агрегації, пошуку, платного просування та добору оголошень про продаж автомобілів, з форумом.

**У межах системи:** вебзастосунок, API, сервіси, брокер і сховища даних.
**Поза межами:** Telegram, API зовнішніх платформ, платіжний шлюз (sandbox). Реальні платежі та зберігання даних карток не передбачені.
**Користувачі:** гість, користувач, продавець, модератор, адміністратор.

## 2. Functional Requirements

| ID | Назва | Вимога |
|---|---|---|
| <a id="srs-fun-01"></a>SRS-FUN-01 | Автентифікація | Система повинна автентифікувати користувача за OAuth 2.0 з видачею підписаного JWT. |
| <a id="srs-fun-02"></a>SRS-FUN-02 | Авторизація | Система повинна перевіряти роль користувача (RBAC) перед виконанням захищеної операції. |
| <a id="srs-fun-03"></a>SRS-FUN-03 | Rate Limiting | api-gateway повинен відхиляти запити понад ліміт (Token Bucket, 100 запитів/хв на IP) з кодом 429. |
| <a id="srs-fun-04"></a>SRS-FUN-04 | Оголошення | listing-service повинен забезпечувати створення оголошення продавцем. |
| <a id="srs-fun-05"></a>SRS-FUN-05 | Медіафайли | listing-service повинен зберігати фото оголошення в MinIO. |
| <a id="srs-fun-06"></a>SRS-FUN-06 | Пошук | search-service повинен виконувати повнотекстовий пошук із фасетною фільтрацією. |
| <a id="srs-fun-07"></a>SRS-FUN-07 | Форум | forum-service повинен забезпечувати створення тем. |
| <a id="srs-fun-08"></a>SRS-FUN-08 | Модерація | forum-service повинен надавати модератору обробку скарг. |
| <a id="srs-fun-09"></a>SRS-FUN-09 | Кампанії | promotion-service повинен забезпечувати задання бюджету кампанії. |
| <a id="srs-fun-10"></a>SRS-FUN-10 | Ранжування | search-service повинен поєднувати релевантність із рекламним коефіцієнтом. |
| <a id="srs-fun-11"></a>SRS-FUN-11 | Оплата | promotion-service повинен приймати оплату кампанії через sandbox-шлюз. |
| <a id="srs-fun-12"></a>SRS-FUN-12 | Імпорт | aggregator-service повинен приймати повідомлення з дозволених джерел через Telegram Bot API або офіційні API. |
| <a id="srs-fun-13"></a>SRS-FUN-13 | Нормалізація | aggregator-service повинен перетворювати імпортоване повідомлення на єдину схему оголошення. |
| <a id="srs-fun-14"></a>SRS-FUN-14 | Дедуплікація | aggregator-service повинен виявляти дублікати за VIN, хешем фото та текстовою схожістю. |
| <a id="srs-fun-15"></a>SRS-FUN-15 | Трекінг | tracking-service повинен приймати поведінкові події лише за наявності згоди користувача. |
| <a id="srs-fun-16"></a>SRS-FUN-16 | Рекомендації | recommendation-service повинен формувати персональний перелік гібридним методом. |
| <a id="srs-fun-17"></a>SRS-FUN-17 | Згода | auth-service повинен забезпечувати надання згоди на трекінг. |
| <a id="srs-fun-18"></a>SRS-FUN-18 | Видалення даних | Система повинна видаляти персональні та поведінкові дані користувача за його запитом. |
| <a id="srs-fun-19"></a>SRS-FUN-19 | Ідемпотентність | Кожен споживач Apache Kafka повинен ігнорувати повторно доставлену подію. |
| <a id="srs-fun-20"></a>SRS-FUN-20 | Dead Letter Queue | Споживач повинен передавати подію в DLQ після 5 невдалих спроб обробки. |
| <a id="srs-fun-21"></a>SRS-FUN-21 | Підпис оплати | promotion-service повинен відхиляти webhook платіжного шлюзу з недійсним підписом. |
| <a id="srs-fun-22"></a>SRS-FUN-22 | Секрет webhook | aggregator-service повинен відхиляти webhook Telegram з недійсним secret token. |
| <a id="srs-fun-23"></a>SRS-FUN-23 | Редагування | listing-service повинен забезпечувати редагування оголошення його власником. |
| <a id="srs-fun-24"></a>SRS-FUN-24 | Архівування | listing-service повинен забезпечувати архівування оголошення його власником. |
| <a id="srs-fun-25"></a>SRS-FUN-25 | Коментарі | forum-service повинен забезпечувати створення коментарів. |
| <a id="srs-fun-26"></a>SRS-FUN-26 | Відкликання згоди | auth-service повинен забезпечувати відкликання згоди на трекінг. |
| <a id="srs-fun-27"></a>SRS-FUN-27 | Ліміт показів | promotion-service повинен припиняти покази кампанії після досягнення ліміту. |

## 3. Non-Functional Requirements

| ID | Вимога |
|---|---|
| <a id="srs-nfr-perf-01"></a>SRS-NFR-PERF-01 | Система повинна забезпечувати p95 Latency пошуку ≤ 300 мс при 500 RPS. |
| <a id="srs-nfr-perf-02"></a>SRS-NFR-PERF-02 | Система повинна забезпечувати p95 Latency видачі рекомендацій ≤ 200 мс. |
| <a id="srs-nfr-perf-03"></a>SRS-NFR-PERF-03 | tracking-service повинен приймати ≥ 1000 подій/с без втрати прийнятих подій. |
| <a id="srs-nfr-cons-01"></a>SRS-NFR-CONS-01 | Оголошення повинно з'являтися в пошуку з p95 затримки ≤ 5 с після запису. |
| <a id="srs-nfr-rel-01"></a>SRS-NFR-REL-01 | Повторна доставка події не повинна створювати дубльований результат. |
| <a id="srs-nfr-bus-01"></a>SRS-NFR-BUS-01 | Бюджет кампанії не повинен перевитрачатися при конкурентних показах. |
| <a id="srs-nfr-bus-02"></a>SRS-NFR-BUS-02 | Частка рекламних позицій на сторінці видачі повинна бути ≤ 20 %. |
| <a id="srs-nfr-data-01"></a>SRS-NFR-DATA-01 | Дедуплікація повинна мати precision ≥ 90 %. |
| <a id="srs-nfr-data-02"></a>SRS-NFR-DATA-02 | Дедуплікація повинна мати recall ≥ 90 %. |
| <a id="srs-nfr-sec-01"></a>SRS-NFR-SEC-01 | Зовнішній трафік повинен шифруватися TLS 1.2 або новішою версією. |
| <a id="srs-nfr-sec-02"></a>SRS-NFR-SEC-02 | Паролі повинні зберігатися у вигляді хешів Argon2id. |
| <a id="srs-nfr-sec-03"></a>SRS-NFR-SEC-03 | Строк дії access token повинен бути ≤ 15 хв. |
| <a id="srs-nfr-comp-01"></a>SRS-NFR-COMP-01 | Трекінг повинен припинятися ≤ 60 с після відкликання згоди. |
| <a id="srs-nfr-avail-01"></a>SRS-NFR-AVAIL-01 | Кожен stateless-сервіс повинен мати ≥ 2 репліки. |
| <a id="srs-nfr-avail-02"></a>SRS-NFR-AVAIL-02 | RPO повинен бути ≤ 15 хв. |
| <a id="srs-nfr-avail-03"></a>SRS-NFR-AVAIL-03 | RTO повинен бути ≤ 60 хв. |
| <a id="srs-nfr-maint-01"></a>SRS-NFR-MAINT-01 | Покриття модульними тестами повинно бути ≥ 80 %. |
| <a id="srs-nfr-maint-02"></a>SRS-NFR-MAINT-02 | Контейнерні образи не повинні містити вразливостей рівня CRITICAL. |

## 4. Verification Criteria

| ID вимоги | Метод та інструмент |
|---|---|
| SRS-FUN-01 | інтеграційний тест pytest |
| SRS-FUN-02 | pytest: матриця роль–операція |
| SRS-FUN-03 | k6 |
| SRS-FUN-04 | pytest, Testcontainers |
| SRS-FUN-05 | інтеграційний тест |
| SRS-FUN-06 | pytest, k6 |
| SRS-FUN-07 | pytest |
| SRS-FUN-08 | pytest |
| SRS-FUN-09 | конкурентний тест |
| SRS-FUN-10 | тест на еталонному наборі |
| SRS-FUN-11 | інтеграційний тест webhook |
| SRS-FUN-12 | тест з mock API |
| SRS-FUN-13 | pytest на зразках |
| SRS-FUN-14 | розмічений набір |
| SRS-FUN-15 | інтеграційний тест |
| SRS-FUN-16 | офлайн-оцінка, pytest |
| SRS-FUN-17 | інтеграційний тест |
| SRS-FUN-18 | інтеграційний тест усіх сховищ |
| SRS-FUN-19 | Testcontainers: повторна доставка |
| SRS-FUN-20 | Testcontainers: хибна подія |
| SRS-FUN-21 | pytest: хибний підпис |
| SRS-FUN-22 | pytest: хибний token |
| SRS-FUN-23 | pytest |
| SRS-FUN-24 | pytest |
| SRS-FUN-25 | pytest |
| SRS-FUN-26 | інтеграційний тест |
| SRS-FUN-27 | конкурентний тест |
| SRS-NFR-PERF-01 | k6 (load, spike), Prometheus |
| SRS-NFR-PERF-02 | k6, Prometheus |
| SRS-NFR-PERF-03 | k6, звірка лічильників |
| SRS-NFR-CONS-01 | інтеграційний тест, Prometheus |
| SRS-NFR-REL-01 | Testcontainers |
| SRS-NFR-BUS-01 | конкурентний тест k6, звірка |
| SRS-NFR-BUS-02 | pytest |
| SRS-NFR-DATA-01 | розмічений набір, scikit-learn |
| SRS-NFR-DATA-02 | розмічений набір, scikit-learn |
| SRS-NFR-SEC-01 | перевірка конфігурації TLS |
| SRS-NFR-SEC-02 | unit-тест, SonarQube |
| SRS-NFR-SEC-03 | pytest |
| SRS-NFR-COMP-01 | інтеграційний тест |
| SRS-NFR-AVAIL-01 | перевірка Kubernetes, тест відмови pod |
| SRS-NFR-AVAIL-02 | тест відновлення з backup і WAL |
| SRS-NFR-AVAIL-03 | тест відновлення з вимірюванням часу |
| SRS-NFR-MAINT-01 | pytest-cov, SonarQube |
| SRS-NFR-MAINT-02 | Trivy у GitHub Actions |

## 5. Glossary

| Термін | Визначення |
|---|---|
| Latency | час від надсилання запиту до отримання відповіді |
| p95 | значення, яке не перевищують 95 % вимірювань |
| Eventual Consistency | узгодженість моделей читання із записом досягається із затримкою |
| Transactional Outbox | запис події в одній транзакції з даними та подальша публікація в брокер |
| Idempotent Consumer | споживач, повторна обробка події яким не змінює результат |
| CQRS | розділення моделей запису та читання |
| JWT | підписаний токен із твердженнями про суб'єкта (RFC 7519) |
