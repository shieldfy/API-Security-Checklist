[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md)

# Checklist per la sicurezza delle API
Una checklist per le più importanti contromisure da mettere in pratica quando strutturiamo, testiamo e rilasciamo le nostre API.


---

## Autenticazione
- [ ] Non usare la `Basic Auth` Utilizzare piuttosto dei sistemi di identification standard (es. JWT, OAuth).
- [ ] Non re-inventarsi sistemi di `Autenticazione`, `generazione token`, `salvtaggio password`. Utilizzare gli standard.
- [ ] Use `Max Retry` and jail features in Login.
- [ ] Utilizzare la criptazione per tutti i dati sensibili.

### JWT (JSON Web Token)
- [ ] Use a random complicated key (`JWT Secret`) to make brute forcing the token very hard.
- [ ] Don't extract the algorithm from the payload. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] Make token expiration (`TTL`, `RTTL`) as short as possible.
- [ ] Don't store sensitive data in the JWT payload, it can be decoded [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] Validare sempre il valore di `redirect_uri` lato server permettendo solo url verificati in whitelist.
- [ ] Always try to exchange for code and not tokens (non permettere `response_type=token`).
- [ ] Utilizzare il parametro `state` con un hash random per prevenire il CSRF durante il processo di autenticazione OAuth.
- [ ] Deinire lo scope di default e validare i parametri dello scope per ogni singola applicazione.

## Accesso
- [ ] Limita la richieste (Throttling) per evitare attacchi DDoS o brute-force.
- [ ] Utilizzare il protocollo HTTPS per evitare attacchi MITM (Man In The Middle Attack).
- [ ] Utilizzare l'header `HSTS` per evitare attacchi SSL Strip.

## Input
- [ ] Utilizza il metodo HTTP appropriato in base all'azione: `GET (lettura)`, `POST (scrittura)`, `PUT/PATCH (sostituzione/modifica)`, e `DELETE (cancellazione)`, e rispondi con uno status `405 Method Not Allowed` se il metodo della richiesta non è appropiato.
- [ ] Valida il `content-type` rispetto all' Accept header (Content Negotiation) per permettere solo i formati supportati (es. `application/xml`, `application/json`, ecc.) e rispondi con un `406 Not Acceptable` se la response non coincide.
- [ ] Valida il `content-type` in base alle strutture accettate (es. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc).
- [ ] Validare sempre gli input dell'utente per evitare attacchi comuni (es. `XSS`, `SQL-Injection`, `Remote Code Execution`, ecc.).
- [ ] Non utilizzare mai dati sensibili (`credenziali`, `password`, `security tokens`, o `API keys`) nell'url, utilizza piuttosto gli Authorization header.
- [ ] Utilizza un gateway per abilitare il caching delle API, con sistema di controllo delle chiamate (es. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`).

## Processing
- [ ] Verifica che tutti gli endpoints siano protetti dal sistema di autenticazione, per evitare eventuli falle.
- [ ] L'ID dell'utente attuale andrebbe sempre evitato nelle url. Utilizzare ad esempio `/me/orders` piuttosto che `/user/654321/orders`.
- [ ] Non auto incrementare un ID. Utilizza piuttosto un `UUID`.
- [ ] Se stai effettuando il parsing di un file XML, controlla che l'entity parsing non sia attiva per evitare `XXE` (XML external entity attack).
- [ ] Se stai effettuando il parsing di un file XML, controlla che l'entity expansion non sia attiva per evitare il `Billion Laughs/XML bomb`.
- [ ] Utilizza una CDN per l'upload dei file.
- [ ] Se stai gestendo grandi moli di dati, utilizza Workers e Queues per processare i dati in background evitando che la chiamata HTTP vada in blocco.
- [ ] Ricordarsi sempre di disattivare le eventuali modalità di DEBUG.

## Output
- [ ] Invia l'header `X-Content-Type-Options: nosniff`.
- [ ] Invia l'header `X-Frame-Options: deny`.
- [ ] Invia l'header `Content-Security-Policy: default-src 'none'`.
- [ ] Rimuovi header che permettono il riconoscimento - `X-Powered-By`, `Server`, `X-AspNet-Version` ecc.
- [ ] Forza il `content-type` nella chiamata di risposta, se per esempio ritorni un `application/json` forza il `content-type` a `application/json`.
- [ ] Non ritornare mai dati sensibili come `credenziali`, `password`, `security tokens`.
- [ ] Ritornare sempre lo status code corretto in base a come si è conclusa la chiamata. (es. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, ecc).

## CI & CD
- [ ] Verifica il tuo design attraverso gli unit/integration tests.
- [ ] Definisci e utilizza una procedura di code review per il rilascio, evitando l'auto approvazione.
- [ ] Verifica che tutti i componenti dei tuoi servizi siano controllati da software AV prima di essere messi in produzione, incluse le librerie di terze parti.
- [ ] Definisci una strategia di rollback per il delpoy.


---

## Guarda anche:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Una collezione di risorse utili per la creazione di API RESTful HTTP+JSON.


---

# Contribuire
Sentitivi liberi di contribuire a questo progetto facendo un fork, modificandolo e inviando una pull request. Per qualsiasi dubbio inviare un'email all'indirizzo: `team@shieldfy.io`.
