[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API Безбедносна контролна листа

Безбедносна контролна листа од најважните безбедносни контрамерки при дизајнирање, тестирање и пуштање во употреба на вашето API.

---

## Автентикација

- [ ] Не користете `Basic Auth` Користете стандардна автентикација.
- [ ] Не измислувајте топла вода за `Authentication`, `generation token`, `password storage`. Користете ги стандардите.
- [ ] Користете `Max Retry` и затворските функции во Login.
- [ ] Користете енкрипција на сите чувствителни податоци.

## Пристап

- [ ] Ограничете ги барањата (забавување) за да избегнете напади DDoS / brute-force.
- [ ] Користете HTTPS на страната на серверот за да избегнете MITM (Man In The Middle Attack).
- [ ] Користете `HSTS` насловот со SSL за да избегнете SSL Strip напад.
- [ ] Исклучете ги списоците на директориуми.
- [ ] За приватни API, дозволете пристап само од IP-а/домаќини на белата листа.

## Овластување

### OAuth

- [ ] Секогаш проверувајте ја `redirect_uri` од страна на серверот за да дозволите само бела листа на адреси.
- [ ] Секогаш обидувајте се да разменувате за код, а не токени (не дозволувајте `response_type = token`).
- [ ] Користете `state` параметар со случаен хаш за да се спречи CSRF на процесот на автентикација на OAuth.
- [ ] Дефинирајте го основниот опсег и проверете ги параметрите на опсегот за секоја апликација.

## Влез

- [ ] Користете ја соодветната HTTP-метод според операцијата: "GET (read)", "POST (создади)", "PUT / PATCH (замени / ажурирај)" и "DELETE (за бришење на запис) 405 Метод не е дозволено` ако бараниот метод не е соодветен за бараниот ресурс.
- [ ] Потврдете `content-type` на барање Accept header (Content Negotiation) за да го дозволите само вашиот поддржан формат (на пр.`application/xml`, `application/json`, итн) И да одговори со 406 Not Acceptable` одговор ако не се совпаѓа.
- [ ] Потврдете ги `content-type` на објавените податоци што ги прифаќате (на пр., `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, итн.).
- [ ] Потврдете го корисничкиот влез за да избегнете вообичаени слабости (п.р. `XSS`, `SQL-Injection`, `Remote Code Execution`, итн).
- [ ] Не користете чувствителни податоци(`credentials`, `Passwords`, `security tokens`, или `API keys`) во URL-то, но користете стандарден заглавие за авторизација.
- [ ] Користете само шифрирање од страна на серверот.
- [ ] Користете API Gateway-услуга за да овозможите кеширање, политики за ограничување на тарифите (пр. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) и динамички да ги распоредите ресурсите за API-то.

## Обработка

- [ ] Проверете дали сите крајните точки се заштитени зад автентичност за да се избегне скршен процес на автентикација.
- [ ] Треба да се избегнува идентификација на сопствени ресурси на сопственикот. Користете `/ me / orders` наместо` / user / 654321 / orders`.
- [ ] Не автоматско зголемување на ID-ите. Наместо тоа, употребете `UUID`.
- [ ] Ако ги анализирате XML-датотеките, проверете дали парсирањето на ентитетот не е овозможено за да се избегне `XXE` (напад на надворешен ентитет на XML).
- [ ] Ако анализирате XML-датотеки, проверете дали проширувањето на ентитетот не е овозможено за да се избегне `Billion Laughs / XML бомба` преку експоненцијален напад на експанзија на ентитетот.
- [ ] Користете CDN за закачување на фајлови.
- [ ] Ако се занимавате со огромни количини на податоци, користете Workers and Queues за да процесирате што е можно повеќе во позадина и да го вратите одговорот брзо за да избегнете блокирање на HTTP.
- [ ] Не заборавајте да го исклучите режимот DEBUG.
- [ ] Користете неизвршни stack кога е достапно.

## Излез

- [ ] Праќај `X-Content-Type-Options: nosniff` хедер.
- [ ] Праќај `X-Frame-Options: deny` хедер.
- [ ] Праќај `Content-Security-Policy: default-src 'none'` хедер.
- [ ] Отстранете ги хедерите кој издаваат отповеќе податоци - `X-Powered-By`, `Server`, `X-AspNet-Version` итн.
- [ ] Присилувај `content-type` " за твојот одговор, ако се вратиш `application/json` тогаш твојот одговор `content-type` е `application/json`.
- [ ] Do not return overly specific error messages to the client that could reveal implementation details, use generic messages instead, and log detailed information only on the server side.
- [ ] Не враќајте чувствителни податоци како `credentials`, `Passwords`, `security tokens`.
- [ ] Врати го соодветниот код за статусот според завршената операција. (п.р. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, итн).

## CI & CD

- [ ] Ревизија на вашиот дизајн и имплементација со покриеност тестови за единица / интеграција.
- [ ] Користете процес на прегледување на кодот и не дозволувајте самоодобрување.
- [ ] Осигурајте се дека сите компоненти на вашите услуги се статички скенирани од AV-софтверот пред да се изврши притисок за производство, вклучувајќи библиотеки на продавачи и други зависности.
- [ ] Континуирано извршувајте безбедносни тестови (статичка/динамичка анализа) на вашиот код.
- [ ] Проверете ги вашите зависности (и софтвер и ОС) за познати пропусти.
- [ ] Дизајн на rollback за во продукција.

## Мониторинг

- [ ] Користете централизирани логин за сите услуги и компоненти.
- [ ] Користете агенти за следење на целиот сообраќај, грешки, барања и одговори.
- [ ] Користете предупредувања за SMS, Slack, Е-пошта, Telegram, Kibana, Cloudwatch, итн.
- [ ] Осигурете се дека не внесувате чувствителни податоци како кредитни картички, лозинки, PIN-кодови, итн.
- [ ] Користете IDS и/или IPS систем за следење на вашите барања и примери на API.

---

## Исто така види:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - A collection of useful resources for building RESTful HTTP+JSON APIs.
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

# Придонес

Слободно можете да придонесете со forking на ова repository, правење некои промени и поднесување pull request. За какви било прашања испратете ни е-пошта на `team@shieldfy.io`.
