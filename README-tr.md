[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md)

# API Güvenlik Kontrol Listesi

API'nizi tasarlarken, test ederken ve yayınlarken en önemli güvenlik önlemlerinin kontrol listesi.

---

## Kimlik Doğrulama

- [ ] `Basic Auth` kullanmayın. Bunun yerine standardlaşmış kimlik doğrulama çözümlerini (örneğin [JWT](https://jwt.io/), [OAuth](https://oauth.net/) gibi) kullanmalısınız.
- [ ] `Kimlik doğrulama`, `token oluşturma`, `şifreleri kaydetme` için tekerleği yeniden icat etmeye çalışmayın. Standartları kullanın.
- [ ] `Deneme sayısını` sınırlayarak giriş hakkını kısıtlayın.
- [ ] Tüm hassas verilerde şifreleme kullanın.

### JWT (JSON Web Token)

- [ ] (`JWT Secret`) gibi rastgele, karmaşık ve zor bir anahtar kullanarak kaba kuvvet ile token çözmeyi olabildiğince zorlaştırın.
- [ ] Algoritmayı gelen veri üzerinden belirlemeyin. Arka uçta olmasını sağlayın. (`HS256` veya `RS256`).
- [ ] Token'in son kullanma tarihini (`TTL`, `RTTL`) olabildiğince kısa yapın.
- [ ] Hassas verilerinizi JWT payload içine koymayın, [Kolayca](https://jwt.io/#debugger-io) çözülebilir.

### OAuth

- [ ] Yalnızca beyaz listeye eklenen URL'lere izin vermek için sunucu tarafındaki `redirect_uri` bilgisini her zaman doğrulayın.
- [ ] Her zaman code değiştirmeyi deneyin token değiştirmeyi değil (`response_type=token` kullanımına izin vermeyin).
- [ ] OAuth kimlik doğrulama işlemi sırasında CSRF'yi önlemek için `state` parametresini rasgele hashleyerek kullanın.
- [ ] Varsayılan kapsamı tanımlayın ve her uygulama için kapsam parametrelerini doğrulayın.

## Erişim

- [ ] DDoS ya da kaba kuvvet saldırılarından korunmak için istekleri sınırlamalısınız.
- [ ] MITM (Man In The Middle Attack) saldırılarında korunmak için sunucu tarafında HTTPS kullanın.
- [ ] SSL Strip saldırılarından korunmak için `HSTS` header'ı SSL ile kullan.

## Girdi

- [ ] İşleme göre uygun HTTP yöntemini kullanın: `GET (okumak)`, `POST (oluşturmak)`, `PUT/PATCH (değiştirmek/güncellemk)`, ve `DELETE (bir kaydı silmek için)`, eğer istenen yöntem istenen kaynak için uygun değilse `405 Method Not Allowed` mesajı ile cevap verin.
- [ ] Accept header gelen `content-type` beklediğiniz ve izin verdiğiniz formatta olup olmadığını kontrol edin. (ör. `application/xml`, `application/json`, v.b.) Format uyuşmuyorsa `406 Not Acceptable` mesajı ile cevap verin.
- [ ] Gönderilen verileri doğrularken gelen verinin `content-type` değerini doğrulayın (ör. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, v.b.).
- [ ] Genel güvenlik açıklarını önlemek için kullanıcıdan gelen her veriyi doğrulayın (ör. `XSS`, `SQL-Injection`, `Remote Code Execution`, v.b.).
- [ ] URL'de hassas veriler (`credentials`, `Passwords`, `security tokens`, veya `API keys`) kullanmayın, ancak standart Authorization header kullanın.
- [ ] Önbelleklemeyi ve hız sınır politikalarını (ör. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) etkinleştirmek için ve API kaynaklarını dinamik olarak dağıtmak için bir API Gateway hizmeti kullanın.

## İşleme

- [ ] Kimlik doğrulama işleminin atlatılmasını önlemek için, tüm iştem uç noktalarının kimlik doğrulama arkasında korunup korunmadığını kontrol edin.
- [ ] Kullanıcı için kendi kaynak ID'si kullanılmasından kaçınılmalıdır. `/me/orders` yerine `/user/654321/orders` kullanın.
- [ ] Otomotik artan ID'ler kullanmayın. Yerine `UUID` kullanın.
- [ ] Eğer XML dosyarını (parse) ayrıştırıyorsanız, varlık ayrıştırmasını önlemek için etkin olmadığını doğrulayın `XXE` (XML external entity attack).
- [ ] Eğer XML dosyarını (parse) ayrıştırıyorsanız, `Billion Laughs/XML bomb` varlık genişletme saldırısı yoluyla,varlığın genişlemesinin önlemek için etkinleştirilmediğinden emin olun.
- [ ] Dosya yüklemeleri için bir CDN kullanın.
- [ ] Büyük miktarda veri ile uğraşıyorsanız, HTTP tıkanmasını engellemeyi önlemek için işleyici (Worker) ve kuyrukları (Queues) yapılarını arka planda işlem yapmak ve yanıtı hızlı bir şekilde yanıtlamak için mümkün oluğu kadar kullanın.
- [ ] DEBUG modunu kapatmayı unutmayın!.

## Çıktı

- [ ] `X-Content-Type-Options: nosniff` header'ı gönderin.
- [ ] `X-Frame-Options: deny` header'ı gönderin.
- [ ] `Content-Security-Policy: default-src 'none'` header'ı gönderin.
- [ ] Parmak izi header'larını kaldırın - `X-Powered-By`, `Server`, `X-AspNet-Version` v.b.
- [ ] İsteğe verilen cevapta `content-type` kullanmaya zorlayın, eğer veriyi `application/json` olarak döndürürseniz, `content-type` karşılığı `application/json` olmalı.
- [] `kimlik bilgileri` , `şifreleri` veya `güvenlik token'ları` gibi hassas verileri sonuç içinde göndermeyin.
- [ ] İşlem tamamlandıktan sonra uygun durum kodunu döndürün. (ör. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, v.b.).

## CI & CD

- [ ] unit/integration testi kapsamı ölçümleri ile tasarımınızı ve uygulamanızı denetleyin.
- [ ] Bir kod inceleme süreci kullanın ve kendi onayınızı dikkate almayın.
- [ ] Kodunuzu canlıya göndemreden önce harici kitaplıkları ve diğer bağımlılıklar da dahil olmak üzere hizmetlerinizin tüm bileşenlerinin AntiVirus yazılımıyla statik olarak tarandığından emin olun.
- [ ] Dağıtımlar için bir geriye dönme çözümü tasarlayın.

---

## Ek kaynaklar:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON API'leri oluşturmak için kullanışlı kaynakların bir koleksiyonu.

---

# katkı

Bu depoyu forklayarak, bazı değişiklikler yaparak ve pull requests göndererek katkıda bulunmaktan çekinmeyin. Herhangi bir sorunuz için bize bir e-posta bırakın: `team@shieldfy.io`.
