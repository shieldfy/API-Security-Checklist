[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# Checklist per la sicurezza delle API
Una checklist per le più importanti contromisure da mettere in pratica quando strutturiamo, testiamo e rilasciamo le nostre API.


---

## Autenticazione
- [ ] Non usare la `Basic Auth` Utilizzare piuttosto dei sistemi standard di identificazione (es. JWT, OAuth).
- [ ] Non re-inventarsi sistemi di `autenticazione`, `generazione token`, `salvataggio password`. Utilizzare gli standard.
- [ ] Utilizzare `Max Retry` e le jail features per il Login.
- [ ] Utilizzare la cifratura per tutti i dati sensibili.

### JWT (JSON Web Token)
- [ ] Utilizzare una chiave random complessa (`JWT Secret`) per rendere assai difficile il brute force del token.
- [ ] Non ricavare l'algoritmo dal payload. Forzare l'algoritmo nel backend (`HS256` o `RS256`).
- [ ] Rendere la scadenza del token (`TTL`, `RTTL`) il più breve possibile.
- [ ] Non memorizzare dati sensibili nel payload JWT, può essere decodificato [facilmente](https://jwt.io/#debugger-io).

### OAuth
- [ ] Validare sempre il valore di `redirect_uri` lato server permettendo solo url verificati nella whitelist.
- [ ] Tentare sempre lo scambio attraverso il codice e non tramite token (non permettere `response_type=token`).
- [ ] Utilizzare il parametro `state` con un hash random per prevenire il CSRF durante il processo di autenticazione OAuth.
- [ ] Definire lo scope di default e validare i parametri dello scope per ogni singola applicazione.

## Accesso
- [ ] Limitare le richieste (Throttling) per evitare attacchi DDoS o brute-force.
- [ ] Utilizzare il protocollo HTTPS per evitare attacchi MITM (Man In The Middle Attack).
- [ ] Utilizzare l'header `HSTS` per evitare attacchi SSL Strip.

## Input
- [ ] Utilizzare il metodo HTTP appropriato in base all'azione: `GET (lettura)`, `POST (scrittura)`, `PUT/PATCH (sostituzione/modifica)`, e `DELETE (cancellazione)`, e rispondere con uno status `405 Method Not Allowed` se il metodo della richiesta non è appropriato.
- [ ] Validare il `content-type` rispetto all' Accept header (Content Negotiation) per consentire solo i formati supportati (es. `application/xml`, `application/json`, ecc.) e rispondere con un `406 Not Acceptable` se la risposta non coincide.
- [ ] Validare il `content-type` in base alle strutture accettate (es. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc).
- [ ] Validare sempre gli input dell'utente per evitare attacchi comuni (es. `XSS`, `SQL-Injection`, `Remote Code Execution`, ecc.).
- [ ] Non utilizzare mai dati sensibili (`credenziali`, `password`, `security tokens`, o `API keys`) nell'url, utilizzare piuttosto gli Authorization header.
- [ ] Utilizzare un gateway per abilitare il caching delle API, con sistema di controllo delle chiamate (es. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`).

## Processing
- [ ] Verificare che tutti gli endpoints siano protetti dal sistema di autenticazione, per evitare eventuali falle.
- [ ] L'ID dell'utente corrente andrebbe sempre evitato nelle url. Utilizzare ad esempio `/me/orders` piuttosto che `/user/654321/orders`.
- [ ] Non ricorrere all'autoincremento di un ID. Utilizzare piuttosto un `UUID`.
- [ ] Se stai effettuando il parsing di un file XML, controlla che l'entity parsing non sia attiva per evitare `XXE` (XML external entity attack).
- [ ] Se stai effettuando il parsing di un file XML, controlla che l'entity expansion non sia attiva per evitare il `Billion Laughs/XML bomb`.
- [ ] Utilizzare una CDN per l'upload dei file.
- [ ] Se stai gestendo grandi moli di dati, utilizza Workers e Queues per processare i dati in background evitando che la chiamata HTTP vada in blocco.
- [ ] Ricordarsi sempre di disattivare le eventuali modalità di DEBUG.

## Output
- [ ] Inviare l'header `X-Content-Type-Options: nosniff`.
- [ ] Inviare l'header `X-Frame-Options: deny`.
- [ ] Inviare l'header `Content-Security-Policy: default-src 'none'`.
- [ ] Rimuovere header che permettono il riconoscimento - `X-Powered-By`, `Server`, `X-AspNet-Version` ecc.
- [ ] Forzare il `content-type` nella chiamata di risposta: se per esempio viene ritornato un `application/json` forzare il `content-type` a `application/json`.
- [ ] Non ritornare mai dati sensibili come `credenziali`, `password`, `security tokens`.
- [ ] Ritornare sempre lo status code corretto in base all'esito della chiamata. (es. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, ecc).

## CI & CD
- [ ] Verificare il design attraverso gli unit/integration tests.
- [ ] Definire e utilizzare una procedura di code review per il rilascio, evitando l'auto approvazione.
- [ ] Verificare che tutti i componenti dei servizi siano controllati da software AV prima di essere messi in produzione, incluse le librerie di terze parti.
- [ ] Definire una strategia di rollback per il deploy.


---

## Guarda anche:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Una collezione di risorse utili per la creazione di API RESTful HTTP+JSON.


---

# Contribuire
Siate liberi di contribuire a questo progetto facendo un fork, modificandolo e inviando una pull request. Per qualsiasi dubbio inviare un'email all'indirizzo: `team@shieldfy.io`.
