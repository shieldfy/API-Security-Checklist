[English](./README.md) | [繁中版](./README-tw.md) | [簡中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md)

# API Security Checklist
Checklist ທີ່ຕ້ອງໃຫ້ຄວາມສຳຄັນເມື່ອມີການສ້າງ API ໃນຊ່ວງການອອກແບບ ທົດສອບລະບົບ ແລະ ການປ່ອຍໃຫ້ຄົນນອກໃຊ້


---

## Authentication (ການພິສູດຕົວຕົນ)
- [ ] ບໍ່ຄວນໃຊ້ `Basic Auth` (ການ authen ປົກກະຕິດ້ວຍ username password) ສຳລັບການພິສູດຕົວຕົນ ແຕ່ໃຫ້ໃຊ້ຮູບແບບມາດຕະຖານສາກົນແທນ(e.g. JWT, OAuth).
- [ ] ບໍ່ຕ້ອງເສຍເວລາສ້າງວິທີ Authentication ໃໝ່ຂຶ້ນມາ ໃຫ້ໃຊ້ທີ່ມີຢູ່ໃນມາດຕະຖານໄປເລີຍ
- [ ] ໃຫ້ມີການຈຳກັດຈຳນວນຄັ້ງໃນການພະຍາຍາມ authen ແລະ ສ້າງລະບົບລ໋ອກກໍລະນີພະຍາຍາມເກີນກຳນົດ
- [ ] ຂໍ້ມູນທີ່ສຳຄັນຄວນມີການເຂົ້າລະຫັດສະເໝີ

### JWT (JSON Web Token)
- [ ] key ໃນການ generate token ຄວນມີຄວາມສັບຊ້ອນສູງ ເພື່ອປ້ອງກັນການ brute force ຫາຕົວເຂົ້າລະຫັດ
- [ ] ບໍ່ຄວນມີການແກະຂໍ້ມູນ ຫຼື ຂັ້ນຕອນການຖອດຂໍ້ມູນໃນຝັ່ງ client. ໃຫ້ມີສະເພາະໃນ server ເທົ່ານັ້ນ ໂດຍອາດໃຊ້ວິທີເຂົ້າລະຫັດດ້ວຍ HS256 ຫຼື RS256 ແທນ
- [ ] ພະຍາຍາມໃຫ້ token ໝົດອາຍຸໄວທີ່ສຸດເທົ່າທີ່ຈະເປັນໄປໄດ້ (`TTL`, `RTTL`)
- [ ] ບໍ່ຄວນເກັບຂໍ້ມູນທີ່ສຳຄັນໃນ payload ຂອງ JWT ເພາະອາດຈະຖືກແກະໄດ້ [ງ່າຍ](https://jwt.io/#debugger-io).

### OAuth
- [ ] ມີການ validate `redirect_uri` ໃນຝັ່ງ server ໂດຍຍອມຮັບ uri ສະເພາະທີ່ມີຢູ່ໃນລີສທີ່ເຮົາເຊື່ອຖືເທົ່ານັ້ນ (whitelist)
- [ ] ບັງຄັບໃຫ້ມີການໃຊ້ response_type ເປັນ code ສະເໝີ (ພະຍາຍາມລ່ຽງບໍ່ໃຊ້ `response_type=token`)
- [ ] ໂຕແປ `state` ໃຫ້ໃຊ້ random hash ເພື່ອປ້ອງກັນ CSRF (Cross Site Request Forgery) ໃນຕອນ OAuth authentication.
- [ ] ກຳນົດ scope ແລະ ມີການ validate scope ໂຕແປສຳລັບແຕ່ລະແອັບ

## Access
- [ ] ຈຳກັດຈຳນວນສູງສຸດຂອງ request ເພື່ອປ້ອງກັນ DDoS / Bruteforce
- [ ] ໃຊ້ https ເພື່ອປ້ອງກັນ MITM (Man In The Middle Attack).
- [ ] ໃຊ້ `HSTS` header ກັບ SSL ເພື່ອປ້ອງກັນ SSL Strip attack.

## Input
- [ ] ໃຊ້ຄຳສັ່ງ HTTP ຕາມ operation ທີ່ເຮັດ ເຊັ່ນ `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)` and `DELETE (to delete a record)` ແລະ ສົ່ງກັບດ້ວຍ `405 Method Not Allowed` ຖ້າບໍ່ມີການຮອງຮັບ request ດ້ວຍ method ນັ້ນໃນລະບົບ.
- [ ] Validate `content-type` ໃນ header ຂາ request (Content Negotiation) ໂດຍຍອມໃຫ້ສົ່ງມາສະເພາະ format ທີ່ກຳນົດ (e.g. `application/xml`, `application/json`... ໆລໆ) ແລະ ຕອບກັບດ້ວຍ `406 Not Acceptable` ຖ້າ format ທີ່ສົ່ງມາບໍ່ຖືກ.
- [ ] Validate `content-type` ຂອງ data ທີ່ຮັບມາທຸກຄັ້ງ(e.g. `application/x-www-form-urlencoded`, `multipart/form-data ,application/json`... ).
- [ ] Validate ຂໍ້ມູນ user ໃສ່ເຂົ້າມາທຸກຄັ້ງເພື່ອປ້ອງກັນຊ່ອງໂຫວ່ທີ່ຖືກກັນຫຼາຍໆ (e.g. `XSS`, `SQL-Injection`, `Remote Code Execution` ... etc).
- [ ] ຫ້າມເອົາຂໍ້ມູນທີ່ສຳຄັນໄປໄວ້ໃນ URL (ເຊັ່ນ /servicexxx?creditcardnum=1234) ແຕ່ໃຫ້ໄປໃສ່ໄວ້ໃນ authorization header ແທນ (`credentials`, `Passwords`, `security tokens`, or `API keys`)
- [ ] ເຮັດ API Gateway ເພື່ອໃຫ້ສາມາດເຮັດ caching, Rate Limit, Spike Arrest, ແລະ ຈັດການຊັບພະຍາກອນສຳລັບ API ໄດ້ຢ່າງຍືດຍຸ່ນ

## Processing
- [ ] ກວດເບິ່ງວ່າ endpoints ທຸກຈຸດຢູ່ພາຍໃຕ້ authentication ເພື່ອປ້ອງກັນຊ່ອງໂຫວ່ທີ່ເຮັດໃຫ້ຄົນອື່ນມາເອີ້ນໃຊ້ໂດຍບໍ່ຈຳເປັນຕ້ອງພິສູດຕົວຕົນ
- [ ] ບໍ່ຄວນນຳ resource id ຂອງ user ໄປໃຊ້ (`/user/654321/orders`) ແຕ່ໃຫ້ໄປໃຊ້ແບບ `/me/orders` ແທນ ເພື່ອປ້ອງກັນ user ປ່ຽນໄປໃຊ້ຂອງຄົນອື່ນ
- [ ] ເລກ id ຂອງ user ບໍ່ຄວນມີການສ້າງແບບໄລ່ລຳດັບໄປເລື້ອຍໆ ແຕ່ໃຫ້ສ້າງ UUID ແທນ
- [ ] ຖ້າມີການ parsing ຟາຍ XML, ໃຫ້ປິດສ່ວນຂອງ Entity parsing ໄວ້ເພື່ອຫຼີກລ່ຽງທີ່ຈະຖືກຊ່ອງໂຫວ່ຕ່າງໆເຊັ່ນ (XML external entity attack, Billion Laughs/XML bomb)
- [ ] ໃຊ້ CDN ເມື່ອຈຳເປັນຕ້ອງມີການ upload ຟາຍຈາກ client
- [ ] ຫາກຕ້ອງເຈິກັບຂໍ້ມູນຂະໜາດໃຫຍ່ ໃຫ້ໃຊ້ Workers ກັບ ຄິວໃນການຈັດການເພື່ອໃຫ້ມີການຕອບຂໍ້ມູນກັບໄດ້ຢ່າງວ່ອງໄວຈະໄດ້ບໍ່ເກີດຄວາມສ່ຽງຂຶ້ນ
- [ ] ຢ່າລືມປິດໂໝດ DEBUG ໃນ code ຫາກເຮັດໄວ້

## Output
- [ ] ຕັ້ງ `X-Content-Type-Options: nosniff` ໃນ header.
- [ ] ຕັ້ງ`X-Frame-Options: deny` ໃນ header.
- [ ] ຕັ້ງ `Content-Security-Policy: default-src 'none'` ໃນ header.
- [ ] ເອົາ fingerprinting headers ອອກ - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] ກຳນົດ content-type ໃນ response ເຊັ່ນຖ້າຕ້ອງການຂໍ້ມູນທີ່ເປັນ json ກັບໄປ ກໍເຊັດ `content-type` ເປັນ `application/json` ໄປເລີຍ
- [ ] ບໍ່ຕ້ອງສົ່ງຂໍ້ມູນສຳຄັນກັບໄປຫາ client (`credentials`, `Passwords`, `security tokens`).
- [ ] ຕອບ status code ທີ່ກົງກັບ operation ກັບໄປ (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` ... etc).

## CI & CD
- [ ] ກວດສອບ design ກັບ implementation ໃນຂັ້ນ unit/integration test ຢ່າງຄອບຄຸມ
- [ ] ໃຫ້ໃຊ້ code review process ບໍ່ແມ່ນວ່າໂຕເອງພໍໃຈກໍໂອເຄແລ້ວ
- [ ] ໝັ້ນໃຈວ່າທຸກຢ່າງ service ປອດໄວລັດແລ້ວກ່ອນຈະນຳຂຶ້ນ production ລວມໄປເຖິງ lib ຂອງພວກ vendor ກັບ dependencies ອື່ນໆ ອີກດ້ວຍ
- [ ] ອອກແບບວິທີ rollback ໄວ້ກ່ອນຈະນຳຂຶ້ນໄປ ເພາະເວລາເກີດບັນຈະໄດ້ຍ້ອນກັບມາໃຊ້ version ເກົ່າໄປກ່ອນໄດ້ (ອາດເຈິໄດ້ຫຼາຍໃນຕອນພັດທະນາ feature ໃໝ່ໆ)


---

## ເບິ່ງສິ່ງນີ້ດ້ວຍ:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - ຊຸດແຫຼ່ງຂໍ້ມູນທີ່ເປັນປະໂຫຍດໃນການສ້າງ API RESTful HTTP+JSON.


---

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
