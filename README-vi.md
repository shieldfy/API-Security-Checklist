[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md)

# Danh sách các giải pháp an toàn cho API

Những giải pháp an toàn và cách khắc phục khi thiết kế, kiểm tra và phát hành API cho ứng dụng của bạn.

---

## Xác thực (Authentication)

- [ ] Không sử dụng `Basic Auth`. Sử dụng giao thức xác thực tiêu chuẩn (chẳng hạn [JWT](https://jwt.io/) hay [OAuth](https://oauth.net/)).
- [ ] Không tự thiết kế lại các giải pháp `Authentication`, `token generation`, `password storage`. Hãy sử dụng các giải pháp tiêu chuẩn.
- [ ] Sử dụng `Max Retry` và chức năng Auto Block ở trang Login.
- [ ] Mã hóa các dữ liệu nhạy cảm.

### JWT (JSON Web Token)

- [ ] Sử dụng khóa ngẫu nhiên (`JWT Secret`) để tăng sự khó khăn của việc tấn công Brute Force.
- [ ] Không sử dụng các thuật toán có trong `Payload` của người dùng. Bắt buộc sử dụng thuật toán phía backend (`HS256` hoặc `RS256`).
- [ ] Đặt thời hạn token (`TTL`, `RTTL`) càng ngắn càng tốt.
- [ ] Không lưu các thông tin nhạy cảm trong JWT, nó có thể [dễ dàng](https://jwt.io/#debugger-io) được giải mã.
- [ ] Tránh lưu trữ quá nhiều dữ liệu. JWT thường được chia sẻ trong header và chúng có giới hạn về kích thước.

## Quyền

- [ ] Giới hạn request (Throttling) để phòng tránh các tấn công DDoS / brute-force.
- [ ] Sử dụng giao thức HTTPS ở phía server để tránh MITM (Man In The Middle Attack).
- [ ] Sử dụng `HSTS` header với SSL để tránh tấn công SSL Strip.
- [ ] Tắt danh sách thư mục.
- [ ] Đối với các API riêng tư, chỉ cho phép truy cập từ các IP / máy chủ có trong danh sách cho phép / danh sách trắng / whitelist.

## Ủy quyền (Authorization)

### OAuth Ủy quyền hoặc chứng thực giao thức

- [ ] Luôn xác nhận `redirect_uri` phía server để chỉ cho phép redirect đến các URL tin cậy.
- [ ] Ưu tiên sử dụng `response_type=code` thay vì `response_type=token`).
- [ ] Sử dụng tham số `state` cùng một giá trị hash ngẫu nhiên để chống lại tấn công CSRF trong quá trình xác thực OAuth.
- [ ] Định nghĩa phạm vi mặc định, và xác nhận các tham số phạm vi cho mỗi ứng dụng.

## Input

- [ ] Sử dụng các HTTP method phù hợp với từng hành động: `GET (đọc)`, `POST (tạo mới)`, `PUT/PATCH (cập nhật/sửa)`, `DELETE (để xóa bản ghi)`, và phản hồi `405 Method Not Allowed` nếu HTTP method không phù hợp với tài nguyên được request.
- [ ] Xác nhận dữ liệu `content-type` ở mỗi tiêu đề (Content Negotiation) chỉ cho phép những định dạng được hỗ trợ (chẳng hạn như. `application/xml`, `application/json`, vv) và phản hồi `406 Not Acceptable` nếu không khớp.
- [ ] Xác nhận dữ liệu `content-type` được chấp nhận khi gửi lên (chẳng hạn như. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`...).
- [ ] Kiểm tra dữ liệu truyền lên từ người dùng để tránh các lỗ hổng phổ biến (chẳng hạn như `XSS`, `SQL-Injection`, `Remote Code Execution`...).
- [ ] Không sử dụng các dữ liệu nhạy cảm như (`credentials`, `Passwords`, `security tokens`, hoặc `API keys`) tại URL, sử dụng header Authorization để xác thực.
- [ ] Chỉ sử dụng mã hóa phía máy chủ.
- [ ] Sử dụng API Gateway để kích hoạt cache, Rate Limit policies (chẳng hạng như. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) và deploy APIs resources linh động hơn.

## Processing

- [ ] Đảm bảo rằng các endpoint chỉ xử lý dữ liệu sau khi đã qua bước xác thực.
- [ ] Nên tránh việc sử dụng ID của tài nguyên. Sử dụng `/me/orders` thay vì `/user/654321/orders`.
- [ ] Không nên thiết kế ID dạng tự động tăng. Sử dụng UUID để thay thế.
- [ ] Nếu bạn muốn parse XML, hãy chắc chắn rằng `entity parsing` không được kích hoạt để tránh tấn công `XXE` (XML external entity attack).
- [ ] Nếu bạn muốn parse XML, hãy chắc chắn rằng `entity expansion` không được kích hoạt để tránh tấn công `Billion Laughs/XML bomb`.
- [ ] Sử dụng CDN để tải lên tệp tin.
- [ ] Nếu bạn đang cần xử lý với lượng dữ liệu lớn, sử dụng các kỹ thuật Workers và Queues để xử lý tác vụ dưới nền càng nhiều càng tốt và giúp phản hồi nhanh để tránh bị timeout HTTP.
- [ ] Đừng quên tắt chế độ DEBUG.
- [ ] Sử dụng stack không thực thi khi có sẵn.

## Output

- [ ] Thêm `X-Content-Type-Options: nosniff` vào response headers.
- [ ] Thêm `X-Frame-Options: deny` vào response headers.
- [ ] Thêm `Content-Security-Policy: default-src 'none'` vào response headers.
- [ ] Loại bỏ các header chứa thông tin nhạy cảm như phiên bản web server, ví dụ: `X-Powered-By`, `Server`, `X-AspNet-Version`, v.v...
- [ ] Bắt buộc có `content-type` trong response headers, nếu bạn trả về `application/json` thì header `content-type` sẽ có giá trị `application/json`.
- [ ] Không trả về client các thông tin nhạy cảm như `credentials`, `Passwords`, `security tokens`.
- [ ] Trả về status code tương ứng với hành động đã hoàn thành. (chẳng hạn. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`...).

## CI & CD (Tích hợp và triển khai liên tục)

- [ ] Kiểm tra thiết kế và thực hiện đầy đủ việc test với unit/integration.
- [ ] Áp dụng quy trình đánh giá code và bỏ qua việc tự phê duyệt.
- [ ] Đảm bảo các thành phần của dịch vụ được quét với các anti virus trước khi đưa ra phiên bản production, bao gồm các thư viện và các gói khác.
- [ ] Liên tục chạy các bài kiểm tra bảo mật (phân tích tĩnh/động) trên mã của bạn.
- [ ] Kiểm tra các phần phụ thuộc của bạn (cả phần mềm và hệ điều hành) để tìm các lỗ hổng đã biết.
- [ ] Thiết kế một giải pháp rollback cho việc triển khai.

## Giám sát (Monitoring)

- [ ] Sử dụng đăng nhập tập trung cho tất cả các dịch vụ và thành phần.
- [ ] Sử dụng các tác nhân để giám sát tất cả lưu lượng truy cập, lỗi, yêu cầu, và phản hồi.
- [ ] Sử dụng cảnh báo cho SMS, Slack, Email, Telegram, Kibana, Cloudwatch, vv.
- [ ] Đảm bảo rằng bạn không ghi nhật ký bất kỳ dữ liệu nhạy cảm nào thẻ tín dụng, mật khẩu, mã PIN, vv.
- [ ] Sử dụng hệ thống IDS và/hoặc IPS để giám sát các yêu cầu và phản hồi của API của bạn.

---

## Xem thêm:

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Tập hợp các tài nguyên hữu ích để xây dựng API RESTful HTTP+JSON.

---

# Đóng góp

Hãy đóng góp bằng cách forking kho này, thực hiện một số thay đổi và gửi yêu cầu kéo. Đối với bất kỳ câu hỏi nào, hãy gửi email cho chúng tôi theo địa chỉ `team@shieldfy.io`.
