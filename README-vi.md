[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# Danh sách các giải pháp an toàn cho API
Những giải pháp an toàn và cách khắc phục khi thiết kế, kiểm tra và phát hành API cho ứng dụng của bạn.


---

## Xác thực (Authentication)
- [ ] Không sử dụng `Basic Auth` Sử dụng giao thức xác thực tiêu chuẩn (chẳng hạn. [JWT](https://jwt.io/), [OAuth](https://oauth.net/)).
- [ ] Không cung cấp các thông tin `Authentication`, `token generation`, `password storage`. Sử dụng các tiêu chuẩn.
- [ ] Sử dụng `Max Retry` và chức năng Auto Block ở trang Login.
- [ ] Mã hóa các dữ liệu nhạy cảm.

### JWT (JSON Web Token)
- [ ] Sử dụng các mã ngẫu nhiên (`JWT Secret`) để tăng sự khó khăn của việc tấn công Brute Force.
- [ ] Không loại bỏ các thuật toán từ tải trọng. Bắt buộc sử dụng thuật toán trong backend (`HS256` hoặc `RS256`).
- [ ] Đặt thời hạn hết hạn token (`TTL`, `RTTL`) càng ngắn càng tốt.
- [ ] Không lưu các thông tin nhạy cảm trong JWT, nó có thể [dễ dàng](https://jwt.io/#debugger-io) được giải mã.

### OAuth Ủy quyền hoặc chứng thực giao thức
- [ ] Luôn xác nhận `redirect_uri` server-side để chỉ cho phép các URL trong danh sách.
- [ ] Luôn luôn cố gắng trao đổi mã và không phải là các tokens (không cho phép `response_type=token`).
- [ ] Sử dụng tham số `state` cùng với bảng băm ngẫu nhiên để bảo vệ CSRF ở tiến trình xác thực OAuth.
- [ ] Xác định phạm vi mặc định, và xác nhận các tham số phạm vi cho mỗi ứng dụng..

## Quyền
- [ ] Giới hạn truy cập (Throttling) để phòng tránh các tấn công DDoS / brute-force.
- [ ] Sử dụng giao thức HTTPS ở phía server để tránh MITM (Man In The Middle Attack).
- [ ] Sử dụng tiêu đề `HSTS` với SSL để tránh tấn công SSL Strip.

## Input
- [ ] Sử dụng các phương thức HTTP phù hợp với từng phương thức: `GET (đọc)`, `POST (tạo mới)`, `PUT/PATCH (cập nhật/sửa)`, and `DELETE (để xóa bản ghi)`, và phản hồi với `405 Method Not Allowed` nếu yêu cầu không phù hợp với tài nguyên được yêu cầu.
- [ ] Xác nhận dữ liệu `content-type` ở mỗi tiêu đề (Content Negotiation) chỉ cho phép những định dạng được hỗ trợ (chẳng hạn như. `application/xml`, `application/json`, vv) và phản hồi `406 Not Acceptable` nếu không khớp.
- [ ] Xác nhận dữ liệu `content-type` được chấp nhận khi gửi lên (chẳng hạn như. `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`, vv).
- [ ] Xác nhận đầu vào dữ liệu người dùng để tránh các lỗ hổng phổ biến (chẳng hạn như. `XSS`, `SQL-Injection`, `Remote Code Execution`, vv).
- [ ] Không sử dụng các dữ liệu nhạy cảm như (`credentials`, `Passwords`, `security tokens`, or `API keys`) tại URL, tuy nhiên có thể sử dụng header Authorization để xác thực.
- [ ] Sử dụng các dịch vụ API Gateway để bật bộ nhớ cache, Rate Limit policies (chẳng hạng như. `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) và deploy APIs resources linh động hơn.

## Processing
- [ ] Kiểm tra các điểm đầu cuối đều được bảo vệ để tránh các tiến trình xác thực bị hỏng.
- [ ] Nên tránh việc sử dụng ID của tài nguyên. Sử dụng `/me/orders` thay vì `/user/654321/orders`.
- [ ] Không tự động tăng ID. Sử dụng UUID để thay thế.
- [ ] Nếu bạn muốn phân tích các tập tin XML, hãy chắc chắn các phần tử không được bật để tránh `XXE` (XML tấn công thực thể từ bên ngoài).
- [ ] Nếu bạn muốn phân tích các tập tin XML, đảm bảo việc mở rộng thực thể không được kích hoạt để tránh để tránh `Billion Laughs/XML bomb` qua việc tấn công.
- [ ] Sử dụng CDN để tải lên tệp tin.
- [ ] Nếu bạn đang cần xử lý với lượng dữ liệu lớn, sử dụng các kỹ thuật Workers và Queues để xử lý tác vụ dưới nền càng nhiều càng tốt và giúp phản hồi nhanh để tránh bị chặn HTTP.
- [ ] Đừng quên tắt chế độ DEBUG.

## Output
- [ ] Thêm `X-Content-Type-Options: nosniff` vào response headers.
- [ ] Thêm `X-Frame-Options: deny` vào response headers.
- [ ] Thêm `Content-Security-Policy: default-src 'none'` vào response headers.
- [ ] Loại bỏ các header chứa thông tin nhạy cảm như phiên bản web server, ví dụ: `X-Powered-By`, `Server`, `X-AspNet-Version`, v.v...
- [ ] Bắt buộc có `content-type` trong response headers, nếu bạn trả về `application/json` thì header `content-type` sẽ có  giá trị `application/json`.
- [ ] Không gửi các thông tin nhạy cảm như `credentials`, `Passwords`, `security tokens`.
- [ ] Trả về status code tương ứng với hành động đã hoàn thành. (chẳng hạn. `200 OK`, `400 Bad Request`, `401 Unauthorized`, 405 `Method Not Allowed`, v.v...).

## CI & CD ( Tích hợp và triển khai liên tục)
- [ ] Kiểm tra thiết kế và thực hiện đầy đủ việc test với unit/integration.
- [ ] Áp dụng quy trình đánh giá code và bỏ qua việc tự phê duyệt.
- [ ] Đảm bảo các thành phần của dịch vụ được duyệt với phần mềm AV trước khi được đẩy lên bản chính, bao gồm các thư viện và các sự phụ thuộc khác.
- [ ] Thiết kế một giải pháp rollback (quản lý dữ liệu) cho việc triển khai.


---

## Xem thêm:
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - Tập hợp các tài nguyên hữu ích để xây dựng API RESTful HTTP+JSON.


---

# Đóng góp
Hãy đóng góp bằng cách forking kho này, thực hiện một số thay đổi và gửi yêu cầu kéo. Đối với bất kỳ câu hỏi nào, hãy gửi email cho chúng tôi theo địa chỉ `team@shieldfy.io`.
