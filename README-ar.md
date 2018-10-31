[English](./README.md) | [繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md)
<div dir="rtl">

# API Security Checklist
قائمة تحتوي على أهم الاحتياطات الأمنية حينما تقوم بتخطيط واختبار وإطلاق الـAPI الخاصة بك


---

## المصادقة (Authentication)
- [ ] لا تستخدم `Basic Auth` لكن استخدم المعايير القياسية للمصادقة (مثال [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] لا تعد اختراع العجلة في `المصادقة`، `توليد الرموز`، `تخزين كلمات المرور`. قم باستخدام المعايير القياسية.
- [ ] استخدم `تحديد عدد المحاولات` و`الرمان من الدخول jail feature` في تسجيل الدخول.
- [ ] استخدم التشفير في كل البيانات الحساسة.

### JSON Web Token) JWT)
- [ ] استخدم مفتاح عشوائي ومعقد (`JWT Secret`) لتجعل هجوم التخمين بالقوة brute forcing صعبا جدا.
- [ ] لا تقم باستخراج خوارزمية التشفير من محتوى رمز الـ JWT. قم بإجبار الرمز البرمجي على استخدام خوارزمية (`HS256` or `RS256`).
- [ ] اجعل مدة انتهاء الرمز (`TTL`, `RTTL`) قصيرة قدر الإمكان.
- [ ] لا تقم بتخزين أي بيانات حساسة داخل محتوى رمز الـ JWT, لأنه يمكن كشف هذه المحتويات بسهولة [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] تحقق دائما من `redirect_uri` في الرمز البرمجي للخادوم لتسمح فقط بقائمة محددة من الروابط.
- [ ] دائما حاول أن تقوم بالتبادل والرد برمز برمجي وليس بالرمز (لا تسمح `response_type=token`).
- [ ] استخدم متغير `state` في الرابط مع مزيج عشوائي من الحروف لتمنع هجمات الـ CSRF على عملية المصادقة الخاصة بالـ OAuth.
- [ ] حدد الصلاحية والنطاق الافتراضي scope، وقم بالتحقق منه مع كل تطبيق.

## الوصول
- [ ] حدد الطلبات (Throttling) لتتجنب هجوم حجب الخدمة DDoS وهجوم التخمين بالقوة brute-force.
- [ ] استخدم HTTPS على الخادوم لتتجنب هجمات التنصت على الطلبات MITM (Man In The Middle Attack).
- [ ] استخدم `HSTS` header مع الـ SSL لتتجنب هجمات الـ SSL Strip.

## الإدخال
- [ ] استخدم الوسيلة المناسبة HTTP method حسب العملية التي تريد القيام بها : `GET (للقرائة)`, `POST (انتاج أو اضافة)`, `PUT/PATCH (لإستبدال او تحديث)`, and `DELETE (لحذف سجل)`, و قم بالرد بـ `405 Method Not Allowed` في حالة إذا كانت الوسيلة method غير مناسبة .
- [ ] قم بالتحقق من `content-type` في رأس الطلب reuest header أو ما يسمى بـ (Content Negotiation) لتسمح فقط بالتنسيقات المدعومة (مثال `application/xml`, `application/json`, إلى آخره) وقم بالرد بـ `406 Not Acceptable` إذا كان التنسيق غير ذلك.
- [ ] قم بالتحقق من `content-type` في محتوى الطلب نفسه posted data (مثال `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, إلى آخره).
- [ ] قم بالتحقق من مدخلات المستخدم لتتجنب الثغرات الشائعة (مثال `XSS`, `SQL-Injection`, `Remote Code Execution`, إلى آخره).
- [ ] لا تستخدم أي بيانات حساسة (`credentials`, `Passwords`, `security tokens`, or `API keys`) في الرابط ولكن استخدم الطريقة القياسية وهي رأس الطلب الخاص بالمصادقة Authorization header.
- [ ] استخدم واجهة للـ API لتستفيد من التخزين المؤقت caching وسياسات تحديد عدد الطلبات Rate Limit policies (مثال `الحصة Quota`, `التنبية في الارتفاع المفاجئ Spike Arrest`, `وتحديد عدد الطلبات المتزامنة Concurrent Rate Limit`)

## المعالجة
- [ ] قم بفحص كل النطاقات والروابط للتحقق من كونهم محميين وراء مصادقة authentication لتتجنب المصادقة المكسورة broken authentication.
- [ ] يجب تجنب استخدام المعرف الخاص بالموارد. قم باستخدام `/me/orders` بدلا من `/user/654321/orders`.
- [ ] لا تقم باستخدام المعرف التلقائي auto-increment. قم باستخدام `UUID` بدلا منه.
- [ ] لو قمت بمعالجة ملفات XML, تأكد من أن معالجة entity parsing غير مفعلة لتتجنب هجمات `XXE` (XML external entity).
- [ ] لو قمت بمعالجة ملفات XML, تأكد من أن entity expansion غير مفعلة لتتجنب هجمات `Billion Laughs/XML bomb` من خلال هجوم exponential entity expansion.
- [ ] استخدم شبكات تسليم المحتوى CDN لرفع الملفات.
- [ ] لو كنت تتعامل مع حجم بيانات ضخم، استخدم عمليات منفصلة Workers, Queues لمعالجة البيانات في الخلفية والرد على المستخدم بسرعة لتجنب حجب الطلب HTTP Blocking.
- [ ] لا تترك وضع التصحيح DEBUG mode في حالة التشغيل.

## المخرجات
- [ ] استخدم `X-Content-Type-Options: nosniff` في رأس الطلب header.
- [ ] استخدم `X-Frame-Options: deny` في رأس الطلب header.
- [ ] استخدم `Content-Security-Policy: default-src 'none'` في رأس الطلب header.
- [ ] احذف الرؤوس headers التي تدل عليك - `X-Powered-By`, `Server`, `X-AspNet-Version` إلى آخره.
- [ ] قم بإجبار إرسال `content-type` مع الرد، لو قمت بالرد بمحتويات من توع `application/json` فمن المستحسن أن يكون الرد ب`content-type` `application/json`.
- [ ] لا تقم بالرد بمعلومات وبيانات حساسة مثل `credentials`, `Passwords`, `security tokens`.
- [ ] قم بالرد بكود حالة صحيح status code طبقا للعملية التي تقوم بها. (مثال `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, إلى آخره).

## التكامل المستمر CI & النشر المستمر CD
- [ ] مراجعة التصميم الخاص بك والتنفيذ مع وحدة / التكامل اختبارات الاختبار unit/integration tests coverage.
- [ ] استخدام عملية مراجعة الرمز البرمجي وتجاهل الموافقة على الرمز البرمجي الذي قمت بكتابته.
- [ ] تأكد من أن جميع مكونات الخدمات الخاصة بك يتم فحصها بشكل ثابت بواسطة برامج الفيروسات قبل إرسالها إلى الإنتاج، بما في ذلك المكتبات الخارجية وغيرها من التبعيات.
- [ ] تصميم حل التراجع عن عمليات النشر rollback.


---

## انظر أيضا:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - مجموعة من الادوات و المصادر لبناء RESTful HTTP+JSON APIs.


---

# المشاركة
لا تتردد في المساهمة عن طريق أخذ نسخة من هذه القائمة fork ، وإجراء بعض التغييرات، وتقديم طلبات المراجعة pull request. أي أسئلة الرجاء مراسلتنا على البريد الإلكتروني `team@shieldfy.io`.
</div>
