[English](./README.md) | [繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [ລາວ](./README-lo.md)

# API Безбедносна контролна листа
Безбедносна контролна листа од најважните безбедносни контрамерки при дизајнирање, тестирање и пуштање во употреба на вашето API.


---

## Автентикација
- [ ] Не користете `Basic Auth` Користете стандардна автентикација (п.р. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Не измислувајте топла вода за `Authentication`, `generation token`, `password storage`. Користете ги стандардите.
- [ ] Користете `Max Retry` и затворските функции во Login.
- [ ] Користете енкрипција на сите чувствителни податоци.

### JWT (JSON Web Token)
- [ ] Користете случајно генериран и комплициран клуч (`JWT Secret`) за да направите што можно потешко погодување на токенот со испробување на секоја можна комбинација
- [ ] Don't extract the algorithm from the payload. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] Направете токенот да истече (`TTL`, `RTTL`) што е можно побрзо.
- [ ] Не чувајте чувствителни податоци во JWR payload, може да се декодира [лесно](https://jwt.io/#debugger-io).

### OAuth
- [ ] Секогаш проверувајте ја `redirect_uri` од страна на серверот за да дозволите само бела листа на адреси.
- [ ] Секогаш обидувајте се да разменувате за код, а не токени (не дозволувајте `response_type = token`).
- [ ] Користете `state` параметар со случаен хаш за да се спречи CSRF на процесот на автентикација на OAuth
- [ ] Дефинирајте го основниот опсег и проверете ги параметрите на опсегот за секоја апликација.

## Пристап
- [ ] Ограничете ги барањата (забавување) за да избегнете напади DDoS / brute-force.
- [ ] Користете HTTPS на страната на серверот за да избегнете MITM (Man In The Middle Attack).
- [ ] Користете `HSTS` насловот со SSL за да избегнете SSL Strip напад.

## Влез
- [ ] Користете ја соодветната HTTP-метод според операцијата: "GET (read)", "POST (создади)", "PUT / PATCH (замени / ажурирај)" и "DELETE (за бришење на запис) 405 Метод не е дозволено` ако бараниот метод не е соодветен за бараниот ресурс.
- [ ] Потврдете `content-type` на барање Accept header (Content Negotiation) за да го дозволите само вашиот поддржан формат (на пр.`application/xml`, `application/json`, итн) И да одговори со 406 Not Acceptable` одговор ако не се совпаѓа.
- [ ] Потврдете ги `content-type` на објавените податоци што ги прифаќате (на пр., `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, итн.).
- [ ] Потврдете го корисничкиот влез за да избегнете вообичаени слабости (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, итн).
- [ ] Не користете чувствителни податоци(`credentials`, `Passwords`, `security tokens`, или `API keys`) во URL-то, но користете стандарден заглавие за авторизација.
- [ ] Користете API Gateway-услуга за да овозможите кеширање, политики за ограничување на тарифите (пр. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) и динамички да ги распоредите ресурсите за API-то.

## Processing
- [ ] Проверете дали сите крајните точки се заштитени зад автентичност за да се избегне скршен процес на автентикација.
- [ ] Треба да се избегнува идентификација на сопствени ресурси на сопственикот. Користете `/ me / orders` наместо` / user / 654321 / orders`.
- [ ] Не автоматско зголемување на ID-ите. Наместо тоа, употребете `UUID`.
- [ ] Ако ги анализирате XML-датотеките, проверете дали парсирањето на ентитетот не е овозможено за да се избегне `XXE` (напад на надворешен ентитет на XML).
- [ ] Ако анализирате XML-датотеки, проверете дали проширувањето на ентитетот не е овозможено за да се избегне `Billion Laughs / XML бомба` преку експоненцијален напад на експанзија на ентитетот.
- [ ] Користете CDN за закачување на фајлови.
- [ ] Ако се занимавате со огромни количини на податоци, користете Workers and Queues за да процесирате што е можно повеќе во позадина и да го вратите одговорот брзо за да избегнете блокирање на HTTP
- [ ] Не заборавајте да го исклучите режимот DEBUG.

## Излез
- [ ] Праќај `X-Content-Type-Options: nosniff` хедер.
- [ ] Праќај `X-Frame-Options: deny` хедер.
- [ ] Праќај `Content-Security-Policy: default-src 'none'` хедер.
- [ ] Отстранете ги хедерите кој издаваат отповеќе податоци - `X-Powered-By`, `Server`, `X-AspNet-Version` итн.
- [ ] Присилувај `content-type` " за твојот одговор, ако се вратиш `application/json` тогаш твојот одговор `content-type` е `application/json`.
- [ ] Не враќајте чувствителни податоци како `credentials`, `Passwords`, `security tokens`.
- [ ] Врати го соодветниот код за статусот според завршената операција. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, итн).

## CI & CD
- [ ] Ревизија на вашиот дизајн и имплементација со покриеност тестови за единица / интеграција.
- [ ] Користете процес на прегледување на кодот и не дозволувајте самоодобрување
- [ ] Осигурајте се дека сите компоненти на вашите услуги се статички скенирани од AV-софтверот пред да се изврши притисок за производство, вклучувајќи библиотеки на продавачи и други зависности.
- [ ] Дизајн на rollback за во продукција


---

## Исто така види:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - A collection of useful resources for building RESTful HTTP+JSON APIs.


---

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
