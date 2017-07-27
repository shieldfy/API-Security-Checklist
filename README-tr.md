[中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-jp.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md)

# API Güvenlik Kontrol Listesi
API'nizi tasarlarken, test ederken ve yayınlarken en önemli güvenlik önlemlerinin kontrol listesi.

------------------------------------------------------------------------------
## Authentication (Kimlik doğrulama) 
- [ ] `Basic Auth` kullanmayın. Standard authentication kullanın (ör. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] `Authentication`, `token generation`, `password storage` için tekerleği yeniden icat etmeyin. Standartları kullanın.
- [ ] `Max Retry` kullanarak giriş hakkını sınırlayın.
- [ ] Tüm hassas verilere şifreleme kullanın.

### JWT (JSON Web Token)
- [ ] Brute forcing yönetimi ile oluşturulan token'in çözülmemesi için (`JWT Secret`) gibi rasgele, karmaşık ve zor bir anahtar kullanın.
- [ ] Don't extract the algorithm from the payload. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] Token'in son kullanma tarihini (`TTL`, `RTTL`) olabildiğince kısa yapın.
- [ ] Hassas verilerinizi JWT payload'a koymayın, decode edilebilir. [Basit olarak](https://jwt.io/#debugger-io).

### OAuth
- [ ] Yalnızca beyaz listeye eklenen URL'lere izin vermek için sunucu tarafındaki `redirect_uri` daima doğrulayın.
- [ ] Always try to exchange for code and not tokens (don't allow `response_type=token`).
- [ ] Use `state` parameter with a random hash to prevent CSRF on the OAuth authentication process.
- [ ] Define the default scope, and validate scope parameters for each application.

## Access
- [ ] DDoS / brute-force saldırılarından korunmak için istekleri sınırlamalısınız.
- [ ] MITM (Man In The Middle Attack) korunmak için sunucu tarafında HTTPS kullanın.
- [ ] SSL Strip saldırılarından korunmak için `HSTS` header'ı SSL ile kullan.

## Input
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (ör. `application/xml`, `application/json`, v.b.) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (ör. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, v.b.).
- [ ] Validate User input to avoid common vulnerabilities (ör. `XSS`, `SQL-Injection`, `Remote Code Execution`, v.b.).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (ör. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) and deploy APIs resources dynamically.

## Processing 
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource ID should be avoided. Use `/me/orders` instead of `/user/654321/orders`.
- [ ] Don't auto-increment IDs. Use `UUID` instead.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Dosya yüklemeleri için bir CDN kullanın.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
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


------------------------------------------------------------------------------

# Destek
Bu depoyu forklayarak, bazı değişiklikler yaparak ve pull requests göndererek katkıda bulunmaktan çekinmeyin. Herhangi bir sorunuz için bize bir e-posta bırakın: `team@shieldfy.io`.
