[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API নিরাপত্তা তালিকা

তালিকা করুন সবচেয়ে গুরুত্বপূর্ন নিরাপত্তা পাল্টা ব্যবস্থা যখন পরিকল্পনা, পরীক্ষামূলক, এবং নিষ্কৃতি করছেন আপনার API।

---

## প্রমাণীকরণ

- [ ] `Basic Auth` ব্যাবহার করবেন না । এর পরিবর্তে standard প্রমাণীকরণ ব্যবহার করুন (যেমন [JWT](https://jwt.io/)).
- [ ] `Authentication`, `token generation`, `password storage` এ নতুন করে চাকা উদ্ভাবন করবেন না । standards গুলোই ব্যবহার করুন ।
- [ ] `Max Retry` এবং জেলে দেওয়া(block) বৈশিষ্ট্য সম্পূর্ণ করুন
- [ ] সংবেদনশীল তথ্য গোপন(encryption) করে ব্যবহার করন

### JWT (JSON Web Token)

- [ ] একটি এলোমেলো জটিল পিন (`JWT Secret`) ব্যবহার করুন brute forcing প্রক্রিয়া কে অনেক কঠিন করতে।
- [ ] header থেকে অ্যালগরিদম নির্যাস(extract) করবেন না।অ্যালগরিদম টি কে ব্যাকএন্ড(backend) এ পাঠিয়ে দিন (`HS256` অথবা `RS256`) ।
- [ ] টোকেন (`TTL`, `RTTL`) মেয়াদকাল যত কম করা যায় তা করেন ।
- [ ] সংবেদনশীল তথ্য JWT payload এ সংরক্ষণ করবেন না। এটি খুব সহজে ডিকোড করা যায় [easily](https://jwt.io/#debugger-io)।
- [ ] অনেক বেশি তথ্য সংরক্ষণ করবেন না। JWT এটি সাধারণত হেডার এ ভাগ করে এবং এটার একটা আয়তন সীমা আছে।

## অ্যাক্সেস

- [ ] Requests এ সীমা দিয়ে দিন (Throttling) DDoS / brute-force আক্রমণ এড়ানোর জন্য।
- [ ] সার্ভার এ HTTPS এর সাথে TLS 1.2+ এবং নিরাপদ ciphers ব্যবহার করুন MITM (Man in the Middle Attack) এড়ানোর জন্য।
- [ ] `HSTS` header ব্যবহার করুন SSL এর সাছে SSL Strip আক্রমণ এড়ানোর জন্য।
- [ ] Directory তালিকা দেখানো বন্ধ করুন।
- [ ] ব্যক্তিগত APIs এর জন্য, শুধুমাত্র সাদা তালিকাভুক্ত IPs/hosts থেকে access গ্রহণ করুন।

## অনুমোদন

### OAuth

- [ ] `redirect_uri` সব সময় সার্ভার এ যাচাই করে শুধুমাত্র সাদা তালিকাভুক্ত URLs কে গ্রহণ করবেন।
- [ ] সর্বদা কোড বিনিময় করার চেষ্টা করুন, টোকেন নয় (`response_type=token` গ্রহণ করবেন না)।
- [ ] OAuth অনুমোদন প্রক্রিয়া কালে CSRF আক্রমণ থেকে বাচার জন্য `state` প্যারামিটারটি সবসময় এলোমেলো hash এর সাথে বেব্যহার করবেন।
- [ ] ডিফল্ট scope সংজ্ঞায়িত করুন, এবং প্রতিটি আবেদনের জন্য প্যারামিটারটি যাচাই করুন.

## ইনপুট

- [ ] যথাযথ HTTP পদ্ধতি ব্যবহার করুন কাজ অনুযায়ী: `GET (পড়া)`, `POST (সৃষ্টি করা)`, `PUT/PATCH (প্রতিস্থাপন/হালনাগাদ)`, and `DELETE (মুছে ফেলা)`, এবং `405 Method Not Allowed` জবাব দেওয়া যদি resource এর সাথে উপযুক্ত না হয়।
- [ ] আলাপ - আলোচনা করার সময় `content-type` টি যাচাই করুন এবং আপনার সমর্থিত বিন্যাস (যেমন, `application/xml`, `application/json`, ইত্যাদি) না হলে `406 Not Acceptable` জবাব দেওয়া।
- [ ] পাঠানো তথ্য `content-type` টি যাচাই করুন এবং আপনার সমর্থিত বিন্যাস এর সাথে (যেমন, `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, ইত্যাদি)।
- [ ] সাধারণ এবং সচরাচর দুর্বলতা এড়াতে ব্যবহারকারীর ইনপুট যাচাই করা (যেমন., `XSS`, `SQL-Injection`, `Remote Code Execution`, ইত্যাদি)।
- [ ] সংবেদনশীল তথ্য (`credentials`, `Passwords`, `security tokens`, or `API keys`) URL এ ব্যবহার করবেন না, কিন্তু standard Authorization header ব্যবহার করবেন।
- [ ] শুধুমাত্র সার্ভার এ গোপন(encryption) প্রক্রিয়া ব্যবহার করবেন।
- [ ] একটি API প্রবেশপথ সেবা ব্যবহার করবেন caching সক্রিয় করতে, হার সীমা নীতি (যেমন, `Quota`, `Spike Arrest`, or `Concurrent Rate Limit`) এবং গতিশীলভাবে APIs সংস্থান স্থাপন করুন।

## প্রক্রিয়াকরণ

- [ ] ভাঙ্গা authentication প্রক্রিয়া এড়াতে সবগুলো endpoints প্রমাণীকরণ(authentication) সহ কাজ করছে কিনা তা যাচাই করুন।
- [ ] ব্যবহারকারীর নিজের ID ব্যবহার করা উচিত নয়। `/user/654321/orders` না ব্যবহার করে এটা `/me/orders` ব্যবহার করুন।
- [ ] auto-increment ID ব্যবহার না করে, `UUID` ব্যবহার করুন।
- [ ] যদি আপনি XML তথ্য parsing করছেন, তাহলে নিশ্চিত হয়ে নিন যেন entity parsing চালু না থাকে `XXE` (XML external entity attack) আক্রমণ এড়ানোর জন্য।
- [ ] যদি আপনি XML, YAML অথবা অন্য কোন ভাষা anchors এবং refs দিয়ে parsing করছেন, তাহলে নিশ্চিত হয়ে নিন যেন entity expansion চালু না থাকে `Billion Laughs/XML bomb` via exponential entity expansion আক্রমণ এড়ানোর জন্য।
- [ ] CDN ব্যাবহার করুন ফাইল আপলোড এর জন্য।
- [ ] যদি আপনি অনেক গুলো তথ্য নিয়ে কাজ করেন তাহলে, Workers এবং Queues পটভূমিতে যত সম্ভব ব্যবহার করুন এবং তাড়াতাড়ি প্রতিক্রিয়া জানান HTTP Blocking না করার জন্য।
- [ ] DEBUG মোড বন্ধ করতে ভুলবেন না।
- [ ] non-executable stacks ব্যবহার করবেন যখন সম্ভব।

## আউটপুট

- [ ] `X-Content-Type-Options: nosniff` header পাঠান।
- [ ] `X-Frame-Options: deny` header পাঠান।
- [ ] `Content-Security-Policy: default-src 'none'` পাঠান।
- [ ] Fingerprinting headers গুলো সরিয়ে দিন - `X-Powered-By`, `Server`, `X-AspNet-Version`, ইত্যাদি।
- [ ] আপনার প্রতিক্রিয়ায় `content-type` থাকতে বাধ্য করুন. যদি আপনি `application/json` পাঠান, তাহলে আপনার `content-type` প্রতিক্রিয়া হবে `application/json`।
- [ ] সংবেদনশীল তথ্য পাঠাবেন না যেমন `credentials`, `passwords`, or `security tokens`।
- [ ] অপারেশন অনুযায়ী যথাযথ status code পাঠাবেন (যেমন, `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`, ইত্যাদি)।

## CI & CD

- [ ] আপনার পরিকল্পনা এবং বাস্তবায়ন যাচাই করুন unit/integration tests coverage এর সাথে।
- [ ] কোড পুনঃমূল্যায়ন প্রক্রিয়া ব্যবহার করুন এবং নিজের অনুমোদন উপেক্ষা করুন।
- [ ] নিশ্চিত করেন যেন আপনার সেবার সবগুলো উপাদান স্থিতিশীলভাবে AV সফটওয়্যার দ্বারা স্ক্যান করা থাকে production এ যাওয়ার আগেই, বিক্রেতা লাইব্রেরি এবং অন্যান্য নির্ভরতা সহ।
- [ ] ক্রমাগত নিরাপত্তা পরীক্ষা চালান (স্থির/গতিশীল বিশ্লেষণ) আপনার কোডে।
- [ ] আপনার নির্ভরতা চেক করুন (দুইটাই software এবং OS) পরিচিত দুর্বলতার জন্য।
- [ ] স্থাপনার জন্য একটি রোলব্যাক সমাধান পরিকল্পনা করুন।

## মনিটরিং

- [ ] সমস্ত সেবা এবং উপাদানগুলির জন্য কেন্দ্রীভূত লগইনগুলো ব্যবহার করুন৷
- [ ] ট্র্যাফিক, ত্রুটি, অনুরোধ এবং প্রতিক্রিয়াগুলো নিরীক্ষণ করতে এজেন্ট ব্যবহার করুন।
- [ ] SMS, Slack, Email, Telegram, Kibana, Cloudwatch, ইত্যাদির জন্য সতর্কতা ব্যবহার করুন।
- [ ] আপনি কোন সংবেদনশীল তথ্য লগ করছেন না তা নিশ্চিত করুন যেমন credit cards, passwords, PINs, ইত্যাদি।
- [ ] IDS অথবা IPS পদ্ধতি ব্যবহার করুন API requests এবং instances মূল্যায়ন করতে।

---

## আরও দেখুন:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON APIs নির্মাণ করার একটি দরকারী সংগ্রহ।

---

# অবদান

নিঃসঙ্কোচে repository টি fork করে অবদান রাখুন, কিছু পরিবর্তন করে এবং পুল অনুরোধ জমা দিয়ে নির্দ্বিধায় অবদান রাখুন। কোন প্রশ্নের জন্য আমাদের একটি ইমেল পাঠান `team@shieldfy.io`.
