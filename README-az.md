[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API təhlükəsizlik yoxlama siyahısı

API-nizi tərtib edərkən, sınaqdan keçirərkən və dərc edərkən ən vacib təhlükəsizlik tədbirlərinin siyahısı.

---

## Autentifikasiya

- [ ] `Basic Auth` istifadə etməyin. Bunun əvəzinə standart identifikasiya həllərindən (məsələn: [JWT](https://jwt.io/), [OAuth](https://oauth.net/) kimi) istifadə edin.
- [ ] `Autentifikasiya`, `tokenlərin yaradılması`, `parolların saxlanması` üçün təkəri yenidən kəşf etməyə çalışmayın. Standartlardan istifadə edin.
- [ ] `Cəhdlərin sayını` məhdudlaşdırmaqla giriş hüquqlarını məhdudlaşdırın.
- [ ] Bütün həssas məlumatlarda şifrələmədən istifadə edin.

### JWT (JSON Veb Token)

- [ ] (`JWT Secret`) kimi təsadüfi, mürəkkəb və çətin açardan istifadə edərək, kobud qüvvə ilə şifrənin açılmasını mümkün qədər çətinləşdirin.
- [ ] Daxil olan məlumatlara əsasən alqoritmi təyin etməyin. Bunu arxa planda reallaşdırın. ('HS256' və ya 'RS256').
- [ ] Tokenin son istifadə tarixini (`TTL`, `RTTL`) mümkün qədər qısa edin.
- [ ] Həssas məlumatlarınızı JWT faydalı yükünə qoymayın, o [Asanlıqla](https://jwt.io/#debugger-io) deşifrə edilə bilər.
- [ ] Çox məlumat saxlamaqdan çəkinin. JWT adətən başlıqlarda paylaşılır və onların ölçü limiti var.

## Giriş

- [ ] Özünüzü DDoS və ya kobud güc hücumlarından qorumaq üçün sorğuları məhdudlaşdırmalısınız.
- [ ] MITM (Man In The Middle Attack) hücumlarından qorunmaq üçün server tərəfində HTTPS-dən istifadə edin.
- [ ] SSL Strip hücumlarından qorunmaq üçün SSL ilə `HSTS` başlığından istifadə edin.
- [ ] Kataloq siyahılarını bağlayın.
- [ ] Şəxsi API-lər üçün yalnız ağ siyahıya alınmış IP-lərdən/hostlardan girişə icazə verin.

## Səlahiyyət

### OAuth

- [ ] Yalnız ağ siyahıya alınmış URL-lərə icazə vermək üçün həmişə server tərəfindəki `redirect_uri` məlumatını yoxlayın.
- [ ] Həmişə işarəni deyil, kodu dəyişməyə çalışın (`response_type=token` istifadə etməyə icazə verməyin).
- [ ] OAuth autentifikasiyası zamanı CSRF-nin qarşısını almaq üçün `state` parametrini təsadüfi olaraq hash edin.
- [ ] Standart əhatə dairəsini təyin edin və hər bir tətbiq üçün əhatə dairəsi parametrlərini yoxlayın.

## Giriş

- [ ] Əməliyyata uyğun olaraq müvafiq HTTP metodundan istifadə edin: `GET (oxu)`, `POST (yarat)`, `PUT/PATCH (dəyişiklik etmək/yeniləmək üçün)` və `DELETE (yazı silmək üçün)`, əgər istədiyiniz üsul resurs üçün uyğun deyilsə, `405 Metoduna İcazə Verilmədi` mesajı ilə cavab verin.
- [ ] Qəbul başlığındakı `məzmun növü` gözlədiyiniz və icazə verdiyiniz formatda olub-olmadığını yoxlayın. (məsələn, `application/xml`, `application/json` və s.) Format uyğun gəlmirsə, `406 Qəbul Edilməz` mesajı ilə cavab verin.
- [ ] Göndərilən məlumatı təsdiq edərkən, daxil olan məlumatların 'məzmun növünü' yoxlayın (məsələn, 'application/x-www-form-urlencoded', 'multipart/form-data', 'application/json' və s.).
- [ ] Ümumi təhlükəsizlik zəifliklərinin qarşısını almaq üçün istifadəçidən gələn hər bir məlumatı yoxlayın (məsələn, `XSS`, `SQL-Injection`, `Remote Code Execution` və s.).
- [ ] URL-də həssas datadan (`etimadnamələr`, `Parollar`, `təhlükəsizlik nişanları` və ya `API açarları`) istifadə etməyin, lakin standart Avtorizasiya başlığından istifadə edin.
- [ ] Yalnız server tərəfində şifrələmədən istifadə edin.
- [ ] Keşləmə və sürət limiti siyasətlərini aktivləşdirmək (məsələn, `Kvota`, `Spike Həbs`, `Paylaşım sürəti limiti`) və API resurslarını dinamik şəkildə yaymaq üçün API Gateway xidmətindən istifadə edin.

## Emal

- [ ] Doğrulama yan keçməsinin qarşısını almaq üçün bütün proses son nöqtələrinin autentifikasiya arxasında qorunub-qorunmadığını yoxlayın.
- [ ] İstifadəçinin öz resurs identifikatorundan istifadə etməkdən çəkinmək lazımdır. `/me/orders` əvəzinə `/user/654321/orders` istifadə edin.
- [ ] Avtomatik artan ID-lərdən istifadə etməyin. Əvəzinə `UUID` istifadə edin.
- [ ] XML fayllarını təhlil edirsinizsə (analiz edirsinizsə), `XXE` (XML xarici obyekt hücumu) qarşısını almaq üçün obyektin təhlilinin aktiv edilmədiyini yoxlayın.
- [ ] Əgər XML fayllarını təhlil edirsinizsə (analiz edirsinizsə), `Milyard Gülüş/XML bomba` obyektinin genişləndirilməsi hücumu vasitəsilə obyektin genişlənməsinin qarşısını almaq üçün onun aktiv olmadığından əmin olun.
- [ ] Fayl yükləmələri üçün CDN istifadə edin.
- [ ] Böyük həcmdə məlumatlarla məşğul olursunuzsa, HTTP bloklanmasının qarşısını almaq üçün arxa planda işləmək və tez cavab vermək üçün mümkün qədər işçilərdən və növbələrdən istifadə edin.
- [ ] DEBUG rejimini söndürməyi unutmayın!
- [ ] Əgər varsa, icra olunmayan parçalardan istifadə edin.

## Çıxış

- [ ] `X-Content-Type-Options: nosniff` başlığını göndərin.
- [ ] `X-Frame-Options: rədd et` başlığını göndərin.
- [ ] `Məzmun-Təhlükəsizlik-Siyasəti: default-src 'heç biri'' başlığını göndərin.
- [ ] Barmaq izi başlıqlarını silin - `X-Powered-By`, `Server`, `X-AspNet-Version` və s.
- [ ] Sorğuya cavab olaraq `content-type` istifadə etməyə məcbur edin, əgər məlumatları `application/json` kimi qaytarsanız, `content-type` `application/json` olmalıdır.
- [ ] Nəticədə "etimadnamələr", "parollar" və ya "təhlükəsizlik nişanları" kimi həssas məlumatları göndərməyin.
- [ ] Əməliyyat başa çatdıqdan sonra müvafiq status kodunu qaytarın. (məsələn, `200 OK`, `400 Bad Sorğu`, `401 İcazəsiz`, `405 Metod İcazə Verilmir` və s.).

## CI & CD

- [ ] Vahid/inteqrasiya testi əhatə ölçüləri ilə dizayn və tətbiqinizi yoxlayın.
- [ ] Kodun nəzərdən keçirilməsi prosesindən istifadə edin və öz təsdiqinizə məhəl qoymayın.
- [ ] Kodunuzu aktivləşdirməzdən əvvəl xarici kitabxanalar və digər asılılıqlar daxil olmaqla xidmətlərinizin bütün komponentlərinin AntiVirus proqramı ilə statik olaraq skan edildiyinə əmin olun.
- [ ] Davamlı olaraq kodunuzda təhlükəsizlik testlərini (statik/dinamik analiz) keçirin.
- [ ] Məlum zəifliklər üçün asılılıqlarınızı (həm proqram təminatı, həm də əməliyyat sistemi) yoxlayın.
- [ ] Yerləşdirmələr üçün ehtiyat həlli dizayn edin.

## İzləmə

- [ ] Bütün xidmətlər və komponentlər üçün mərkəzi girişdən istifadə edin.
- [ ] Bütün trafikə, səhvlərə, sorğulara və cavablara nəzarət etmək üçün agentlərdən istifadə edin.
- [ ] SMS, Slack, E-poçt, Telegram, Kibana, Cloudwatch və s. xəbərdarlıqlardan istifadə edin.
- [ ] Kredit kartları, parollar, PIN-lər və s. Həssas məlumatları daxil etmədiyinizə əmin olun.
- [ ] API sorğularınızı və nümunələrinizi izləmək üçün IDS və/və ya IPS sistemindən istifadə edin.

---

## Əlavə resurslar:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP + JSON API qurmaq üçün faydalı resurslar toplusu.

---

# Töhfə

Bu deponu budaqlamaq, bəzi dəyişikliklər etmək və pull requests göndərməklə töhfə verməkdən çəkinməyin. Hər hansı bir sual üçün bizə bir e-poçt yazın: `team@shieldfy.io`.
