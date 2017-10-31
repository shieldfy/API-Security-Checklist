[中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md)

# API Security Checklist
قائمة تحتوي على أهم الاحتياطات الامنية حينما تقوم بتخطيط و اختبار و اطلاق ال API الخاصة بك


---

## المصادقة (Authentication)
- [ ] لا تستخدم `Basic Auth` لكن استخدم المعايير القياسية للمصادقة (مثال [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] لا تعيد اختراع العجله في `المصادقة`, `توليد الرموز`, `تخزين كلمات المرور`. قم بإستخدام المعايير القياسية.
- [ ] استخدم `تحديد عدد المحاولات` و `الرمان من الدخول jail feature` في تسجيل الدخول.
- [ ] استخدم التشفير في كل البيانات الحساسة.

### JWT (JSON Web Token)
- [ ] إستخدم مفتاح عشوائي و معقد (`JWT Secret`) لتجعل هجوم التخمين بالقوة brute forcing صعب جدا.
- [ ] لا تقم بإستخراج خوارزمية التشفير من محتوى رمز ال JWT. قم بإجبار الكود بإستخدام خوارزمية (`HS256` or `RS256`).
- [ ] إجعل مدة انتهاء الرمز (`TTL`, `RTTL`) قليلة قدر الإمكان.
- [ ] لا تقم بتخزين اي بيانات حساسة داخل محتوى رمز ال JWT, لانه يمكن كشف هذه المحتويات بسهولة [easily](https://jwt.io/#debugger-io).

### OAuth
- [ ] تحقق دائما من  `redirect_uri` في كود السيرفر لتسمح فقط بقائمة محددة من الروابط.
- [ ] دائما حاول ان تقولم بالتبادل و الرد بكود و ليس بالرمز (لا تسمح  `response_type=token`).
- [ ] إستخدم متغير `state` في الرابط مع مزيج عشوائي من الحروف لتمنع هجمات ال CSRF على عملية المصادقة الخاصة بال OAuth.
- [ ] حدد الصلاحية و النطاق الافتراضي scope, و قم بالتحقق منه مع كل تطبيق.

## الوصول
- [ ] حدد الطلبات (Throttling) لتتجنب هجوم حجب الخدمة  DDoS و هجوم التخمين بالقوة brute-force.
- [ ] إستخدم HTTPS على السيرفر لتتجنب هجمات التنصت على الطلبات  MITM (Man In The Middle Attack).
- [ ] إستخدم `HSTS` header مع ال  SSL لتتجنب هجمات ال  SSL Strip.

## الإدخال
- [ ] إستخدم الوسيلة المناسبة  HTTP method حسب العملية التي تريد القيام بها : `GET (للقرائة)`, `POST (إنتاج او اضافة)`, `PUT/PATCH (لإستبدال او تحديث)`, and `DELETE (لحذف سجل)`, و قم بالرد ب  `405 Method Not Allowed` في حالة إذا كانت الوسيلة method غير مناسبة .
- [ ] قم بالتحقق من  `content-type` في رأس الطلب reuest header أو ما يسمى ب (Content Negotiation) لتسمح فقط بالتنسيقات المدعومة  (مثال `application/xml`, `application/json`, إلى آخره) و قم بالرد ب  `406 Not Acceptable` إذا كان التنسيق غير ذلك.
- [ ] قم بالتحقق من  `content-type` في محتوى الطلب نفسه posted data  (مثال `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, إلى آخره).
- [ ] قم بالتحثث من مدخلات المستخدم لتتجنب الثغرات الشائعة  (مثال `XSS`, `SQL-Injection`, `Remote Code Execution`, إلى آخره).
- [ ] لا تستخدم اي بيانات حساسة  (`credentials`, `Passwords`, `security tokens`, or `API keys`) في الرابط و لكن استخدم الطريقة القياسية وهي رأس الطلب الخاص بالمصادقة Authorization header.
- [ ] إستخدم واجهة لل API ل تستفيد من التخزين المؤقت caching و سياسات تحديد عدد الطلبات Rate Limit policies (مثال `الحصة Quota`, `التنبية في الارتفاع المفاجئ Spike Arrest`, `و تحديد عدد الطلبات المتزامنة Concurrent Rate Limit`)

## المعالجة
- [ ] قم بفحص كل النطاقات و الروابط انهم محميين وراء مصادقة authentication لتتجنب المصادقة المكسورة broken authentication.
- [ ] يجب تجنب استخدام المعرف الخاص بالموارد . قم بإستخدام   `/me/orders` بدلا من `/user/654321/orders`.
- [ ] لا تقم بإستخدام المعرف التلقائي auto-increment . قم بإستخدام `UUID` بدلا منه.
- [ ] لو انك تقوم بمعالجة ملفات  XML, تأكد من ان معالجة  entity parsing غير مفعلة لتتجنب هجمات  `XXE` (XML external entity).
- [ ] لو انك تقوم بمعالجة ملفات  XML, تأكد من ان entity expansion غير مفعلة لتتجنب هجمات `Billion Laughs/XML bomb` من خلال هجوم  exponential entity expansion.
- [ ] إستخدم شبكات تسليم المحتوى CDN لرفع الملفات.
- [ ] لو انك تتعامل مع حجم بيانات ضخم, إستخدم عمليات منفصلة Workers , Queues لمعالجة البيانات في الخلفية و الرد على المستخدم بسرعه لتجنب حجب الطلب HTTP Blocking.
- [ ] لا تنسى و تترك وضع التصحيح DEBUG mode في حالة التشغيل.

## المخرجات
- [ ] إستخدم `X-Content-Type-Options: nosniff` في رأس الطلب  header.
- [ ] إستخدم `X-Frame-Options: deny`  في رأس الطلب  header.
- [ ] إستخدم `Content-Security-Policy: default-src 'none'`  في رأس الطلب  header.
- [ ] إحذف الرؤوس headers التي تدل عليك  - `X-Powered-By`, `Server`, `X-AspNet-Version` إلى آخره.
- [ ] إجبر إرسال `content-type` مع الرد, لو انك تقوم بالرد بمحتويات من توع  `application/json`  فم بالرد ب`content-type`  `application/json`.
- [ ] لا تقم بالرد بمعلومات  و بيانات حساسة مثل  `credentials`, `Passwords`, `security tokens`.
- [ ] قم بالرد بكود حالة صحيح status code طبقا للعملية التي تقوم بها. (مثال `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, إلى آخره).

## التكامل المستمر CI & النشر المستمر CD
- [ ] مراجعة التصميم الخاص بك والتنفيذ مع وحدة / التكامل اختبارات الاختبار unit/integration tests coverage.
- [ ] استخدام عملية مراجعة الكود وتجاهل الموافقة على الكود الذي قمت بكتابته.
- [ ] تأكد من أن جميع مكونات الخدمات الخاصة بك يتم فحصها بشكل ثابت بواسطة برامج الفيروسات قبل ارسالها إلى الإنتاج، بما في ذلك المكتبات الخارجية وغيرها من التبعيات.
- [ ] تصميم حل التراجع عن عمليات النشر rollback.


---

## أنظر أيضا:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - مجموعة من الادوات و المصادر لبناء RESTful HTTP+JSON APIs.


---

# المشاركة
لا تتردد في المساهمة عن طريق اخذ نسخة من هذه القائمة fork ، وإجراء بعض التغييرات، وتقديم طلبات المراجعة pull request.  أي أسئلة الرجاء مراسلتنا على البريد الإلكتروني `team@shieldfy.io`.
