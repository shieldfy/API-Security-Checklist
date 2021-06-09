[English](./README-en.md) | [繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md) | [Ελληνικά](./README-el.md) | [فارسی](./README-fa.md)
<div dir="rtl">


# چک لیست امنیت API ها
در این صفحه چک لیستی از موارد امنیتی API ها در هنگام طراحی، توسعه، تست و انتشار بررسی شده است.



---

## Authentication
- [ ] از احراز هویت پایه (`Basic Auth`) استفاده نکنید، از روش های استاندارد نظیر [JWT](https://jwt.io/), [OAuth](https://oauth.net/) استفاده کنید.
- [ ] چرخ را دوباره اختراع نکنید، از روش های استاندارد احراز هویت (`Authentication`)، تولید توکن (`Token Generation`) و ذخیره کلمه عبور (`Password Storage`) استفاده کنید.
- [ ] از روش های `Max Retry` و محدود کردن تعداد دفعات ورود استفاده کنید.
- [ ] برای تمام داده های حساس از رمزنگاری استفاده کنید. 

## JWT (Json Web Token)
- [ ] از یک کلید تصادفی و پیچیده برای `JWT Secret` استفاده کنید تا حمله `Brute Force` بر روی توکن بسیار سخت شود. 
- [ ] الگوریتم را از طریقه `Payload` باز نکنید، الگوریتم را در سمت `Backend` نگه دارید. (روش‌های رمزنگاری `HS256` و `RS256` پیشنهاد می‌شود)
- [ ] زمان عمر `Token` را (`TTL`, `RTTL`) تا حد امکان کوتاه تنظیم کنید.
- [ ] اطلاعات حساس را در `JWT Payload` ذخیره نکنید، در این حالت [به راحتی](https://jwt.io/#debugger-io) قابل شکستن است.

## OAuth
- [ ] همیشه `redirect_uri` را در سمت سرور اعتبارسنجی کنید تا تنها آدرس های Whitelist دسترسی داشته باشند.
- [ ] همیشه سعی کنید بجای Tokenها، code ها را تبادل کنید. هرگز اجازه ندهید `response_type=token` باشد.
- [ ] از پارامتر `state` با یک هش تصادفی استفاده کنید تا جلوی حمله CSRF در فرآیند احرازهویت OAuth گرفته شود.
- [ ] محدوده پیش فرض را مشخص کنید و برای هر برنامه، پارامترهای این محدوده را اعتبارسنجی کنید.

## Access
- [ ] درخواست‌ها را محدود کنید (Throttling) تا جلوی حمله DDoS / Brute-Force گرفته شود.
- [ ] در سمت سرور از HTTPS استفاده کنید تا جلوی حمله مرد میانی (Man in the Middle) گرفته شود.
- [ ] از هدر `HSTS` همراه با SSL استفاده کنید تا جلوی حمله Strip گرفته شود.

## Input
- [ ] باتوجه به نوع عملیات، نوع هدر بسته HTTP را مشخص کنید. از متد `GET` برای خواندن، متد `POST` برای ساختن، متد `PUT/PATCH` برای جایگزینی یا آپدیت و متد `DELETE` برای پاک کردن استفاده کنید و درخواست هایی که متد آن‌ها مناسب نمی‌باشد را با کد  `405 Method Not Allowed` پاسخ دهید.
- [ ] مقدار پارامتر `content-type` هدر بسته‌ها را اعتبارسنجی کنید تا تنها به نوع های مشخص `application/xml` یا `application/json` و ... پاسخ دهد و درخواست های غیرمجاز را با ` 406 Not Acceptable` پاسخ دهید.
- [ ] مقدار پارامتر `content-type` را در متد `POST` اعتبارسنجی کنید تا مقادیری نظیر `application/x-www-form-urlencoded`، `multipart/form-data` یا `application/json` و ... باشد.
- [ ] مقدار ورودی ارسالی کاربر را اعتبارسنجی کنید تا از حملاتی نظیر `XSS`، `SQL-Injection`، `Remote Code Execution` و ... جلوگیری شود.
- [ ] از اطلاعات حساس نظیر `Crendetials`، `Passwords`، `Security Tokens` یا `API Keys` در URL استفاده نکنید و از یک `Authorization Header` استاندارد استفاده کنید.
- [ ] از یک سرویس API Gateway استفاده کنید تا عملیاتی نظیر Caching یا سیاست های ایجاد محدودیت نظیر `Quota`، `Spike Arrest` یا `Concurrent Rate Limit` را انجام دهید و به صورت پویا منابع API ها را deploy کنید.

## Processing
- [ ] تمام Endpoint های دارای عملیات احرازهویت را بررسی کنید تا فرآیند احرازهویت شکسته نشود.
- [ ] مقدار ID کاربران نباید نمایش داده شود. برای مثال بجای `/user/654321/orders` از `/me/orders` استفاده کنید.
- [ ] از مقادیر ID به صورت Auto-increment استفاده نکنید و بجای آن از `UUID` استفاده کنید.
- [ ] اگر XML Parsing انجام می‌دهید، اطمینان حاصل کنید عملیات Parsing توسط آسیب پذیری `XXE` (XML external entity attack) قابل نفوذ نباشد. 
- [ ] اگر XML Parsing انجام می‌دهید، اطمینان حاصل کنید حمله هایی نظیر `Billion Laughs/XML bomb` صورت نگیرد.
- [ ] برای آپلود فایل‌ها از CDN یا شبکه توزیع محتوا استفاده کنید.
- [ ] اگر روی مقدار عظیمی از داده عملیات محاسباتی انجام می‌دهید، از Worker ها Queue ها استفاده کنید تا عملیات را تا حد امکان در پس‌زمینه انجام دهید و پاسخ را به سرعت برگردانید و از HTTP Blocking جلوگیری کنید.
- [ ] هرگز فراموش نکنید حالت DEBUG را OFF کنید.

## Output
- [ ] هدر `X-Content-Type-Options: nosniff` را ارسال کنید.
- [ ] هدر `X-Frame-Options: deny` را ارسال کنید.
- [ ] هدر `Content-Security-Policy: default-src ‘none’` را ارسال کنید.
- [ ] هدرهای اثرانگشتی نظیر `X-Powered-By`، `Server`، `X-AspNet-Version` و ... را ارسال نکنید.
- [ ] حتما `content-type` برای پاسخ‌های خود درنظر بگیرید، برای مثال اگر پاسخ از نوع `application/json` است آنگاه `content-type` باید برابر `application/json` باشد.
- [ ] اطلاعات حساس نظیر `Credentials`، `Passwords` و `Security Token` ها را ارسال نکنید.
- [ ] باتوجه به نوع عملیات، `Status Code` مناسب برگردانید برای مثال `200 OK`، `400 Bad Request`، `401 Unauthorized`، `405 Method Not Allowed` و ...

## CI & CD
- [ ] طراحی و پیاده سازی را باتوجه به پوشش Unit/Integration Test بازرسی کنید.
- [ ] از یک روند Code Review استفاده کنید و از Self-Approval خودداری کنید.
- [ ] اطمینان حاصل کنید تمام اجزای سرویس شما قبل از Push بر روی حالت Production توسط یک آنتی ویروس اسکن شده اند، از جمله کتابخانه ها و سایر وابستگی‌ها
- [ ] یک حالت Rollback برای Deployment های خود طراحی کنید.

---

## همچنین ببینید
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - مجموعه ای از ابزارهای مفید برای ساخت RESTful HTTP+JSON APIs

---

# مشارکت
مشارکت در این منبع آزاد است و می‌توانید این منبع را fork کنید، تغییراتی اعمال کنید و Pull Request ثبت کنید. برای هرگونه سوالی به `team@shieldfy.io` ایمیل ارسال کنید.
</div>
