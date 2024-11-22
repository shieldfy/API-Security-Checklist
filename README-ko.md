[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [Azərbaycan](./README-az.md) | [Български](./README-bg.md) | [বাংলা](./README-bn.md) | [Català](./README-ca.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [ພາສາລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# API 보안 점검표

API를 설계하고, 테스트하고, 배포할 때 고려해야 할 중요한 보안 대책에 대한 점검 목록입니다.

---

## 인증 (Authentication)

- [ ] `Basic Auth`를 사용하지 말고 표준 인증방식을 사용하세요. (예로, JWT, OAuth 등)
- [ ] `인증`, `토큰 생성`, `패스워드 저장`은 직접 개발하지 말고 표준을 사용하세요.
- [ ] 로그인에서 `Max Retry`와 격리 기능을 사용하세요.
- [ ] 민감한 데이터는 모두 암호화하세요.

### JWT (JSON Web Token)

- [ ] 무작위 대입 공격을 어렵게 하기 위해 랜덤하고 복잡한 키값 (`JWT Secret`)을 사용하세요.
- [ ] 요청 페이로드에서 알고리즘을 가져오지 마세요. 알고리즘은 백엔드에서 강제로 적용하세요. (`HS256` 혹은 `RS256`)
- [ ] 토큰 만료 기간 (`TTL`, `RTTL`)은 되도록 짧게 설정하세요.
- [ ] JWT 페이로드는 [디코딩이 쉽기](https://jwt.io/#debugger-io) 때문에 민감한 데이터는 저장하지 마세요.
- [ ] 너무 많은 데이터를 저장하지 마십시오. JWT는 일반적으로 header서 공유되며 크기 제한이 있습니다.

## 접근 (Access)

- [ ] DDoS나 무작위 대입 공격을 피하려면 요청 수를 제한하세요. (Throttling)
- [ ] MITM (중간자 공격)을 피하려면 서버 단에서 HTTPS를 사용하세요.
- [ ] SSL Strip 공격을 피하려면 `HSTS` 헤더를 SSL과 함께 사용하세요.
- [ ] 디렉토리 목록을 끕니다.
- [ ] 프라이빗 API의 경우, 화이트리스트에 있는 IP/호스트에서만 액세스를 허용합니다.

## 권한 부여 (Authorization)

### OAuth

- [ ] 허용된 URL만 받기 위해서는 서버 단에서 `redirect_uri`의 유효성을 항상 검증하세요.
- [ ] 항상 토큰 대신 코드를 주고받으세요. (`response_type=token`을 허용하지 마세요)
- [ ] OAuth 인증 프로세스에서 CSRF를 방지하기 위해 랜덤 해쉬값을 가진 `state` 파라미터를 사용하세요.
- [ ] 디폴트 스코프를 정의하고 각 애플리케이션마다 스코프 파라미터의 유효성을 검증하세요.

## 입력 및 요청 (Input)

- [ ] 각 요청의 연산에 맞는 적절한 HTTP 메서드를 사용하세요. `GET (읽기)`, `POST (생성)`, `PUT (대체/갱신)`, `DELETE (삭제)`. 그리고 요청 메소드가 리소스에 적합하지 않은 경우 `405 Method Not Allowed`로 응답하세요.
- [ ] 여러분이 지원하는 포맷 (예를 들어 `application/xml`이나 `application/json` 등)만을 허용하려면 요청의 Accept 헤더에서 `content-type`의 유효성을 검사하고 일치하지 않으면 `406 Not Acceptable`로 응답하세요.
- [ ] 요청받은 POST 데이터의 `content-type`을 검증하세요. (예를 들어 `application/x-www-form-urlencoded`나 `multipart/form-data` 또는 `application/json` 등)
- [ ] 일반적인 취약점들을 피하기 위해선 사용자 입력의 유효성을 검증하세요. (예를 들어 `XSS`, `SQL-Injection` 또는 `Remote Code Execution` 등)
- [ ] URL에는 그 어떤 민감한 데이터 (`자격 인증 (credentials)`, `패스워드`, `보안 토큰` 또는 `API 키`)도 포함하고 있어서는 안 되며 이러한 것들은 표준 인증 방식의 헤더를 사용하세요.
- [ ] 서버 측 암호화만 사용하십시오.
- [ ] 캐싱과 속도 제한 정책을 제공하는 API 게이트웨이 서비스 (예를 들어 `Quota`, `Spike Arrest`, `Concurrent Rate Limit`)를 사용하고, API 리소스를 동적으로 배포하세요.

## 서버 처리

- [ ] 인증 프로세스가 손상되는 것을 피하기 위해 모든 엔드포인트가 인증 프로세스 뒤에서 보호되고 있는지 확인하세요.
- [ ] 사용자 소유의 리소스 식별자는 피해야 합니다. `/user/654321/orders` 대신 `/me/orders`를 사용하세요.
- [ ] 자동 증가 (auto-increment) 식별자 대신 `UUID`를 사용하세요.
- [ ] XML 파일을 파싱하고 있다면, `XXE` (XML 외부 엔티티 공격, XML external entity attack)를 피하기 위해 엔티티 파싱을 비활성화하세요.
- [ ] XML 파일을 파싱하고 있다면, 지수적 엔티티 확장 공격을 통한 빌리언 러프/XML 폭탄을 피하기 위해 엔티티 확장을 비활성화하세요.
- [ ] 파일 업로드에는 CDN을 사용하세요.
- [ ] 거대한 양의 데이터를 다루고 있다면, 워커나 큐를 사용하여 가능한 한 백그라운드에서 처리하고, HTTP 블로킹을 피하기 위해 응답을 빠르게 반환하세요.
- [ ] 디버그 모드를 꺼놓는 일을 절대 잊지 마세요.
- [ ] 가능한 경우 실행 불가능한 스택을 사용하십시오.

## 반환 및 응답 (Output)

- [ ] `X-Content-Type-Options: nosniff` 헤더를 반환하세요.
- [ ] `X-Frame-Options: deny` 헤더를 반환하세요.
- [ ] `Content-Security-Policy: default-src 'none'` 헤더를 반환하세요.
- [ ] `X-Powered-By`, `Server`, `X-AspNet-Version` 등의 디지털 지문 (fingerprinting) 성격의 헤더는 제거하세요.
- [ ] 응답에 `content-type`을 강제하세요. 만약 `application/json` 데이터를 반환하고 있다면 응답의 `content-type`은 `application/json`입니다.
- [ ] `자격 인증 (credentials)`, `패스워드`, `보안 토큰`과 같은 민감한 데이터는 반환하지 마세요.
- [ ] 각 작업에 맞는 적절한 상태 코드를 반환하세요. (예를 들어 `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` 등)

## CI & CD

- [ ] 단위/통합 테스트 범위로 설계 및 구현을 검토하세요.
- [ ] 코드 리뷰 절차를 사용하고 자체 승인을 무시하세요.
- [ ] 제품 출시전에 백신 소프트웨어로 공급 업체의 라이브러리 및 기타 종속적인 것을 포함한 서비스의 모든 구성 요소들을 정적으로 검사했는지 확인하세요.
- [ ] 코드에 대한 보안 테스트(정적/동적 분석)를 지속해서 실행합니다.
- [ ] 알려진 취약점이 있는지 종속성(소프트웨어 및 OS 모두)을 확인하십시오.
- [ ] 배포에 대한 롤백 솔루션을 설계하세요.

## 모니터링 (Monitoring)

- [ ] 모든 서비스 및 구성 요소에 대해 중앙 집중식 로그인을 사용합니다.
- [ ] 에이전트를 사용하여 모든 트래픽, 오류, 요청 및 응답을 모니터링합니다.
- [ ] SMS, Slack, 이메일, Telegram, Kibana, Cloudwatch, 등에 대한 알림을 사용합니다.
- [ ] 신용 카드, 비밀번호, PIN, 등과 같은 민감한 데이터를 기록하고 있지 않은지 확인하십시오.
- [ ] IDS 및/또는 IPS 시스템을 사용하여 API 요청 및 인스턴스를 모니터링합니다.

---

## 참조 :

- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON API를 만드는 데 유용한 자원의 콜렉션.

---

# 기여하기

포크, 변경, pull request를 보내 자유롭게 기여하세요. 질문은 `team@shieldfy.io`로 이메일을 보내주세요.
