# API Security Checklist
Checklist ที่ต้องให้ความสำคัญเมื่อมีการสร้าง API ในช่วงการออกแบบ ทดสอบระบบ และการปล่อยให้คนนอกใช้

------------------------------------------------------------------------------
## Authentication (การพิสูจน์ตัวตน)
- [ ] ไม่ควรใช้ `Basic Auth` (การ authen ปกติด้วยusername password) สำหรับการพิสูจน์ตัวตน แต่ให้ใช้รูปแบบมาตรฐานสากลแทน(e.g. JWT, OAuth).

*remark : อาจดูงงๆหน่อย แต่จริงๆแล้วเขาต้องการจะสื่อว่าให้มีการ authentication ด้วย username password แค่ครั้งแรกครั้งเดียว จากนั้นให้ใช้วิธีอย่าง JWT ในการยืนยันตัวตนสำหรับครั้งต่อๆไปแทน ซึ่งจะทำให้เราไม่ต้องแนบ password ติดไปกับ request ทุกครั้งที่มีการเรียก service เพื่อ authen ใหม่ และสามารถคุมได้ว่า token นี้ควรจะหมดอายุเมื่อไหร่อีกด้วย
	
- [ ] ไม่ต้องเสียเวลาสร้างวิธี Authentication ใหม่ขึ้นมา ให้ใช้ที่มีอยู่ในมาตรฐานไปเลย 

*remark : คิดว่าน่าจะเป็นเพราะเขาเกรงว่าถ้ามานั่งทำเอง เราพลาดบางจุดไปได้ สู้ไปใช้ที่เขาวิเคราะห์กันมาแล้วว่าปลอดภัยไปเลยดีกว่า

- [ ] ให้มีการจำกัดจำนวนครั้ง และสร้างระบบคุกขึ้นมา

*remark : ระบบคุกคือ หากมีจำนวนครั้งการ authentication ผิดมาเกินกำหนดที่ตั้งไว้ ก็อาจโยนเขาเข้าไปในแบล็คลิสต์ไม่ให้มีการยิงเข้ามาอีกในเวลาระยะหนึ่ง (กัน brute force, DDOS)

- [ ] ข้อมูลที่สำคัญควรมีการเข้ารหัสเสมอ 

*remark : เช่นบัตรเครดิต

### JWT (JSON Web Token)
- [ ] key ในการ generate token ควรมีความซับซ้อนสูง เพื่อป้องกันการ brute force หาตัวเข้ารหัส 
- [ ] ไม่ควรมีการแกะข้อมูลหรือขั้นตอนการถอดข้อมูลในฝั่ง client. ให้มีเฉพาะในฝั่ง server เท่านั้น โดยอาจใช้วิธีเข้าหรัสด้วย HS256 หรือ RS256 เอา

*remark : ทั้งนี้เพื่อเป็นการป้องกันไม่ให้ผู้ไม่ประสงค์ดีรู้วิธีการแกะ token รวมไปถึง key ที่ใช้ในการเข้ารหัส

- [ ] พยายามให้ token หมดอายุให้ไวที่สุดเท่าที่จะเป็นไปได้ (`TTL`, `RTTL`)

- [ ] ไม่ควรเก็บข้อมูลสำคัญใน payload ของ JWT เพราะอาจถูกแกะได้ [ง่าย](https://jwt.io/#debugger-io).

*remark : จริงๆควรระวังไว้ทุกครั้งที่มีการส่ง sensitive data ระหว่าง client-server ไม่จำกัดว่าต้องเป็น JWT
			  แต่ที่เขาเน้นเรื่อง JWT เพราะ token ถูก generate จาก server ซึ่งปกติเราไม่ควรส่ง sensitive data กลับไปให้ client อยู่แล้ว เพราะ token ปกติไม่ควรมีการแกะไปใช้ในฝั่ง client ซึ่งเมื่อไม่มีการนำไปใช้ ก็ไม่ควรมีการส่งไปแต่แรก

### OAuth
- [ ] มีการ validate `redirect_uri` ในฝั่ง server โดยยอมรับuriเฉพาะที่มีอยู่ในลิสต์ที่เราเชื่อถือเท่านั้น (whitelist)

*remark ป้องกันคนทำ phishing website ที่ redirect_uri กลับไปหาตัวserver ตัวเอง

- [ ] บังคับให้มีการใช้ response_type เป็น code เสมอ (พยายามเลี่ยง `response_type=token`)

*remark แบบ code จะมีอายุสั้น แบบ token จะอยู่ได้นานเป็นชั่วโมง

- [ ] ตัวแปร `state` ให้ใช้ random hash เพื่อป้องกัน CSRF (Cross Site Request Forgery) ในช่วง OAuth authentication.

*remark OAuth คือเรื่องของการให้สิทธิ์ (Authorization) เช่นพวกเกมมือถือที่ขอสิทธิ์ในการเข้าถึงรายชื่อเพื่อนบนเฟสบุคเป็นต้น โดย CSRF คิอการที่โจรทำรายการถึงขั้นตอนที่แสดงว่ายอมรับเข้าถึงสิทธิ์ใดบ้าง พอถึงขั้นที่จะให้กรอก user/password ก็ดัก url ที่ได้มาแล้วเอาไปให้เหยื่อด้วยวิธีอะไรก็ได้(เช่น phishing) แล้วหากเหยื่อ login เข้าด้วย url นั้น(ซึ่งหลอกเหยื่อได้เพราะ domain มันคือของตัวมันจริงๆ) ก็จะกลายเป็นให้สิทธิ์กับโจร การใช้ state มาช่วยจะเป็นการดูว่า requester คนแรก กับคนที่กรอก user,pass เป็นคนเดียวกันหรือเปล่า

- [ ] กำหนด scope และมีการ validate scope ตัวแปรสำหรับแต่ละแอป

## Access

- [ ] จำกัดจำนวนสูงสุดของ request เพื่อป้องกัน DDoS / Bruteforce
- [ ] ใช้ https เพื่อป้องกัน MITM (Man In The Middle Attack).
- [ ] ใช้ `HSTS` header กับ SSL เพื่อป้องกัน SSL Strip attack. 
*remark SSL Strip attack คือการบังคับเหยื่อให้ใช้ protocol http แทน https

## Input
- [ ] ใช้คำสั่ง HTTP ตาม operation ที่ทำ เช่น `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)` and `DELETE (to delete a record)` และตอบกลับด้วย  `405 Method Not Allowed` ถ้าไม่มีการรองรับ request ด้วย method นั้นในระบบ.

- [ ] Validate `content-type` ใน header ขา request (Content Negotiation) โดยยอมให้ส่งมาเฉพาะ format ที่กำหนด (e.g. `application/xml`, `application/json`... และอื่นๆ) และตอบกลับด้วย `406 Not Acceptable` ถ้า format ที่ส่งมาไม่ถูก.
- [ ] Validate `content-type` ของ data ที่รับมาทุกครั้ง(e.g. `application/x-www-form-urlencoded`, `multipart/form-data ,application/json`... ).
- [ ] Validate ข้อมูลที่ user ใส่เข้ามาทุกครั้งเพื่อป้องกันช่องโหว่ที่โดนกันบ่อยๆ (e.g. `XSS`, `SQL-Injection` , `Remote Code Execution`... etc).

*remark สั้นๆห้ามเชื่ออะไรที่ได้รับจาก client (never trust client) และให้ validate ในฝั่ง server ไม่ใช่ client

- [ ] ห้ามเอาข้อมูลสำคัญไปใส่ไว้ใน URL (เช่น /servicexxx?creditcardnum=1234) แต่ให้ไปแปะไว้ใน authorization header แทน (`credentials` , `Passwords`, `security tokens`, or `API keys`)

- [ ] ทำ API Gateway เพื่อให้สามารถทำ caching, Rate Limit, Spike Arrest, และการจัดสรรค์ทรัพยากรสำหรับ API ได้อย่างยืดหยุด

*remark การทำ gateway ให้กับ API จะทำให้เราตรวจได้ง่ายขึ้น หากพบความผิดปกติในระบบ

## Processing
- [ ] ตรวจดูว่า endpoints ทุกจุดอยู่ภายใต้ authentication เพื่อป้องกันช่องโหว่ที่ทำให้คนอื่นมาเรียกใช้โดยไม่จำเป็นต้องพิสูจน์ตัวตน

- [ ] ไม่ควรนำ resource id ของ user ไปใช้ (`/user/654321/orders`) แต่ให้ไปใช้แบบ  `/me/orders` แทน เพื่อป้องกัน user เปลี่ยนไปใช้ของคนอื่น

- [ ] เลข id ของ user ไม่ควรมีการสร้างแบบไล่ลำดับเพิ่มไปเรื่อยๆ แต่ให้สร้าง UUID แทน

- [ ] ถ้ามีการ parsing ไฟล์ XML, ให้ปิดส่วนของ Entity parsing ไว้เพื่อเลี่ยงที่จะโดนช่องโหว่ต่างๆเช่น (XML external entity attack,Billion Laughs/XML bomb) 

- [ ] ใช้ CDN เมื่อจำเป็นต้องมีการ upload ไฟล์จาก client

- [ ] หากต้องเผชิญกับข้อมูลขนาดใหญ่ ให้ใช้ Workers กับ คิวในการจัดการเพื่อให้มีการตอบข้อมูลกลับได้อย่างรวดเร็วจะได้ไม่เกิดคอขวดขึ้น

*remark ส่วนนี้ดูเหมือนจะเป็นเรื่องของการ design มากกว่า security, เวลาทำงานกับ task ที่ต้องใช้เวลาประมวลผลนานให้ใช้ทริคทำ async แทน. เวลา client ส่ง request เข้ามาก็นำ job นั้นไปเก็บไว้ในคิวแล้วปล่อยไปเพื่อให้ client ไม่ติดคอขวดจะได้ทำอย่างอื่นต่อไปได้ จากนั้นก็ดึงข้อมูลในคิวไปประมวลผลหลังบ้านเมื่อเสร็จแล้วอาจส่ง notification กลับไปหา client ก็ได้ (พูดง่ายๆคือเวลาเจอข้อมูลใหญ่ๆไม่ต้องให้ client มานั่งรอตอบกลับ)

- [ ] อย่าลืมปิดโหมด DEBUG ใน code หากทำไว้

## Output
- [ ] ตั้ง `X-Content-Type-Options: nosniff` ใน header.
- [ ] ตั้ง `X-Frame-Options: deny` ใน header.
- [ ] ตั้ง  `Content-Security-Policy: default-src 'none'` ในheader.
- [ ] เอา fingerprinting headers ออก - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] กำหนด content-type ใน response เช่นถ้าต้องการส่งข้อมูลที่เป็น json กลับไป ก็เซ็ต `content-type` เป็น `application/json` ไปเลย
- [ ]  ไม่ต้องส่งข้อมูลส่งข้อมูลสำคัญกลับไปหา client  (`credentials`, `Passwords`, `security tokens`).
- [ ] ตอบ status code ที่ตรงกับ operation กลับไป (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` ... etc).

## CI & CD
- [ ] ตรวจสอบ design กับ implementation ในขั้น unit/integration test อย่างครอบคลุม

*remark ไม่ใช่แค่ความถูกต้องของโปรแกรมแต่รวมในเรื่องของ security ด้วย  

- [ ] ให้ใช้ code review process ไม่ใช่ว่าตัวเองพอใจก็โอเคแล้ว

*remark หา tool ไม่ก็ให้คนอื่นมาตรวจสอบ เวลาตัวเองทำเองมักจะหลุดตลอด เพราะคิดว่าเพอร์เฟคแล้ว

- [ ] มั่นใจว่าทุกอย่างใน service ปลอดไวรัสแล้วก่อนจะนำขึ้น production รวมถึง lib ของพวก vendor กับ dependencies อื่นๆด้วย

*remark ถ้าเอาไฟล์ที่ติดไวรัสขึ้น production ก็จะทำให้ user ทุกท่านประสบกับความสนุกสนานกันอย่างถ้วนหน้า ทั้งนี้เคยเกิดเหตุนี้ในเมืองไทยมาแล้ว

- [ ] ออกแบบวิธี rollback ไว้ด้วยก่อนจะนำขึ้นไป เพราะเวลาเกิดปัญหาจะได้ย้อนกลับมาใช้ version เก่าไปก่อนได้ (อาจพบได้บ่อยตอนพัฒนา feature ใหม่ๆ)

*remark ขอเพิ่มหน่อย นอกจากแผน rollback ก็ควรเตรียมแผนสำรองสำหรับกรณีระบบเกิดรูรั่วด้วย โดยจะทำการปิดเซอวิสนั้น หรือบล็อค user นั้นก็แล้วแต่ ซึ่งการวางแผนแต่เนิ่นๆจะทำให้เราสามารถออกแบบระบบได้ยืดหยุ่นกว่า เช่นการแบ่งกลุ่มของ service เวลาปิดเราสามารถปิดเป็นกลุ่มๆไปได้หากสงสัยว่าเกิด vulnerable ทั้งก้อน โดยยังปล่อยให้ระบบอื่นที่ไม่เกี่ยวข้องทำงานต่อไปตามปกติ

------------------------------------------------------------------------------

# Contribution
Feel free to contribute by forking this repository, making some changes, and submitting pull requests. For any questions drop us an email at `team@shieldfy.io`.
