[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [বাংলা](./README-bn.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# Lista kontrolna bezpieczeństwa API
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
- [ ] Ustaw wygaszanie tokenów (`TTL`, `RTTL`) najkrótsze jak to możliwe.
- [ ] Nie przechowuj wrażliwych danych w payloadzie `JWT`, mogą być one [łatwo zdekodowane](https://jwt.io/#debugger-io).
- [ ] Unikaj przechowywania zbyt dużej ilości danych. JWT jest zwykle udostępniany w nagłówkach i ma limit rozmiaru.

## Dostęp
- [ ] Ustaw limit zapytań (Throttling) aby uniknąć ataku DDoS / brute-force.
- [ ] Użyj HTTPS aby uniknąć MITM (Man In The Middle Attack) - Ataku polegającego na pośrednictwie w wymianie informacji pomiędzy dwoma punktami np. klientem i serwerem.
- [ ] Użyj nagłówka `HSTS` z SSL aby uniknąć SSL Strip attack.
- [ ] Wyłącz wykazy katalogów.
- [ ] W przypadku prywatnych API, zezwalaj na dostęp tylko z adresów IP/hostów umieszczonych na białej liście.

## Authorization

### OAuth
- [ ] Zawsze waliduj `redirect_uri` po stronie serwera aby zezwolić tylko URL-om z dozwolonej listy (`whitelist`).
- [ ] Zawsze próbuj wymienić kodem nie tokenami (nie zezwalaj na `response_type=token`).
- [ ] Użyj parametru `state` z losowym hashem aby zabezpieczyć proces OAuth przed atakiem CSRF.
- [ ] Zdefiniuj oraz waliduj zakres parametrów dla każdej aplikacji.

## Wejście
- [ ] Użyj odpowiedniej metody protokołu HTTP dla danej operacji: `GET (odczyt)`, `POST (tworzenie)`, `PUT/PATCH (zmiana)`, and `DELETE (usuwanie)`, i odpowiadaj `405 Method Not Allowed` jeżeli metoda zapytania jest niepoprawna.
- [ ] Waliduj `content-type` podczas zapytań i zezwalaj jedynie na wymagane typy danych (np. `application/xml`, `application/json`) oraz odpowiadaj `406 Not Acceptable` jeżeli nie pasują.
- [ ] Waliduj `content-type` informacji przekazywanych metodą POST (np. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`).
- [ ] Waliduj informacje wprowadzane przez użytkownika, aby uniknąć zagrożeń (np.. `XSS`, `SQL-Injection`, `Zdalne Wykonanie Skryptu`).
- [ ] Nie używaj żadnych wrażliwych danych w URL, zamiast tego użyj standardowego nagłówka Autoryzującego.
- [ ] Użyj tylko szyfrowania po stronie serwera.
- [ ] Użyj usługi API Gateway aby włączyć caching oraz np. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`.

## Przetwarzanie
- [ ] Sprawdź czy wszystkie endpointy są zabezpieczone uwierzytelnianiem aby uniknąć niautoryzowanego dostępu.
- [ ] Unikaj ukazywania ID użytkownika. Użyj np. `/me/orders` zamiast `/users/654321/orders/`.
- [ ] Nie używaj auto inkrementacji w polu ID. Zamiast tego użyj `UUID`.
- [ ] Jeżeli parsujesz pliki XML, upewnij się, że jesteś odporny na `XXE` (XML external entity attack) oraz `Billion Laughs/XML bomb`.
- [ ] Użyj CDN do przechowywania wysyłanych plików.
- [ ] Jeżeli pracujesz z dużą ilością danych, użyj procesów Workers oraz kolejkowania Queues aby przetworzyć jak najwięcej w tle i zwrócić informacje szybko aby uniknąć blokowania HTTP.
- [ ] Nie zapomnij o wyłączeniu trybu debugowania.
- [ ] Użyj niewykonywalnych stacks jeśli są dostępne.

## Wyjście
- [ ] Wyślij nagłówek `X-Content-Type-Options: nosniff`.
- [ ] Wyślij nagłówek `X-Frame-Options: deny`.
- [ ] Wyślij nagłówek `Content-Security-Policy: default-src 'none'`.
- [ ] Usuń nagłówki cyfrowego odcisku palca (digital fingerprint) - `X-Powered-By`, `Server`, `X-AspNet-Version`.
- [ ] Wymuś `content-type` podczas zwracania danych. Jeżeli zwracasz `application/json` wtedy twój `content-type` to `application/json`.
- [ ] Nie zwracaj ważnych informacji jak `dane uwierzytelniające`, `hasła`, `tokeny bezpieczeństwa`.
- [ ] Zwróc odpowiedni status w zależności od operacji. (np. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`).

## CI & CD
- [ ] Przetestuj wszystkie rozwiązania stosując testy jednostkowe.
- [ ] Oddaj kod do przejrzenia innym, poddaj go `code review`.
- [ ] Upewnij się, że wszystkie komponenty twojej usługi są skanowane przez oprogramowanie antywirusowe przed wejściem na produkcje. Uwzględnij także zewnętrzne biblioteki.
- [ ] Ciągle uruchamiaj testy bezpieczeństwa (analiza statyczna/dynamiczna) w swoim kodzie.
- [ ] Sprawdź swoje zależności (zarówno oprogramowanie i system operacyjny) pod kątem znanych luk w zabezpieczeniach.
- [ ] Stwórz możliwość szybkiego wycofania udostępnionego wdrożenia.

## Monitorowanie
- [ ] Użyj ze scentralizowanych logowań dla wszystkich usług i komponentów.
- [ ] Użyj agentów do monitorowania całego ruchu, błędów, żądań i odpowiedzi.
- [ ] Użyj alertów dla SMS, Slack, Email, Telegram, Kibana, Cloudwatch, itp.
- [ ] Upewnij się, że nie rejestrujesz żadnych poufnych danych, takich jak karty kredytowe, hasła, kody PIN, itp.
- [ ] Użyj systemu IDS i/lub IPS do monitorowania żądań i instancji API.


---

## Zobacz także:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - [ENG] Zbiór wartościowych narzędzi do tworzenia REST HTTP+JSON API.


---

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
