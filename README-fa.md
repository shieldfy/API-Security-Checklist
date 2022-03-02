[繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md) | [Ελληνικά](./README-el.md) | [മലയാളം](./README-ml.md)

<div dir="rtl">

# چک‌لیست امنیتی API
چک‌لیستی از مهم‌ترین کارهای لازم برای حفظ امنیت در زمان طراحی، تست و انتشار API.

---

## احراز هویت
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از `Basic Auth` یا همان `اصالت‌سنجی برای دسترسی‌های اولیه` استفاده نکن. به جای آن از روش‌های استاندارد احراز هویت استفاده کن (مثلا [JWT](https://jwt.io/) یا [OAuth](https://oauth.net/)).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای کارهایی مثل `احراز هویت`، `تولید توکن` و `ذخیره پسوورد` چرخ را دوباره اختراع نکن. از استانداردها استفاده کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای لاگین محدودیت‌های `تعداد ماکسیمم تلاش مجدد`  و تعداد دفعات ورود را قرار بده.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همه‌ی داده‌های حساس را رمزگذاری کن.

### JWT (JSON Web Token)
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک کلید پیچیده‌ی تصادفی برای `JWT Secret` استفاده کن تا حمله‌ی بروت‌فورس به توکن بسیار سخت باشد.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;الگوریتم را از هدر استخراج نکن. در بک‌اند الگوریتم را تحمیل کن (`HS256` یا `RS256`).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;انقضای توکن (`TTL` یا `RTTL`) را تا حد ممکن کوتاه کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اطلاعات حساس را در پی‌لود JWT ذخیره نکن چون [به راحتی](https://jwt.io/#debugger-io) قابل رمزگشایی است.

### OAuth
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همیشه `redirect_uri` را در سمت سرور اعتبارسنجی کن تا تنها به URLهای مجاز اجازه داده شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;همیشه تلاش کن تا code را به جای token تبادل کنی (اجازه `response_type=token` را نده).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از پارامتر `state` با یک هش تصادفی استفاده کن تا از CSRF روی پروسه‌ی احراز هویت OAuth جلوگیری کنی.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار scope پیش‌فرض را تعریف کن و پارامترهای scope را برای هر اپلیکیشن اعتبارسنجی کن.

## دسترسی
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;رکوئست‌ها را محدود کن (Throttling) تا از حملات DDos یا بروت‌فورس جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;در سمت سرور از HTTPS استفاده کن تا از حملات مرد میانی جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از هدر `HSTS` استفاده کن تا از حمله‌ی SSL Strip جلوگیری شود.

## ورودی
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از متد HTTP مناسب با توجه به نوع عملیات استفاده کن: `GET` برای خواندن، `POST` برای ایجاد کردن، `PUT/PATCH` برای جایگزین یا بروزرسانی و `DELETE` برای حذف یک رکورد، و در صورتیکه متد درخواستی برای منبع درخواست‌شده مناسب نیست با `405 Method Not Allowed` پاسخ بده.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` را در هدر Accept رکوئست (مذاکره محتوا یا Content Negotiation) اعتبارسنجی کن تا فقط به فرمت‌های مورد پشتیبانی اجازه داده شود (مثلا `application/xml`، `application/json` و ...).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` در داده‌ی پست‌شده را اعتبارسنجی کن (مثلا `application/x-www-form-urlencoded`، `multipart/form-data`، `application/json` و ...).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ورودی کاربر را اعتبارسنجی کن تا از آسیب‌پذیری‌های معمول جلوگیری شود (مثلا `XSS`، `SQL-Injection` و `Remote Code Execution`). 
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هیچ داده‌ی حساسی مثل (داده‌های اعتبارسنجی، پسوورد‌ها، توکن‌های امنیتی یا کلید‌های API) را داخل URL قرار نده و از هدر Authorization استاندارد استفاده کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک سرویس API Gateway استفاده کن تا کش‌کردن و سیاست‌های Rate Limit (مثلا `Quota`، `Spike Arrest` یا `Concurrent Rate Limit`) فعال شوند و منابع APIها را به صورت داینامیک دپلوی کن.

## پردازش
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;چک کن که تمامی endpointها توسط احراز هویت محافظت شوند تا از شکستن پروسه‌ی احراز هویت جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از استفاده از ID ریسورس خود کاربر اجتناب کن. به جای `user/654321/orders` از `/me/orders` استفاده کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از IDهای auto-increment استفاده نکن. به جای آن از `UUID` استفاده کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر فایل‌های XML را parse میکنی مطمئن شو تا entity parsing غیرفعال باشد تا از `XXE` (XML External entity attack) جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر فایل‌های XML را parse میکنی، مطمئن شو تا entity expansion غیرفعال باشد تا از `Billion Laughs/XML bomb` توسط exponential entity expansion attack جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک CDN برای آپلودهای فایل استفاده کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اگر با مقادیر بسیار حجیمی از داده باید کار کنی، از Workerها و Queueها استفاده کن تا حداکثر پردازش در بک‌گراند انجام شود و سریع پاسخ را برگردان تا از HTTP Blocking جلوگیری شود.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;خاموش کردن حالت DEBUG را فراموش نکن.

## خروجی
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `X-Content-Type-Options: nosniff` را ارسال کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `X-Frame-Options: deny` را ارسال کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدر `'Content-Security-Policy: default-src 'none` را ارسال کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;هدرهایی که به نوعی اثرانگشت برجای میگذارند را حذف کن، مثلا `X-Powered-By`، `Server` و ‍`X-AspNet-Version`.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مقدار `content-type` را برای جواب اجباری کن. اگر `application/json` برمیگردانی، پس `content-type` پاسخ `application/json` است.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اطلاعات حساس مثل `داده‌های اعتبارسنجی`، `پسوورد‌ها` و `توکن‌های امنیتی` را برنگردان.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;با توجه به عملیات انجام‌شده، status code مناسب را برگردان. مثلا `200 OK`، `400 Bad Request`، `401 Unauthorized` و `405 Method Not Allowed`.

## CI & CD
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;طراحی و پیاده سازی خودت را با پوشش تست‌های unit/integration بازرسی کن.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;از یک پروسه‌ی مرور کد استفاده کن و خود-تاییدی را نادیده بگیر.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مطمئن شو تا تمامی اجزای سرویس‌هایت، شامل کتابخانه‌های استفاده‌شده و دیگر وابستگی‌ها، قبل از انتشار در حالت production، به طور ایستا توسط نرم‌افزارهای آنتی‌ویروس اسکن شده‌اند.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;برای دپلوی، یک راه‌حل با قابلیت عقبگرد (rollback) طراحی کن.


---

## نگاهی بیانداز به:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - یک مجموعه از منابع بردردبخور برای ساختن APIهای RESTful با HTTP و JSON - 


---

# مشارکت
برای همکاری و کمک می‌توانی به راحتی این مخزن را fork کنی، تغییرات مورد نظرت را اعمال کنی و یک pull request ثب کنی. اگر سوالی داشتی به آدرس `team@shieldfy.io` ایمیل بزن.
</div>
