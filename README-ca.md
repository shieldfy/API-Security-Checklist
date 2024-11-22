[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# Llista de verificació de seguretat per a APIs

Llista de comprovació de les contramesures de seguretat més importants a l'hora de dissenyar, provar i llançar la vostra API.

---

## Autenticació

- [ ] No utilitzeu `Basic Auth`. Utilitzeu l'autenticació estàndard en el seu lloc (per exemple, [JWT](https://jwt.io/)).
- [ ] No reinventeu la roda en `Autenticació`, `generació de tokens`, `emmagatzematge de contrasenyes`. Utilitzeu els estàndards.
- [ ] Utilitzeu polítiques de límit de reintents (`Max Retry`) i funcionalitats de jailing al Login.
- [ ] Utilitzeu el xifratge en totes les dades sensibles.

### JWT (JSON Web Token)

- [ ] Utilitzeu una clau complicada aleatòria (`JWT Secret`) per fer que forçar el token sigui molt difícil.
- [ ] No extregueu l'algorisme de l'encapçalament. Forci l'algorisme al backend (`HS256` o `RS256`).
- [ ] Feu l'expiració del token (`TTL`, `RTTL`) el més curt possible.
- [ ] No emmagatzemeu dades sensibles en la càrrega útil del JWT, es pot descodificar [fàcilment](https://jwt.io/#debugger-io).
- [ ] Eviteu emmagatzemar massa dades. El JWT normalment es comparteix en encapçalaments i tenen un límit de mida.

## Accés

- [ ] Limiteu les sol·licituds (`Throttling`) per evitar atacs de DDoS / força bruta.
- [ ] Utilitzeu HTTPS al servidor amb TLS 1.2+ i xifrats segurs per evitar atacs MITM (Man In The Middle Attack).
- [ ] Utilitzeu l'encapçalament `HSTS` amb SSL per evitar atacs d'extracció SSL.
- [ ] Desactiveu les llistes de directoris.
- [ ] Per a les API privades, permeteu l'accés només des de IPs/hosts autoritzats.

## Autorització

### OAuth

- [ ] Valideu sempre `redirect_uri` al servidor per permetre només URL autoritzades.
- [ ] Intenteu canviar sempre per codi i no per tokens (no permeteu `response_type=token`).
- [ ] Utilitzeu el paràmetre `state` amb un hash aleatori per evitar CSRF en el procés d'autorització d'OAuth.
- [ ] Definiu l'scope per defecte i valideu els paràmetres d'scope per a cada aplicació.

## Entrada

- [ ] Utilitza el mètode HTTP adequat segons l'operació: `GET (llegir)`, `POST (crear)`, `PUT/PATCH (reemplaçar/actualitzar)`, i `DELETE (eliminar)`, i respon amb `405 Method Not Allowed` si el mètode sol·licitat no és adequat per al recurs sol·licitat.
- [ ] Valida el `content-type` a l'encapçalament Accept de la sol·licitud (Content Negotiation) per permetre només el teu format compatible (per exemple, `application/xml`, `application/json`, etc.) i respon amb una resposta `406 Not Acceptable` si no coincideix.
- [ ] Valida el `content-type` de les dades enviades com accepteu (per exemple, `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc.).
- [ ] Valida l'entrada de l'usuari per evitar vulnerabilitats comunes (per exemple, `XSS`, `Injecció SQL`, `Execució de codi remot`, etc.).
- [ ] No utilitzis cap dada sensible (`credentials`, `passwords`, `security tokens`, or `API keys`) a l'URL, sinó que utilitza l'encapçalament d'autorització estàndard.
- [ ] Utilitza només el xifratge al servidor.
- [ ] Utilitza un servei d'API Gateway per habilitar polítiques de memòria cau, polítiques de límit de taxa (per exemple, `Quota`, `Spike Arrest` o `Concurrent Rate Limit`) i desplegar recursos d'API dinàmicament.

## Processament

- [ ] Comprova si tots els endpoints estan protegits darrere de l'autenticació per evitar el procés d'autenticació trencat.
- [ ] S'hauria d'evitar l'ID de recurs propi de l'usuari. Utilitza `/me/orders` en lloc de `/user/654321/orders`.
- [ ] No utilitzis IDs autoicrementals. Utilitza `UUID` en lloc d'això.
- [ ] Si estàs analitzant dades XML, assegura't que l'anàlisi d'entitats no estigui habilitat per evitar `XXE` (XML external entity attack).
- [ ] Si estàs analitzant XML, YAML o qualsevol altre llenguatge amb àncores i referències, assegura't que l'expansió d'entitats no estigui habilitada per evitar `Billion Laughs/XML bomb` a través d'un atac d'expansió d'entitats exponencial.
- [ ] Utilitza un CDN per carregar fitxers.
- [ ] Si estàs tractant amb una gran quantitat de dades, utilitza Workers i Queues per processar el màxim possible en segon pla i retornar una resposta ràpida per evitar el bloqueig HTTP.
- [ ] No oblidis desactivar el mode DEBUG.
- [ ] Utilitza stacks no executables quan estiguin disponibles.

## Sortida

- [ ] Envia l'encapçalament `X-Content-Type-Options: nosniff`.
- [ ] Envia l'encapçalament `X-Frame-Options: deny`.
- [ ] Envia l'encapçalament `Content-Security-Policy: default-src 'none'`.
- [ ] Elimina els encapçalaments d'identificació - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc.
- [ ] Força `content-type` per a la teva resposta. Si tornes `application/json`, llavors la teva resposta de `content-type` és `application/json`.
- [ ] No retornis dades sensibles com `credencials`, `contrasenyes` o `tokens de seguretat`.
- [ ] Retorna el codi d'estat adequat segons l'operació completada. (per exemple, `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc.).

## CI & CD

- [ ] Auditora el teu disseny i implementació amb cobertura de tests unitàris i d'integració.
- [ ] Utilitza un procés de revisió de codi i ignora l'autoaprovació.
- [ ] Assegura't que tots els components dels teus serveis siguin escanejats estàticament per un programari AV abans de desplegar-los a producció, incloent biblioteques de tercers i altres dependències.
- [ ] Executa contínuament tests de seguretat (anàlisi estàtica/dinàmica) en el teu codi.
- [ ] Comprova les teves dependències (tant el programari com el sistema operatiu) per a vulnerabilitats conegudes.
- [ ] Dissenyar una solució de reversió per a desplegaments.

## Monitoratge

- [ ] Utilitza inicis de sessió centralitzats per a tots els serveis i components.
- [ ] Utilitza agents per monitorar tot el tràfic, errors, sol·licituds i respostes.
- [ ] Utilitza alertes per SMS, Slack, correu electrònic, Telegram, Kibana, Cloudwatch, etc.
- [ ] Assegura't de no registrar cap dada sensible com ara targetes de crèdit, contrasenyes, PINs, etc.
- [ ] Utilitza un sistema IDS i/o IPS per monitorar les teves sol·licituds d'API i instàncies.

---

## També podeu veure:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Una col·lecció de recursos útils per a la construcció de RESTful HTTP+JSON APIs.

---

# Contribució

No dubteu a contribuir fent un fork d'aquest repositori, fent alguns canvis i enviant pull requests. Per a qualsevol pregunta envia'ns un correu electrònic a `team@shieldfy.io`.
