[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API Security Checkliste

Checkliste für die wichtigsten Sicherheitsmaßnahmen beim Designen, Testen und Veröffentlichen deiner API.

---

## Authentifizierung

- [ ] Verwende kein `Basic Auth`. Nutze standardisierte Authentifizierungsmethoden (bspw. JWT, OAuth).
- [ ] Erfinde das Rad nicht neu für `Authentication`, `Tokengenerierung` oder `Passwort speichern`. Nutze hierfür existierende Standards.
- [ ] Nutze eine `limitierte Anzahl von Anmeldeversuche` und Aussperrfunktionen (Ban, IP-Block, Permanent) im Loginprozess.
- [ ] Nutze Verschlüsselung für alle sensitiven Daten.

### JWT (JSON Web Token)

- [ ] Verwende einen per Zufall generierten, komplizierten Schlüssel (`JWT Secret`), um Brute Force Attacken gegen diesen so schwer wie möglich zu machen.
- [ ] Verwende den Algorithmus des Payloads ausschließlich über das Backend, sodass dieser geheim bleibt (`HS256` oder `RS256`).
- [ ] Lege einen möglichst kurzen Gültigkeitszeitraum für den Token fest (`TTL`, `RTTL`).
- [ ] Speichere keine sensitiven Daten im JWT Payload, denn dieser kann [einfach entkodiert werden](https://jwt.io/#debugger-io).
- [ ] Vermeiden zu viele Daten zu speichern. JWT wird normalerweise in Headern geteilt und hat eine Größenbeschränkung.

## Zugriff

- [ ] Limitiere alle Requests (Throttling), um DDoS / Brute-Force Attacken zu verhindern.
- [ ] Nutze HTTPS serverseitig, um MITM (Man In The Middle Attack) zu verhindern.
- [ ] Setze `HSTS` (HTTP Strict Transport Security) im Header bei SSL, um SSLStrip Attacken zu verhindern.
- [ ] Deaktivieren Verzeichniseinträge.
- [ ] Erlauben für private APIs den Zugriff nur von IPs/Hosts auf der Whitelist.

## Autorisierung

### OAuth

- [ ] Überprüfe stets die `redirect_uri` serverseitig und erlaube nur URLs aus einer Whitelist.
- [ ] Frage immer mit einem Access-Code (vom initialen Request) einen Access-Token ab (verbiete `response_type=token`).
- [ ] Nutze den `state` Parameter immer mit einem zufälligem Hash, um CSRF auf den OAuth Authentifizierungsprozess zu verhindern.
- [ ] Definiere einen Standard-Scope und validiere alle Scope Parameter für jede Applikation.

## Input

- [ ] Nutze für Requests die passenden HTTP Methoden: `GET (Lesen)`, `POST (Erzeugen)`, `PUT/PATCH (Ersetzen/Aktualisieren)`, and `DELETE (Datensatz löschen)`, und gib `405 Method Not Allowed`, wenn die angeforderte Methode nicht auf die Ressource passt.
- [ ] Validiere den `content-type` im "Accept" Header der Anfrage und erlaube nur unterstützte Formate (wie `application/xml`, `application/json`, usw). Gib den Response `406 Not Acceptable` zurück, wenn keine der übergebenen Content-Typen unterstützt wird.
- [ ] Validiere den `Content-Type` im Header der Anfrage für übertragene Daten (bspw. POST oder PUT) wie bspw. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, usw.
- [ ] Validiere immer alle Eingaben im Request und allen Parametern um allgemeine Angriffsmöglichkeiten zu verhindern (bspw. `XSS`, `SQL-Injection`, `Remote Code Execution`, usw).
- [ ] Verwende niemals sensitive Daten (`Anmeldedaten`, `Passwörter`, `Security Tokens`, oder `API-Schlüssel`) in der URL, aber nutze den standardisierten "Authorization" Header.
- [ ] Verwenden nur serverseitige Verschlüsselung.
- [ ] Nutze ein API Gateway Service für Caching, Rate Limit Regeln (bspw. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) und der Bereitstellung dynamischer API Ressourcen.

## Verarbeitung

- [ ] Überprüfe, ob alle Endpunkte mit einer Authentifizierung geschützt sind.
- [ ] Nutzereigene Ressourcen-Ids sollten vermieden werden. Verwende `/me/orders` statt `/user/654321/orders`.
- [ ] Verwende keine automatisch hochzählende IDs, sondern `UUID`, damit Ressourcen nicht einfach erraten werden können.
- [ ] Beim Verarbeiten einer XML-Datei, sollte Entitätsverarbeitung deaktiviert sein, um `XXE` (XML External Entity Attacken) zu verhindern.
- [ ] Beim Verarbeiten einer XML-Datei, sollte Entitätsexpansion deaktiviert sein, um `Billion Laughs/XML Bombe` zu verhindern.
- [ ] Nutze CDN für Dateiuploads.
- [ ] Wenn du eine große Menge an Daten verarbeiten musst, nutze Worker und Queues, um so viel wie möglich im Hintergrund zu verarbeiten und schnelle Antwortzeiten zu gewährleisten.
- [ ] Vergiss nicht den DEBUG Modus zu deaktivieren.
- [ ] Verwenden nicht ausführbare Stacks sofern verfügbar.

## Output

- [ ] Sende `X-Content-Type-Options: nosniff` im Header.
- [ ] Sende `X-Frame-Options: deny` im Header.
- [ ] Sende `Content-Security-Policy: default-src 'none'` im Header.
- [ ] Entferne Header wie `X-Powered-By`, `Server`, `X-AspNet-Version` usw, um eventuell veraltete Softwareversionen nicht zu verraten.
- [ ] Sende immer einen `Content-Type` bei Antworten. Wenn du ein JSON lieferst gib als `Content-Type` `application/json` an.
- [ ] Gib niemals sensitive Daten zurück wie `Anmeldedaten`, `Passwörter` oder `Sicherheitsschlüssel`.
- [ ] Verwende immer einen passenden HTTP Statuscode je nach Status der Operation (bspw. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, usw).

## Kontinuierliche Integration (CI) & Continuous Delivery (CD)

- [ ] Nutze Unit- und Integrationstest und deren Abdeckung (Test Coverage), um deine Implementierungen und Design zu kontrollieren.
- [ ] Nutze einen Code Review Prozess, aber bleib sachlich.
- [ ] Stelle sicher, dass alle verwendeten Komponenten (Bibliotheken und alle anderen Abhängigkeiten) noch einmal statisch von einer Anti-Virus Software überprüft wurden bevor diese in die Produktionsumgebung gehen.
- [ ] Führen kontinuierlich Sicherheitstests (statische/dynamische Analyse) für Ihren Code.
- [ ] Überprüfen Ihre Abhängigkeiten (Software und Betriebssystem) auf bekannte Schwachstellen.
- [ ] Stelle sicher, dass du im Fehlerfall auch schnell wieder den vorherigen Stand einspielen kannst (Rollback).

## Überwachung

- [ ] Verwenden Sie zentralisierte Logins für alle Dienste und Komponenten.
- [ ] Verwenden Sie Agenten, um den gesamten Datenverkehr, Fehler, Anfragen und Antworten zu überwachen.
- [ ] Verwenden Sie Benachrichtigungen für SMS, Slack, E-Mail, Telegramm, Kibana, Cloudwatch, usw.
- [ ] Stellen Sie sicher, dass Sie keine sensiblen Daten wie Kreditkarten, Passwörter, PINs, usw protokollierst.
- [ ] Verwenden Sie ein IDS-System und/oder ein IPS-System um die Anforderungen und Instanzen Ihrer API zu überwachen.

---

## Siehe auch:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Eine Sammlung nützlicher Ressourcen für den Aufbau von RESTful HTTP+JSON APIs.

---

# Contribution

Du kannst gerne etwas beisteuern, indem du einen Fork dieses Repositorys erstellst, Änderungen vornimmst und dann einen Pull Request anlegst. Bei Fragen schick uns eine E-Mail an `team@shieldfy.io`.
