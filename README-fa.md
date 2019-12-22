[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md) | [Ελληνικά](./README-el.md) | [فارسی](./README-fa.md)
<div dir="rtl">

# چک لیست نکات امنیتی API

نکات مهم امنیتی در طراح و توسعه API


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
- [ ] برای `redirect_uri` در بک اند white list مسیر های مجاز را تعریف کنید.
- [ ] برای ارتباط بخش های مختلف از کد به جای تو کن استفاده کنید (از `response_type=token` استفاده نکنید).
- [ ] از پارامتر `state` با مقدار تصادفی برای جلوگیری از حملات CSRF استفاده کنید.
- [ ] مقدار پیش فرضی برای scope تعریف کنید, و مقدار scope را برای هر application جداگانه تعریف کنید.

## کنترل دسترسی(Access)
- [ ] محدودیت درخواست برای جلوگیری از حملات DDOS یا تکذیب سرور\حملات brute force یا حدس زدن تعریف کنید.
- [ ] از پروتکل HTTPS برای جلوگیری از حملات MITM(Man-in-the-middle) یا مرد میانی تعریف کنید.
- [ ] از `HSTS` در هدر درخواست ها به همراه SSL برای جلوگیری از حملات SSL Strip استفاده کنید.

## کنترل ورودی یا Input
- [ ] برای هر عملیات از متد متناسب در درخواست استفاده کنید: `GET (خواندن)`, `POST (ایجاد)`, `PUT/PATCH (جایگذاری/بروزرسانی)`, و `DELETE (برای حذف رکورد)`, و برای متد های نا متناسب از پاسخ `405 Method Not Allowed` استفاده کنید.
- [ ] مقدار `content-type` را بررسی و اعتبار سنجی کنید (برای مثال از `application/xml`, `application/json` استفاده کنید) و هرچه به غیر مقادیر مجاز را با `406 Not Acceptable` پاسخ دهید.
- [ ] همچنین برای `content-type` در متد  post از مقادیر (`application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` و ...) استفاده کنید.
- [ ] ورودی های سمت کاربر را برای جلوگیری از حملات متداول (مانند `XSS`, `SQL-Injection`, `Remote Code Execution`و ...) کنترل کنید.
- [ ] اطلاعات مهم مانند (`credentials`, `Passwords`, `security tokens`, یا `API keys`) در URL انتقال ندهید و به جای آن از هدر های استاندارد در درخواست استفاده کنید.
- [ ] از درگاه برای API برای cache و قوانین rate limit استفاده کنید (برای مثال `Quota`, `Spike Arrest`, یا `Concurrent Rate Limit`) و همچنین منابع مورد نیاز را به صورت پویا بارگذاری کنید.

## پردازش(Processing)
- [ ] تمام نقاظ یا endpoint هایی که برای دسترسی به آن نیاز به احراز هویت است را بررسی کنید.
- [ ] برای دریافت ID موجودیت ها از این شکل  `/me/orders` به جای این شکل `/user/654321/orders` استفاده کنید.
- [ ] از حالت auto-increment برای ID ها استفاده نکنید. از `UUID` استفاده کنید.
- [ ] از فایل های xml استفاده می کنید, کدهای دریافت و parse نمودن xml را برای جلوگیری از حملات `XXE` (XML external entity attack) کنترل کنید.
- [ ] اگر از فایل های xml استفاده می کنید, کدهای دریافت و parse نمودن xml را برای جلوگیری از حملات XXE (XML external entity attack) کنترل کنید..
- [ ] از CDN های برای بارگذاری فایل ها استفاده کنید.
- [ ] اگر با داده های بسیار زیادی سر و کار دارید, از Workers و Queues برای پردازش استفاده کنید تا پاسخ به درخواست ها سریع تر باشد.
- [ ] فراموش نکنید حالت DEBUG را غیر فعال کنید.

## خروجی(Output)
- [ ] پارامتر و مقدار `X-Content-Type-Options: nosniff` در هدر پاسخ ارسال کنید.
- [ ] پارامتر و مقدار `X-Frame-Options: deny` در هدر پاسخ ارسال کنید.
- [ ] پارامتر و مقدار `Content-Security-Policy: default-src 'none'` در هدر پاسخ ارسال کنید.
- [ ] اطلاعاتی که باعث شناسایی سیستم مورد استفاده می شود را حذف کنید مانند: `X-Powered-By`, `Server`, `X-AspNet-Version`.
- [ ] مقدار `content-type` در پاسخ را حتما تعیین کنید, اگر نوع خروجی  `application/json` است پس نوع پاسخ یا  `content-type` هم باید `application/json` باشد.
- [ ] اطلاعات مهم مانند `credentials`, `Passwords`, یا `security tokens` در پاسخ ارسال نکنید.
- [ ] کد وضیعت پاسخ ارسال شده را تعیین کنید. (مانند `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`.).

## یکپارچه سازی و تحویل مستمر(CI & CD)
- [ ] بررسی و ممیزی امنیتی برای تست های unit/integration .
- [ ] استفاده از code review در روند توسعه جدا از بررسی کد توسط شخص توسعه دهنده.
- [ ] تمام اجزای مورد استفاده به صورت مستقیم و غیر مستقیم مانند کتابخانه ها و دیگر وابستگی های پروژه قبل از استفاده در محیط production باید با استفاده از AV تست شوند.
- [ ] طراحی راه حل هایی برای rollback در محیط توسعه در نظر گرفته شود.


---

## همچنین مشاهده کنید:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - مجموعه ای از منابع مفید برای ساخت سرویس های  RESTful HTTP+JSON APIs.


---

# مشارکت کنید
برای مشارکت آزاد کافی است این مخزن را FORK کنید, تغییرات خود را اعمال کنید, و آن را به عنوان pull request ارسال کنید. در صورت هرگونه سوالی با `team@shieldfy.io` درارتباط باشید.
