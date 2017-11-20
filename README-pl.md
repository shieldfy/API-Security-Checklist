# Lista kontrolna bezpieczeństw API
Lista kontrolna najważniejszych metod zabezpieczenia podczas projektowania, testowania oraz wypuszczania własnego API.


---


## Uwierzytelnianie
- [ ] Nie używaj `Basic Auth`. Użyj standardów uwierzytelniania (np. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Nie wynajduj koła na nowo podczas `Uwierzytelniania`, `generowanie tokenów`, `przechowywania haseł`. Użyj sprawdzonych standardów.
- [ ] Dodaj `Maksymalną ilość prób` oraz inne opcje ograniczające podczas Logowania.
- [ ] Szyfruj wszystkie wrażliwe (ważne) dane.

### JWT (JSON Web Token)
- [ ] Użyj losowego, skomplikowanego klucza (`JWT Secret`) aby uczynić token bezpieczniejszym przeciw atakom typu `brute force`.
- [ ] Algorytmy trzymaj w backendzie, nie upubliczniaj algorytmów.
- [ ] Ustaw wygaszanie tokenów  (`TTL`, `RTTL`) najkrótsze jak to możliwe.
- [ ] Nie przechowuj wrażliwych danych w `JWT payload`, mogą był łatwo dekodowane przy pomocy [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] Zawsze waliduj `redirect_uri` po stronie serwera aby zezwolić tylko URL-om z dozwolonej listy (`whitelist`).
- [ ] Zawsze próbuj wymienić kodem nie tokenami (nie zezwalaj na `response_type=token`).
- [ ] Użyj parametru `state` z losowym hashem aby zabezpieczyć proces OAuth przed atakiem CSRF.
- [ ] Zdefiniuj oraz waliduj zakres parametrów dla każdej aplikacji.

## Dostęp
- [ ] Ustaw limit zapytań (Throttling) aby uniknąć ataku DDoS / brute-force.
- [ ] Użyj HTTPS aby uniknąć MITM (Man In The Middle Attack) - ataku polegającego na pośrednictwie w wymianie informacji pomiędzy dwoma punktami np. klientem i serwerem.
- [ ] Użyj nagłówka `HSTS` z SSL aby uniknąć SSL Strip attack.


## Wejście
- [ ] Użyj odpowiedniej metody protokołu HTTP dla danej operacji: `GET (odczyt)`, `POST (tworzenie)`, `PUT/PATCH (zmiana)`, and `DELETE (usuwanie)`, i odpowiadaj `405 Method Not Allowed` jeżeli metoda zapytania jest niepoprawna.
- [ ] Waliduj `content-type` podczas zapytań i zezwalaj jedynie na wymagane typy danych (np. `application/xml`, `application/json`) oraz odpowiadaj `406 Not Acceptable` jeżeli nie pasują.
- [ ] Waliduj  `content-type` informacji przekazywanych metodą POST (np. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`).
- [ ] Waliduj informacje wprowadzane przez użytkownika, aby uniknąć zagrożeń (np.. `XSS`, `SQL-Injection`, `Zdalne Wykonanie Skryptu`).
- [ ] Nie używaj żadnych wrażliwych danych w URL, zamiast tego użyj standardowego nagłówka Autoryzującego.
- [ ] Użyj usługi API Gateway aby włączyć caching oraz np. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`.


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
