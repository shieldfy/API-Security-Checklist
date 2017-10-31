[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# API Security Checklist
Checklist met de belangrijkste tegenmaatregelen bij het ontwerpen, testen en uitbrengen van een API.


---

## Authenticatie
- [ ] Gebruik geen `Basic Auth` Gebruik industrie standaarden (v.b. JWT, OAuth).
- [ ] Vind het wiel niet opnieuw uit voor `Authenticatie`, `Genereren van Tokens` en `Opslaan van Wachtwoorden`. Gebruik de standaarden.
- [ ] Gebruik `Max Retry` en Jail features in de login.
- [ ] Encrypt alle gevoelige data.

### JWT (JSON Web Token)
- [ ] Gebruik random ingewikkelde keys (`JWT Secret`) om brute forcing lastiger te maken.
- [ ] Haal het algoritme niet uit de payload. Dwing het algoritme af in de backend (`HS256` of `RS256`).
- [ ] Zet de token vervaltijd (`TTL`, `RTTL`) zo kort mogelijk.
- [ ] Sla geen gevoelige data op in de JWT payload, deze is [makkelijk](https://jwt.io/#debugger-io) te decoderen.

### OAuth
- [ ] Valideer **ALTIJD** de `redirect_uri` op de server om alleen toegestane URL te accepteren.
- [ ] Probeer altijd een exchange voor code, niet voor tokens (sta `response_type=token` niet toe).
- [ ] Gebruik de `state` parameter met een random hash om CSRF op een OAuth authentication process te voorkomen.
- [ ] Definieer een standaard scope, en valideer deze scope parameter voor elke applicatie.

## Toegang
- [ ] Limiteer het aantal requests om DDoS en/of Bruteforce aanvallen te ontkrachten.
- [ ] Gebruik HTTPS aan de server zijde om MITM (Man In The Middle Attacks) tegen te gaan.
- [ ] Gebruik de `HSTS` header i.c.m SSL om een SSL Strip attack te ontkrachten.

## Invoer
- [ ] Gebruik de correcte HTTP methode voor de operatie, `GET (lezen)`, `POST (schrijven)`, `PUT (vervangen/updaten)` and `DELETE (verwijderen)`.
- [ ] Valideer de `content-type` header bij een request Accept header (Content Negotiation) om alleen de ondersteunde formaten toe te staan (e.g. `application/xml`, `application/json` ... etc) en stuur een `406 Not Acceptable` response als de `content-type` niet ondersteund is.
- [ ] Valideer de `content-type` header van gestuurde data (e.g. `application/x-www-form-urlencoded`, `multipart/form-data ,application/json` ... etc ).
- [ ] Valideer de gebruiker invoer om veel voorkomende kwetsbaarheden te voorkomen (v.b. `XSS`, `SQL-Injection`, `Remote Code Execution` ... etc).
- [ ] Gebruik geen gevoelige data (`credentials`, `Wachtwoorden`, `security tokens`, of `API keys`) in de URL, maar gebruik de standaard Authorization header.
- [ ] Gebruik een API Gateway service voor caching, policies (b.v. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) en voor het dynamisch deployen van API middelen.

## Processing
- [ ] Controleer dat alle endpoints zijn beschermd achter de authenticatie om het omzeilen van authenticatie te voorkomen.
- [ ] Gebruik `/me/orders` i.p.v. `/user/654321/orders` om het 'lekken' van id's te voorkomen.
- [ ] Gebruik geen auto increment id's. Maak gebruik van `UUID`.
- [ ] Als je XML files parsed, controleer dat entity parsing niet aan staat om `XXE` (XML external entity attack) te voorkomen.
- [ ] Als je XML files parsed, controleer dat entity expansion niet aan staat om `Billion Laughs/XML bomb` te voorkomen via `exponential entity expansion attack`.
- [ ] Gebruik CDN voor het uploaden van bestanden.
- [ ] Als er met grote/mega hoeveelheden data gewerkt wordt, gebruik dan Workers en Queues om snel een response te geven en HTTP Blocking te voorkomen.
- [ ] Vergeet niet om de DEBUG mode uit te zetten.

## Output
- [ ] Stel de `X-Content-Type-Options: nosniff` header in.
- [ ] Stel de `X-Frame-Options: deny` header in.
- [ ] Stel de `Content-Security-Policy: default-src 'none'` header in.
- [ ] Verwijder vingerafdruk headers - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Dwing `content-type` headers af voor de response. Als je `application/json` antwoordt, dan is de `content-type` : `application/json`.
- [ ] Stuur geen gevoelige data terug: `Gebruikersnamen`, `Wachtwoorden`, `security tokens`.
- [ ] Geef de correcte HTTP antwoord code terug op basis van de uitgevoerde operatie (v.b. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` ... etc).

## CI & CD
- [ ] Controleer het ontwerp en de implementatie met unit/integration test dekking.
- [ ] Gebruik een code review traject en controleer niet zelf je eigen code.
- [ ] Scan de API voor het naar productie zetten door AV software, niet alleen eigen code maar ook de libraries en andere gebruikte dependencies.
- [ ] Ontwikkel een terugrol oplossing.


---

## Zie ook:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Een verzameling nuttige bronnen voor het bouwen van RESTful HTTP+JSON API's.


---

Translation by | Vertaling door :[S.Holzhauer](https://github.com/SHolzhauer)

# Contribution
Voel u vrij om bij te helpen door deze repository te fork, wijzigingen aan te brengen, en pull requests in te dienen. Voor vragen kunt u ons mailen op `team@shieldfy.io`.
