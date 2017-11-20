[繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Македонски](.README-mk.md) 

# API Безбедносна контролна листа
Безбедносна контролна листа од најважните безбедносни контрамерки при дизајнирање, тестирање и пуштање во употреба на вашето API.
---

## Автентикација
- [ ] Не користете `Basic Auth` Користете стандардна автентикација (п.р. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Не измислувајте топла вода за  `Authentication`,` generation token `, `password storage`. Користете ги стандардите.
- [ ] Користете `Max Retry` и затворските функции во Login.
- [ ] Користете енкрипција на сите чувствителни податоци.

### JWT (JSON Web Token)
- [ ] Користете случајно генериран и комплициран клуч (`JWT Secret`) за да направите што можно потешко погодување на токенот со испробување на секоја можна комбинација
- [ ] Don't extract the algorithm from the payload. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] Направете токенот да истече (`TTL`, `RTTL`) што е можно побрзо .
- [ ] Не чувајте чувствителни податоци во JWR payload, може да се декодира [лесно](https://jwt.io/#debugger-io).

### OAuth
- [ ] Секогаш проверувајте ја `redirect_uri` од страна на серверот за да дозволите само бела листа на адреси.
- [ ]Секогаш обидувајте се да разменувате за код, а не токени (не дозволувајте `response_type = token`).
- [ ] Користете го параметрот `state` со случаен хаш за да го спречите CSRF за процесот на автентикација на OAuth.
- [ ] Дефинирајте го основниот опсег и проверете ги параметрите на опсегот за секоја апликација.

## Пристап
- [ ] Ограничете ги барањата (забавување) за да избегнете напади DDoS / brute-force.
- [ ] Користете HTTPS на страната на серверот за да избегнете MITM (Man In The Middle Attack).
- [ ] Користете `HSTS` насловот со SSL за да избегнете SSL Strip напад.

## Влез
- [ ] Користете ја соодветната HTTP-метод според операцијата: "GET (read)", "POST (создади)", "PUT / PATCH (замени / ажурирај)" и "DELETE (за бришење на запис) 405 Метод не е дозволено` ако бараниот метод не е соодветен за бараниот ресурс.
- [ ] Потврдете `content-type` на барање Accept header (Content Negotiation) за да го дозволите само вашиот поддржан формат (на пр.`application/xml`, `application/json`, etc) И да одговори со 406 Not Acceptable` одговор ако не се совпаѓа.
- [ ] Потврдете ги  `content-type` на објавените податоци што ги прифаќате (на пр., `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, итн.).
- [ ] Потврдете го корисничкиот влез за да избегнете вообичаени слабости (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc).
- [ ] Не користете чувствителни податоци(`credentials`, `Passwords`, `security tokens`, или `API keys`) во URL-то, но користете стандарден заглавие за авторизација.
- [ ] Користете API Gateway-услуга за да овозможите кеширање, политики за ограничување на тарифите (пр. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) и динамички да ги распоредите ресурсите за API-то.

## Processing
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource ID should be avoided. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] Don't auto-increment IDs. Use `UUID` instead.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use a CDN for file uploads.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
- [ ] Do not forget to turn the DEBUG mode OFF.

## Output
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `Passwords`, `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc).

## CI & CD
- [ ] Audit your design and implementation with unit/integration tests coverage.
- [ ] Use a code review process and disregard self-approval.
- [ ] Ensure that all components of your services are statically scanned by AV software before push to production, including vendor libraries and other dependencies.
- [ ] Design a rollback solution for deployments.


---

## See also:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - A collection of useful resources for building RESTful HTTP+JSON APIs.


---

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
