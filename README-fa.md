[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

<div dir="rtl">

# چک‌لیست امنیتی API

چک‌لیستی از مهم‌ترین کارهای لازم برای حفظ امنیت در زمان طراحی، تست و انتشار API.

---

## احراز هویت

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از `Basic Auth` یا همان `اصالت‌سنجی برای دسترسی‌های اولیه` استفاده نکنید. به جای آن از روش‌های استاندارد احراز هویت استفاده کنید (مثلا [JWT](https://jwt.io/) یا [OAuth](https://oauth.net/)).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای کارهایی مثل `احراز هویت`، `تولید توکن` و `ذخیره پسوورد` چرخ را دوباره اختراع نکنید. از استانداردها استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای لاگین محدودیت‌های `تعداد ماکسیمم تلاش مجدد` و تعداد دفعات ورود را قرار بدید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همه‌ی داده‌های حساس را رمزگذاری کنید.

### JWT (JSON Web Token)

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک کلید پیچیده‌ی تصادفی برای `JWT Secret` استفاده کنید تا حمله‌ی بروت‌فورس به توکن بسیار سخت باشد.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;الگوریتم را از هدر استخراج نکنید. در بک‌اند الگوریتم را تحمیل کنید (`HS256` یا `RS256`).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;انقضای توکن (`TTL` یا `RTTL`) را تا حد ممکن کوتاه کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اطلاعات حساس را در پی‌لود JWT ذخیره نکنید چون [به راحتی](https://jwt.io/#debugger-io) قابل رمزگشایی است.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از ذخیره بیش از حد داده ها خودداری کنید. JWT معمولاً در هدر به اشتراک گذاشته می شود و محدودیت اندازه دارند.

## دسترسی

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;رکوئست‌ها را محدود کنید (Throttling) تا از حملات DDos یا بروت‌فورس جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;در سمت سرور از HTTPS استفاده کنید تا از حملات مرد میانی جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از هدر `HSTS` استفاده کنید تا از حمله‌ی SSL Strip جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لیست های دایرکتوری را خاموش کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای APIهای خصوصی، فقط از IPها/میزبانهای لیست سفید اجازه دسترسی داشته باشید.

## Authorization

### OAuth

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همیشه `redirect_uri` را در سمت سرور اعتبارسنجی کنید تا تنها به URLهای مجاز اجازه داده شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همیشه تلاش کنید تا code را به جای token تبادل کنید (اجازه `response_type=token` را ندهید).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از پارامتر `state` با یک هش تصادفی استفاده کنید تا از CSRF روی پروسه‌ی احراز هویت OAuth جلوگیری کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار scope پیش‌فرض را تعریف کنید و پارامترهای scope را برای هر اپلیکیشن اعتبارسنجی کنید.

## ورودی

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از متد HTTP مناسب با توجه به نوع عملیات استفاده کنید: `GET` برای خواندن، `POST` برای ایجاد کردن، `PUT/PATCH` برای جایگزین یا بروزرسانی و `DELETE` برای حذف یک رکورد، و در صورتی‌که متد درخواستی برای منبع درخواست‌شده مناسب نباشد با `405 Method Not Allowed` پاسخ بدهید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` را در هدر Accept رکوئست (مذاکره محتوا یا Content Negotiation) اعتبارسنجی کنید تا فقط به فرمت‌های مورد پشتیبانی اجازه داده شود (مثلا `application/xml`، `application/json` و ...). و در صورت عدم تطابق با یک پاسخ `406 Not Acceptable` پاسخ دهید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` در داده‌ی پست‌شده را اعتبارسنجی کنید (مثلا `application/x-www-form-urlencoded`، `multipart/form-data`، `application/json` و ...).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ورودی کاربر را اعتبارسنجی کنید تا از آسیب‌پذیری‌های معمول جلوگیری شود (مثلا `XSS`، `SQL-Injection` و `Remote Code Execution`).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هیچ داده‌ی حساسی مثل (داده‌های اعتبارسنجی، پسوورد‌ها، توکن‌های امنیتی یا کلید‌های API) را داخل URL قرار ندهید و از هدر Authorization استاندارد استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;فقط از رمزگذاری سمت سرور استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک سرویس API Gateway استفاده کنید تا کش‌کردن و سیاست‌های Rate Limit (مثلا `Quota`، `Spike Arrest` یا `Concurrent Rate Limit`) فعال شوند و منابع APIها را به صورت داینامیک دپلوی کنید.

## پردازش

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;چک کنید که تمامی endpointها توسط احراز هویت محافظت شوند تا از پروسه‌ی احراز هویت ناقص جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از استفاده از ID ریسورس خود کاربر اجتناب کنید. به جای `user/654321/orders` از `/me/orders` استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از IDهای auto-increment استفاده نکنید. به جای آن از `UUID` استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر فایل‌های XML را parse می‌کنید مطمئن شوید تا entity parsing غیرفعال باشد تا از `XXE` (XML External entity attack) جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر فایل‌های XML، YAML یا هر زبان دیگری را با استفاده از anchor ها و ref ها parse می‌کنید، مطمئن شوید تا entity expansion غیرفعال باشد تا از `Billion Laughs/XML bomb` توسط exponential entity expansion attack جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک CDN برای آپلودهای فایل استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر با مقادیر بسیار حجیمی از داده سر و کار دارید، از Workerها و Queueها استفاده کنید تا حد الامکان پردازش در بک‌گراند انجام شود و سریع پاسخ را برگردانید تا از HTTP Blocking جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;خاموش کردن حالت DEBUG را فراموش نکنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;در صورت وجود از پشته های غیر قابل اجرا استفاده کنید.

## خروجی

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `X-Content-Type-Options: nosniff` را ارسال کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `X-Frame-Options: deny` را ارسال کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `'Content-Security-Policy: default-src 'none` را ارسال کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدرهایی که به نوعی اثرانگشت برجای می‌گذارند را حذف کنید، مثلا `X-Powered-By`، `Server` و ‍`X-AspNet-Version`.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` را برای پاسخ اجباری کنید. اگر `application/json` برمیگردانید، پس `content-type` پاسخ، `application/json` است.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اطلاعات حساس مثل `داده‌های اعتبارسنجی`، `رمز های عبور` و `توکن‌های امنیتی` را برنگردانید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;با توجه به عملیات انجام‌شده، status code مناسب را برگردانِد. مثلا `200 OK`، `400 Bad Request`، `401 Unauthorized` و `405 Method Not Allowed`.

## CI & CD

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;طراحی و پیاده سازی خودتان را با پوشش تست‌های unit/integration بازرسی کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک پروسه‌ی مرور کد استفاده کنید و خود-تاییدی را نادیده بگیرید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مطمئن شوید تا تمامی اجزای سرویس‌هایتان، شامل کتابخانه‌های استفاده‌شده و دیگر وابستگی‌ها، قبل از انتشار در حالت production، به طور ایستا توسط نرم‌افزارهای آنتی‌ویروس اسکن شده‌اند.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;به صورت پیوسته روی کدتان تست‌های امنیتی (آنالیز ایستا و پویا)، اجرا کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;وابستگی‌هایتان (نرم افزار و سیستم عامل، هردو) را برای آسیب‌پذیری‌های شناخته شده، چک کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای دپلوی‌هایتان، یک راه‌حل با قابلیت عقبگرد (rollback) طراحی کنید.

## Monitoring

- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از لاگین های متمرکز برای همه سرویس ها و مؤلفه ها استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از agent ها برای مانیتور همه ترافیک, خطاها, درخواست‌ها و پاسخ‌ها استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از alert ها برای اس ام اس, Slack, ایمیل, Telegram, Kibana, Cloudwatch و غیره استفاده کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اطمینان حاصل کنید که هیچ گونه داده حساسی مانند کارت های اعتباری، رمزهای عبور، پین ها و غیره را ثبت نمی کنید.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک سیستم IDS و/یا IPS برای مانیتور درخواست ها API و نمونه های خود استفاده کنید.

---

## نگاهی بیاندازید به:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - یک مجموعه از منابع مفید برای ساختن APIهای RESTful با HTTP و JSON -

---

# مشارکت

برای همکاری و کمک می‌توانید به راحتی این مخزن را fork کنید، تغییرات مورد نظرت را اعمال کنید و یک pull request ثب کنید. اگر سوالی داشتید به آدرس `team@shieldfy.io` ایمیل بزنید.
</div>
