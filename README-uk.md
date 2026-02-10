[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md)

# Контрольний список безпеки API

Контрольний список найбільш важливих контрзаходів безпеки при розробці, тестуванні та випуску вашого API.

---

## Аутентифікація

- [ ] Не використовуйте `Basic Auth` Використовуйте стандартну перевірку справжності.
- [ ] Не "винаходьте колесо" в `аутентіфікаціі`, `створенні токенів`, `зберіганні паролей`. Використовуйте стандарти.
- [ ] Використовуйте `Max Retry` і функції jail в Login.
- [ ] Користуйтеся шифруванням для всіх конфіденційних даних.

## Доступ

- [ ] Обмежте запити (Throttling), щоб уникнути DDoS атак / грубої сили (Brute Force).
- [ ] Використовуйте HTTPS на стороні сервера, щоб уникнути MITM (Man In The Middle Attack / Атака посередника).
- [ ] Використовуйте заголовок `HSTS` (HTTP Strict Transport Security) з SSL, щоб уникнути атаки SSL Strip (перехоплення SSL з'єднань).
- [ ] Вимкніть списки каталогів.
- [ ] Для приватних API, дозвольте доступ лише з IP-адрес/хостів із білого списку.

## Авторизація

### OAuth

- [ ] Завжди перевіряйте `redirect_uri` на стороні сервера, щоб дозволяти тільки URL-адреси з білими списками.
- [ ] Завжди намагайтеся обмінювати код, а не токени (не дозволяти `response_type = token`).
- [ ] Використовуйте параметр `стану` з випадковим хешем, щоб запобігти CSRF в процесі аутентифікації OAuth.
- [ ] Визначте область за замовчуванням і перевірте параметри області для кожної програми.

## Введення

- [ ] Використовуйте відповідний HTTP-метод відповідно до операції: `GET (читання),` POST (створення) `,` PUT / PATCH (заміна / оновлення) `і` DELETE (для видалення запису) `, а також дайте відповідь` 405 Method Not Allowed`, якщо запитаний метод не підходить для запитуваного ресурсу.
- [ ] Підтвердіть `тип вмісту` за запитом "Прийняти заголовок" (Консолідація контенту), щоб дозволити тільки підтримуваний формат (наприклад: `application/xml`, `application/json` і т.д.) І відповідайте з неприпустимим відповіддю 406, якщо він не узгоджений.
- [ ] Перевіряйте вміст опублікованих даних `типу контенту` в міру їх прийняття (наприклад,` application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` і т.д.).
- [ ] Перевірте користувальницьке введення щоб уникнути поширених вразливостей (наприклад: `XSS`, `SQL-ін'єкцій`, `віддалене виконання коду` і т.д.).
- [ ] Не використовуйте конфіденційні дані (`облікові дані`, `паролі`, `маркери безпеки` або `ключі API`) в URL-адресі, але використовуйте стандартний заголовок авторизації.
- [ ] Використовуйте лише шифрування на стороні сервера.
- [ ] Використовуйте службу шлюзу API, щоб активувати кешування, обмеження швидкості, спайк-арешт і динамічне розгортання ресурсів API.

## Обробка

- [ ] Перевірте, чи захищені всі кінцеві точки за аутентифікацією, щоб не порушити процедуру перевірки автентичності.
- [ ] Слід уникати ідентифікатора користувача власного ресурсу. Використовуйте `/me/orders` замість `/user/654321/orders`.
- [ ] Не використовуйте автоінкремент для ID. Замість цього використовуйте `UUID`.
- [ ] Якщо ви розбираєте XML-файли, переконайтеся, що синтаксичний аналіз сутностей не включений, щоб уникнути `атаки на зовнішній об'єкт XML` (XML external entity).
- [ ] Якщо ви розбираєте XML-файли, переконайтеся, що розширення суті не включено, щоб уникнути `Billion Laughs / XML bomb` за допомогою експоненційної атаки розширення сутностей.
- [ ] Використовуйте CDN для завантаження файлів.
- [ ] Якщо ви маєте справу з величезною кількістю даних, використовуйте Workers and Queues, щоб обробляти якомога більше в фоновому режимі і швидко повертати відповідь, щоб уникнути блокування HTTP.
- [ ] Не забудьте вимкнути режим DEBUG.
- [ ] Використовуйте невиконувані stack якщо вони доступні.

## Виведення

- [ ] Надсилайте заголовок `X-Content-Type-Options: nosniff`.
- [ ] Надсилайте заголовок `X-Frame-Options: deny`.
- [ ] Надсилайте заголовок `Content-Security-Policy: default-src 'none'`.
- [ ] Видаліть заголовки відбитків пальців - `X-Powered-By`,` Server`, `X-AspNet-Version` і т.д.
- [ ] Примусите `тип вмісту` для вашої відповіді, якщо ви повернете` application/json`, тоді ваш тип вмісту відповіді буде `application/json`.
- [ ] Do not return overly specific error messages to the client that could reveal implementation details, use generic messages instead, and log detailed information only on the server side.
- [ ] Не повертайте конфіденційні дані, такі як `облікові дані`, `паролі`, `токени безпеки`.
- [ ] Завжди повертайте код стану відповідно до завершеною роботою. (Наприклад: `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` і т.д.).

## Безперервна інтеграція і Безперервне постачання (CI & CD)

- [ ] Аудит вашого дизайну і реалізації з охопленням модулів / інтеграційних тестів.
- [ ] Використовуйте процес перевірки коду і ігноруйте самоокупність.
- [ ] Переконайтеся, що всі компоненти ваших служб статично скануються за допомогою антивірусів перед відправкою на виробництво, включаючи бібліотеки постачальників та інші залежності.
- [ ] Постійно запускайте тести безпеки (статичний/динамічний аналіз) вашого коду.
- [ ] Перевірте свої залежності (як програмне забезпечення, так і ОС) на відомі вразливості.
- [ ] Створіть рішення відкату для розгортання.

## Моніторинг

- [ ] Використовуйте централізований вхід для всіх служб і компонентів.
- [ ] Використовуйте агентів для моніторингу всього трафіку, помилок, запитів і відповідей.
- [ ] Використовуйте сповіщення для SMS, Slack, Email, Telegram, Kibana, Cloudwatch, тощо.
- [ ] Переконайтеся, що ви не реєструєте жодних конфіденційних даних, таких як кредитні картки, паролі, PIN-коди, тощо.
- [ ] Використовуйте систему IDS та/або IPS для моніторингу запитів і екземплярів API.

---

## Дивись також:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Набір корисних ресурсів для створення RESTful HTTP+JSON API.
- You don't need JWT, just use a randomly generated API key. If you need asymmetric encryption or tamper prevention, [here are some alternatives to JWT](https://kevin.burke.dev/kevin/things-to-use-instead-of-jwt/).

---

## API Security Best Practices (Advanced)

### Rate Limiting & Abuse Prevention
- [ ] Implement sliding window rate limiting per API key and IP.
- [ ] Use exponential backoff for repeated failed authentication attempts.
- [ ] Implement CAPTCHA or proof-of-work challenges after suspicious activity.
- [ ] Monitor and alert on unusual API usage patterns (time, volume, endpoints).

### GraphQL-Specific Security
- [ ] Disable introspection in production environments.
- [ ] Implement query depth limiting to prevent nested query attacks.
- [ ] Use query cost analysis to prevent resource exhaustion.
- [ ] Whitelist allowed queries in production when possible.

### Secrets Management
- [ ] Rotate API keys and secrets on a regular schedule.
- [ ] Use hardware security modules (HSM) for signing operations.
- [ ] Implement secret scanning in CI/CD pipelines.
- [ ] Never commit secrets to version control - use environment variables or secret managers.

### Zero Trust Architecture
- [ ] Implement mutual TLS (mTLS) for service-to-service communication.
- [ ] Validate all requests even from internal services.
- [ ] Use short-lived tokens with automatic refresh.
- [ ] Implement request signing for sensitive operations.

---

# Вклад

Не соромтеся робити внесок, відкриваючи цей репозиторій, вносячи деякі зміни і відправляючи `Pull Requests`. З будь-яких питань напишіть нам лист за адресою `team@shieldfy.io`.
