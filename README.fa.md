[繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](.README-mk.md) | [ລາວ](./README-lo.md)

# API Security Checklist
Checklist of the most important security countermeasures when designing, testing, and releasing your API.


# API لیستی از اقدامات امنیتی برای
لیستی از مهم ترین اقدامات امنیتی برای زمان طراحی، آزمایش و منتشر کردن ای پی آی شما که باید رعایت کنید.

---

## اعتبار سنجی
- [ ] به اعتبار سنجی ساده قانع نشید حتما سخت گیرانه و پیشرفته اعتبار سنجی کنید  (e.g. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] های استاندارد و موجود استفاده کنید `Authentication`, `token generation`,  `password storage ` چرخی که هست رو از اول اختراع نکنید و از   
- [ ] برای ورود محدودیت و سقفی تعین کنید، مثلا کاربر بعد سه باز تلاش نا موفق بلاک شود.
- [ ] روی اطلاعات مهم و حساس حتما از رمز گذاری استفاده کنید.

### JWT (JSON Web Token)
- [ ] خیلی پیچیده و غیر قابل شکستن استفاده کنید token برای ایجاد (`JWT Secret`) از یک کلید تصادفی و پیچیده یا  
- [ ] Don't extract the algorithm from the payload. Force the algorithm in the backend (`HS256` or `RS256`).
- [ ] برای توکن های خود تاریخ انقضا کوتاه تعریف کنید (`TTL`, `RTTL`) با استفاده از    
- [ ] ذخیره نکنید چون به راحتی رمزگشایی میشوند JWT payload هیچ وقت اطلاعات حساس را در  [easily](https://jwt.io/#debugger-io).



### OAuth
- [ ] Always validate `redirect_uri` server-side to allow only whitelisted URLs.
- [ ] Always try to exchange for code and not tokens (don't allow `response_type=token`).
- [ ] Use `state` parameter with a random hash to prevent CSRF on the OAuth authentication process.
- [ ] Define the default scope, and validate scope parameters for each application.

## Access
- [ ] Limit requests (Throttling) to avoid DDoS / brute-force attacks.
- [ ] Use HTTPS on server side to avoid MITM (Man In The Middle Attack).
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attack.

## Input
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json`, etc) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc).
- [ ] Validate User input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (e.g. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) and deploy APIs resources dynamically.

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
- [ ] رو ارسال کنید `X-Content-Type-Options: nosniff`هدر 
- [ ] رو ارسال کنید `X-Frame-Options: deny`هدر  
- [ ] رو هم ارسال کنید `Content-Security-Policy: default-src 'none'`هدر
- [ ] رو حذف و ارسال نکنید تا اطلاعات نرم افزاری شما مخفی بماند `X-Powered-By`, `Server`, `X-AspNet-Version` هدر های 
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
