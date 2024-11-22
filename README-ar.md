[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

<div dir="rtl">

# API Security Checklist
قائمة تحتوي على أهم الاحتياطات الأمنية حينما تقوم بتخطيط واختبار وإطلاق الـAPI الخاصة بك


---

## المصادقة (Authentication)
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تستخدم `Basic Auth` لكن استخدم المعايير القياسية للمصادقة (مثال [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تعد اختراع العجلة في `المصادقة`، `توليد الرموز`، `تخزين كلمات المرور`. قم باستخدام المعايير القياسية.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم `تحديد عدد المحاولات` و`الحرمان من الدخول jail feature` في تسجيل الدخول.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم التشفير في كل البيانات الحساسة.

### JSON Web Token) JWT)
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم مفتاح عشوائي ومعقد (`JWT Secret`) لتجعل هجوم التخمين بالقوة brute forcing صعبا جدا.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تقم باستخراج خوارزمية التشفير من محتوى رمز الـ JWT. قم بإجبار الرمز البرمجي على استخدام خوارزمية (`HS256` أو `RS256`).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;اجعل مدة انتهاء الرمز (`TTL`, `RTTL`) قصيرة قدر الإمكان.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تقم بتخزين أي بيانات حساسة داخل محتوى رمز الـ JWT, لأنه يمكن كشف هذه المحتويات بسهولة [easily](https://jwt.io/#debugger-io).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;تجنب تخزين الكثير من البيانات. عادةً ما تتم مشاركة JWT في الرؤوس ولديها حد للحجم.

## الوصول
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;حدد الطلبات (Throttling) لتتجنب هجوم حجب الخدمة DDoS وهجوم التخمين بالقوة brute-force.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم HTTPS على الخادوم لتتجنب هجمات التنصت على الطلبات MITM (Man In The Middle Attack).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم `HSTS` header مع الـ SSL لتتجنب هجمات الـ SSL Strip.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بإيقاف تشغيل قوائم الدليل.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;بالنسبة لواجهات برمجة التطبيقات الخاصة، اسمح بالوصول فقط من عناوين IP والمضيفين المدرجين في القائمة البيضاء.

## Authorization

### OAuth
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;تحقق دائما من `redirect_uri` في الرمز البرمجي للخادوم لتسمح فقط بقائمة محددة من الروابط.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;دائما حاول أن تقوم بالتبادل والرد برمز برمجي وليس بالرمز (لا تسمح `response_type=token`).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم متغير `state` في الرابط مع مزيج عشوائي من الحروف لتمنع هجمات الـ CSRF على عملية المصادقة الخاصة بالـ OAuth.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;حدد الصلاحية والنطاق الافتراضي scope، وقم بالتحقق منه مع كل تطبيق.

## الإدخال
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم الوسيلة المناسبة HTTP method حسب العملية التي تريد القيام بها : `GET (للقرائة)`, `POST (انتاج أو اضافة)`, `PUT/PATCH (لإستبدال او تحديث)`, and `DELETE (لحذف سجل)`, و قم بالرد بـ `405 Method Not Allowed` في حالة إذا كانت الوسيلة method غير مناسبة .
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بالتحقق من `content-type` في رأس الطلب reuest header أو ما يسمى بـ (Content Negotiation) لتسمح فقط بالتنسيقات المدعومة (مثال `application/xml`, `application/json`, إلى آخره) وقم بالرد بـ `406 Not Acceptable` إذا كان التنسيق غير ذلك.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بالتحقق من `content-type` في محتوى الطلب نفسه posted data (مثال `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, إلى آخره).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بالتحقق من مدخلات المستخدم لتتجنب الثغرات الشائعة (مثال `XSS`, `SQL-Injection`, `Remote Code Execution`, إلى آخره).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تستخدم أي بيانات حساسة (`credentials`, `Passwords`, `security tokens`, أو `API keys`) في الرابط ولكن استخدم الطريقة القياسية وهي رأس الطلب الخاص بالمصادقة Authorization header.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم فقط التشفير من جانب الخادم.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم واجهة للـ API لتستفيد من التخزين المؤقت caching وسياسات تحديد عدد الطلبات Rate Limit policies (مثال `الحصة Quota`, `التنبية في الارتفاع المفاجئ Spike Arrest`, `وتحديد عدد الطلبات المتزامنة Concurrent Rate Limit`)

## المعالجة
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بفحص كل النطاقات والروابط للتحقق من كونهم محميين وراء مصادقة authentication لتتجنب المصادقة المكسورة broken authentication.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;يجب تجنب استخدام المعرف الخاص بالموارد. قم باستخدام `/me/orders` بدلا من `/user/654321/orders`.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تقم باستخدام المعرف التلقائي auto-increment. قم باستخدام `UUID` بدلا منه.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لو قمت بمعالجة ملفات XML, تأكد من أن معالجة entity parsing غير مفعلة لتتجنب هجمات `XXE` (XML external entity).
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لو قمت بمعالجة ملفات XML, تأكد من أن entity expansion غير مفعلة لتتجنب هجمات `Billion Laughs/XML bomb` من خلال هجوم exponential entity expansion.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم شبكات تسليم المحتوى CDN لرفع الملفات.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لو كنت تتعامل مع حجم بيانات ضخم، استخدم عمليات منفصلة Workers, Queues لمعالجة البيانات في الخلفية والرد على المستخدم بسرعة لتجنب حجب الطلب HTTP Blocking.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تترك وضع التصحيح DEBUG mode في حالة التشغيل.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم مكدسات غير قابلة للتنفيذ عند توفرها.

## المخرجات
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم `X-Content-Type-Options: nosniff` في رأس الطلب header.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم `X-Frame-Options: deny` في رأس الطلب header.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدم `Content-Security-Policy: default-src 'none'` في رأس الطلب header.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;احذف الرؤوس headers التي تدل عليك - `X-Powered-By`, `Server`, `X-AspNet-Version` إلى آخره.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بإجبار إرسال `content-type` مع الرد، لو قمت بالرد بمحتويات من توع `application/json` فمن المستحسن أن يكون الرد ب`content-type` `application/json`.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;لا تقم بالرد بمعلومات وبيانات حساسة مثل `credentials`, `Passwords`, `security tokens`.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بالرد بكود حالة صحيح status code طبقا للعملية التي تقوم بها. (مثال `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, إلى آخره).

## التكامل المستمر CI & النشر المستمر CD
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;مراجعة التصميم الخاص بك والتنفيذ مع وحدة / التكامل اختبارات الاختبار unit/integration tests coverage.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;استخدام عملية مراجعة الرمز البرمجي وتجاهل الموافقة على الرمز البرمجي الذي قمت بكتابته.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;تأكد من أن جميع مكونات الخدمات الخاصة بك يتم فحصها بشكل ثابت بواسطة برامج الفيروسات قبل إرسالها إلى الإنتاج، بما في ذلك المكتبات الخارجية وغيرها من التبعيات.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;قم بإجراء اختبارات الأمان باستمرار (التحليل الثابت/الديناميكي) على التعليمات البرمجية الخاصة بك.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;تحقق من تبعياتك (البرنامج ونظام التشغيل) بحثًا عن نقاط الضعف المعروفة.
- [ ] &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;تصميم حل التراجع عن عمليات النشر rollback.

## Monitoring
- [ ] Use centralized logins for all services and components.
- [ ] Use agents to monitor all traffic, errors, requests, and responses.
- [ ] Use alerts for SMS, Slack, Email, Telegram, Kibana, Cloudwatch, etc.
- [ ] Ensure that you aren't logging any sensitive data like credit cards, passwords, PINs, etc.
- [ ] Use an IDS and/or IPS system to monitor your API requests and instances.


---

## انظر أيضا:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - مجموعة من الادوات و المصادر لبناء RESTful HTTP+JSON APIs.


---

# المشاركة
لا تتردد في المساهمة عن طريق أخذ نسخة من هذه القائمة fork، وإجراء بعض التغييرات، وتقديم طلبات المراجعة pull request. أي أسئلة الرجاء مراسلتنا على البريد الإلكتروني `team@shieldfy.io`.
</div>
