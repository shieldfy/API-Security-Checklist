[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# API Güvenlik Kontrol Listesi
API'nizi tasarlarken, test ederken ve yayınlarken en önemli güvenlik önlemlerinin kontrol listesi.


---

## Authentication (Kimlik doğrulama)
- [ ] `Basic Auth` kullanmayın. Standard authentication kullanın (ör. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] `Authentication`, `token generation`, `password storage` için tekerleği yeniden icat etmeyin. Standartları kullanın.
- [ ] `Max Retry` kullanarak giriş hakkını sınırlayın.
- [ ] Tüm hassas verilere şifreleme kullanın.

### JWT (JSON Web Token)
- [ ] Brute forcing yönetimi ile oluşturulan token'in çözülmemesi için (`JWT Secret`) gibi rasgele, karmaşık ve zor bir anahtar kullanın.
- [ ] Algoritmayı payload üzerinden çekmeyin. Arka planda içinde kullanın. (`HS256` veya `RS256`).
- [ ] Token'in son kullanma tarihini (`TTL`, `RTTL`) olabildiğince kısa yapın.
- [ ] Hassas verilerinizi JWT payload'a koymayın, decode edilebilir. [Basit olarak](https://jwt.io/#debugger-io).

### OAuth
- [ ] Yalnızca beyaz listeye eklenen URL'lere izin vermek için sunucu tarafındaki `redirect_uri` daima doğrulayın.
- [ ] Daima kodları değiştirmeyi deneyin tokenları değil (`response_type=token` izin vermeyin).
- [ ] OAuth kimlik doğrulama işlemi sırasında CSRF'yi önlemek için `state` parametresini rasgele bir hashleyerek kullanın.
- [ ] Varsayılan kapsamı tanımlayın ve her uygulama için kapsam parametrelerini doğrulayın.

## Access
- [ ] DDoS / brute-force saldırılarından korunmak için istekleri sınırlamalısınız.
- [ ] MITM (Man In The Middle Attack) korunmak için sunucu tarafında HTTPS kullanın.
- [ ] SSL Strip saldırılarından korunmak için `HSTS` header'ı SSL ile kullan.

## Input
- [ ] İşleme göre uygun HTTP yöntemini kullanın: `GET (okumak)`, `POST (oluşturmak)`, `PUT/PATCH (değiştirmek/güncellemk)`, ve `DELETE (bir kaydı silmek için)`, eğer istenen yöntem istenen kaynak için uygun değilse `405 Method Not Allowed` mesajı ile cevap verin.
- [ ] Accept header gelen `content-type` beklediğin ve izin verdiğin formatta olup olmadığını kontrol et. (ör. `application/xml`, `application/json`, v.b.) Format uyuşmuyorsa `406 Not Acceptable` mesajı ile cevap verin.
- [ ] Gönderilen verileri doğrularken gelen verinin `content-type` de doğrulayın (ör. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, v.b.).
- [ ] Genel güvenlik açıklarını önlemek için Kullanıcı girişini doğrulayın (ör. `XSS`, `SQL-Injection`, `Remote Code Execution`, v.b.).
- [ ] URL'de hassas veriler (`credentials`, `Passwords`, `security tokens`, veya `API keys`) kullanmayın, ancak standart Authorization header kullanın.
- [ ] Önbelleklemeyi etkinleştirmek, hız sınır politikalarını (ör. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) ve API kaynaklarını dinamik olarak dağıtmak için bir API Gateway hizmeti kullanın.

## Processing
- [ ] Authentication işleminin sonlandırılmasını önlemek için, tüm bitiş noktalarının Authentication arkasında korunup korunmadığını kontrol edin.
- [ ] Kullanıcı kendi kaynak ID'sinden kaçınmalıdır. `/me/orders` yerine `/user/654321/orders` kullanmalıdır.
- [ ] Otomotik artan ID'ler kullanmayın. Yerine `UUID` kullanın.
- [ ] Eğer XML dosyarını (parse) ayrıştırıyorsanız, varlık ayrıştırmasını önlemek için etkin olmadığını doğrulayın `XXE` (XML external entity attack).
- [ ] Eğer XML dosyarını (parse) ayrıştırıyorsanız, `Billion Laughs/XML bomb` varlık genişletme saldırısı yoluyla,varlığın genişlemesinin önlemek için etkinleştirilmediğinden emin olun .
- [ ] Dosya yüklemeleri için bir CDN kullanın.
- [ ] Büyük miktarda veri ile uğraşıyorsanız, HTTP engellemeyi önlemek için İşçi ve Kuyrukları arka planda olabildiğince işlem yapmak ve yanıtı hızlı bir şekilde yanıtlamak için kullanın.
- [ ] DEBUG modunu kapatmayı unutmayın!.

## Output
- [ ] `X-Content-Type-Options: nosniff` header'ı gönder.
- [ ] `X-Frame-Options: deny` header'ı gönder.
- [ ] `Content-Security-Policy: default-src 'none'` header'ı gönder.
- [ ] Parmak izi başlıklarını kaldırın - `X-Powered-By`, `Server`, `X-AspNet-Version` v.b.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Hassas verilerinizi geri göndermeyin `credentials`, `Passwords`, `security tokens`.
- [ ] İşlem tamamlandıktan sonra uygun durum kodunu döndürür. (ör. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, v.b.).

## CI & CD
- [ ] unit/integration testi kapsamı ile tasarımınızı ve uygulamanızı denetleyin.
- [ ] Bir kod inceleme işlemi kullanın ve kendi onayınızı dikkate almayın.
- [ ] Vendor kitaplıkları ve diğer bağımlılıklar da dahil olmak üzere, oluşturmaya başlamadan önce hizmetlerinizin tüm bileşenlerinin AntiVirus yazılımıyla statik olarak tarandığından emin olun.
- [ ] Dağıtımlar için bir geri yükleme çözümü tasarlayın.


---

## Ayrıca bakınız:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON API'leri oluşturmak için kullanışlı kaynakların bir koleksiyonu.


---

# Destek
Bu depoyu forklayarak, bazı değişiklikler yaparak ve pull requests göndererek katkıda bulunmaktan çekinmeyin. Herhangi bir sorunuz için bize bir e-posta bırakın: `team@shieldfy.io`.
