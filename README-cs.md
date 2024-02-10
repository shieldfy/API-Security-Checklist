[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# Seznam API zabezpečení

Kontrolní seznam nejdůležitějších bezpečnostních opatření při návrhu, testování a uvolňování rozhraní API.

---

## Autentizace

- [ ] Nepoužívejte `Basic Auth`. Místo toho použijte standardní ověřování (např. [JWT](https://jwt.io/)).
- [ ] Nevymýšlejte znovu způsoby `ověření`, `generace tokenů`, `ukládání hesel`. Držte se standardů.
- [ ] Používejte u loginů funkce `Maximum Pokusů` a dočasné zablokování.
- [ ] Šifrujte všecha citlivá data.

### JWT (JSON Web Token)

- [ ] Použijte náhodný a sofistikovaný klíč (`JWT Secret`), aby bylo složité token získat přes brute-force.
- [ ] Nepoužívejte algoritmy posílané v hlavičce. Vynuťte použití algoritmů na backendu (`HS256` nebo `RS256`).
- [ ] Zajistěte, aby platnost tokenu (`TTL`, `RTTL`) byla co nejkratší.
- [ ] Neukládejte uvnitř JWT citlivá data, mohou být následně [poměrně jednoduše] dekódovány (https://jwt.io/#debugger-io).
- [ ] Neukládejte v nich příliš mnoho dat. JWT se obvykle sdílí v hlavičkách a jejich velikost je omezena.

## Přístup

- [ ] Omezte počet příchozích requestů (Zahlcení) aby jste předešli DDoS/brute-force útokům.
- [ ] Na straně serveru používejte protokol HTTPS s protokolem TLS 1.2+ a bezpečnými šiframi, abyste se vyhnuli útoku MITM (Man in the Middle).
- [ ] Použijte hlavičku `HSTS` s protokolem SSL, abyste se vyhnuli útokům SSL Strip.
- [ ] Vypněte vypisování adresářů.
- [ ] U privátních API povolte přístup pouze z IP adres/hostů nastavených ve whitelistu.

## Autorizace

### OAuth

- [ ] Vždy ověřujte `redirect_uri` na straně serveru, abyste povolili pouze adresy URL uvedené ve whitelistu.
- [ ] Vždy se snažte vyměňovat autorizační kód, ne přístupové tokeny (nepovolujte `response_type=token`).
- [ ] Použijte parametr `state` s náhodným hashem, abyste zabránili CSRF v autorizačním procesu OAuth.
- [ ] Definujte výchozí rozsah a ověřte parametry tohoto rozsahu pro každou aplikaci.

## Vstupy

- [ ] Použijte správné metody HTTP podle operace: `GET (čtení)`, `POST (vkládání)`, `PUT/PATCH (nahrazení/update)`, a `DELETE (smazání záznamu)`, a odpovězte `405 Method Not Allowed` pokud požadovaná metoda není vhodná pro požadovaný prostředek.
- [ ] Ověřte `content-type` v hlavičce požadavku Accept (Content Negotiation), abyste povolili pouze vámi podporovaný formát (např. `application/xml`, `application/json` atd.) a v případě neshody odpovězte `406 Not Acceptable`.
- [ ] Ověřte typ `content-type` odesílaných dat tak, jak je přijímáte (např. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` atd.).
- [ ] Ověřujte uživatelské vstupy, abyste se vyhnuli běžným zranitelnostem (např. `XSS`, `SQL-Injection`, `Remote Code Execution` atd.).
- [ ] Nepoužívejte v URL žádné citlivé údaje (`přihlašovací údaje`, `hesla`, `security tokeny` nebo `API klíče`), ale použijte standardní Authorization hlavičku.
- [ ] Používejte pouze šifrování na straně serveru.
- [ ] Pomocí služby API Gateway můžete povolit ukládání do mezipaměti, zásady pro omezení rychlosti (např. `Quota`, `Spike Arrest` nebo `Concurrent Rate Limit`) a dynamické nasazování prostředků API.

## Zpracování

- [ ] Zkontrolujte, zda jsou všechny koncové body chráněny určitým ověřením přístupu, aby nedošlo k porušení procesu ověřování.
- [ ] Neměla by se používat jednotlivá ID uživatelů. Místo `/user/654321/orders` použijte `/me/orders`.
- [ ] Nepoužívejte auto-inkrementaci u ID. Použijte místo toho `UUID`.
- [ ] Pokud zpracováváte XML data, ujistěte se, že není povoleno procházení jednotlivých entit, abyste se vyhnuli `XXE` (XML external entity attack).
- [ ] Pokud zpracováváte XML, YAML nebo jakýkoli jiný jazyk s kotvami a odkazy, ujistěte se, že není povoleno rozšiřování entit, abyste se vyhnuli útokům jako `Billion Laughs/XML bomb` pomocí exponenciálního rozšiřování entit.
- [ ] Pro nahrávání souborů používejte síť CDN.
- [ ] Pokud pracujete s obrovským množstvím dat, použijte Workery a fronty, abyste jich co nejvíce zpracovali na pozadí, rychle vrátili odpověď, a vyhnuli se tak HTTP blokaci.
- [ ] Nezapomeňte vypnout DEBUG režim.
- [ ] Pokud je to možné používejte nespustitelné stacky (NX).

## Výstupy

- [ ] V hlavičce odpovědi posílejte `X-Content-Type-Options: nosniff`.
- [ ] V hlavičce odpovědi posílejte `X-Frame-Options: deny`.
- [ ] V hlavičce odpovědi posílejte `Content-Security-Policy: default-src 'none'`.
- [ ] Z hlavičky odpovědi odstraňte - `X-Powered-By`, `Server`, `X-AspNet-Version`, atd.
- [ ] Vynuťte v odpovědi použití `content-type`. Pokud vrátíte `application/json`, potom `content-type` vaší odpovědi bude `application/json`.
- [ ] Neposílejte v odpovědích citlivá data jako `přihlašovací údaje`, `hesla`, nebo `security tokeny`.
- [ ] Posílejte správný stavový kód podle toho jak byla operace dokončena. (např. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, atd.).

## CI & CD

- [ ] Zkontrolujte svůj návrh a implementaci řešení jednotkovými/integračními testy.
- [ ] Používejte proces kontroly kódu a to nejlépe třetí nezávislou stranou.
- [ ] Zajistěte, aby všechny součásti vašich služeb byly před nasazením do produkce staticky oskenovány antivirem, včetně všech knihoven dodavatelů a dalších součástí.
- [ ] Průběžně provádějte bezpečnostní testy vašeho kódu (statickou/dynamickou analýzu).
- [ ] Zkontrolujte jestli používané technologie (oboje jak software tak OS) neobsahují známé zranitelnosti.
- [ ] Navrhněte pro nasazený systém možnost rollbacku.

## Monitorování

- [ ] Používejte centralizované přihlašovací údaje pro všechny služby a komponenty.
- [ ] Používejte agenty na monitorování veškeré komunikace, errorů, requestů, a odpovědí.
- [ ] Používejte upozornění pomocí SMS, Slacku, Emailu, Telegramu, Kibany, Cloudwatche, atd.
- [ ] Ujistěte se, že neukládáte do logů žádné citlivé údaje, jako čísla kreditních karet, hesla, kódy PIN atd.
- [ ] Ke sledování API requestů a instancí používejte systém IDS a/nebo IPS.

---

## Viz také:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Sbírka užitečných zdrojů pro vytváření rozhraní RESTful HTTP+JSON API.

---

# Příspěvek

Neváhejte přispět forknutím tohoto repozitáře, provedením nějakých změn a zasláním pull requestu. V případě jakýchkoli dotazů nám napište na e-mail `team@shieldfy.io`.
