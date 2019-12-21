[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md) | [Ελληνικά](./README-el.md) | [فارسی](./README-fa.md)
<div dir="rtl">

# چک لیست نکات امنیتی API

نکات مهم امنیتی در طراح و تست API


---

## احراز هویت(Authentication)
- [ ] هیچگاه از `Basic Auth` استفاده نکنید. از روش های استاندارد کنید (برای مثال از [JWT](https://jwt.io/) و یا [OAuth](https://oauth.net/) استفاده کنید).
- [ ] سعی در ایجاد روش های جدید و ساخت دوباره چرخ در `Authentication`, `token generation`, `password storage` نکنید.
- [ ] از `Max Retry` و قابلیت قفل شدن اکانت استفاده کنید .
- [ ] از رمز نگاری برای رمز کردن تمام اطلاعات مهم و حساس استفاده کنید.

### استفاده از JWT (JSON Web Token)
- [ ] از کلید های پیچیده در (`JWT Secret`) استفاده کنید تا حدس زدن آن ها دشوار تر باشد.
- [ ] در هیچ صورت از الگوریتم های رمز نگاری در فرانت اند استفاده نکنید. باید از الگوریتم های  (`HS256` یا `RS256`) در بک اند استفاده کنید.
- [ ] برای توکن با استفاده از (`TTL`, `RTTL`) مدت زمان کوتاه اعتبار تعریف کنید.
- [ ] اطلاعات مهم را داخل JWT نگهداری نکنید, چون می توان  [به راحتی ](https://jwt.io/#debugger-io) آن را رمز گشایی کرد.

### استفاده از OAuth
- [ ] همیشه برای `redirect_uri` در بک اند white list مسیر های مجاز را تعریف کنید.
- [ ] Always try to exchange for code and not tokens (don't allow `response_type=token`).
- [ ] Use `state` parameter with a random hash to prevent CSRF on the OAuth authentication process.
- [ ] Define the default scope, and validate scope parameters for each application.

## Access
- [ ] Limit requests (Throttling) to avoid DDoS / brute-force attacks.
- [ ] Use HTTPS on server side to avoid MITM (Man in the Middle Attack).
- [ ] Use `HSTS` header with SSL to avoid SSL Strip attack.

## Input
- [ ] Use the proper HTTP method according to the operation: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, and `DELETE (to delete a record)`, and respond with `405 Method Not Allowed` if the requested method isn't appropriate for the requested resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json`, etc.) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, etc.).
- [ ] Validate user input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution`, etc.).
- [ ] Don't use any sensitive data (`credentials`, `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use an API Gateway service to enable caching, Rate Limit policies (e.g. `Quota`, `Spike Arrest`, or `Concurrent Rate Limit`) and deploy APIs resources dynamically.

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
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version`, etc.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `Passwords`, or `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, etc.).

## CI & CD
- [ ] Audit your design and implementation with unit/integration tests coverage.
- [ ] Use a code review process and disregard self-approval.
- [ ] Ensure that all components of your services are statically scanned by AV software before pushing to production, including vendor libraries and other dependencies.
- [ ] Design a rollback solution for deployments.


---

## See also:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - A collection of useful resources for building RESTful HTTP+JSON APIs.


---

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
