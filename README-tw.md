[English](./README.md) | [简中版](./README-zh.md) | [العربية](./README-ar.md) | [বাংলা](./README-bn.md) | [Čeština](./README-cs.md) | [Deutsch](./README-de.md) | [Ελληνικά](./README-el.md) | [Español](./README-es.md) | [فارسی](./README-fa.md) | [Français](./README-fr.md) | [हिंदी](./README-hi.md) | [Indonesia](./README-id.md) | [Italiano](./README-it.md) | [日本語](./README-ja.md) | [한국어](./README-ko.md) | [ລາວ](./README-lo.md) | [Македонски](./README-mk.md) | [മലയാളം](./README-ml.md) | [Монгол](./README-mn.md) | [Nederlands](./README-nl.md) | [Polski](./README-pl.md) | [Português (Brasil)](./README-pt_BR.md) | [Русский](./README-ru.md) | [ไทย](./README-th.md) | [Türkçe](./README-tr.md) | [Українська](./README-uk.md) | [Tiếng Việt](./README-vi.md)

# 開發安全的 API 所需要核對的清單
以下是當你在設計, 測試以及發佈你的 API 的時候所需要核對的重要安全措施.


---

## 身份認證
- [ ] 不要使用 `Basic Auth`, 使用標準的認證協議取而代之 (如 JWT, OAuth).
- [ ] 不要再造 `Authentication`, `token generating`, `password storing` 這些輪子, 使用標準的.
- [ ] 在登錄中使用 `Max Retry` 和自動封禁功能.
- [ ] 加密所有的敏感數據.

### JWT (JSON Web Token)
- [ ] 使用隨機複雜的密鑰 (`JWT Secret`) 以增加暴力破解的難度.
- [ ] 不要在請求體中直接提取數據, 要對數據進行加密 (`HS256` 或 `RS256`).
- [ ] 使 token 的過期時間儘量的短 (`TTL`, `RTTL`).
- [ ] 不要在 JWT 的請求體中存放敏感數據, 它是[可破解的](https://jwt.io/#debugger-io).
- [ ] 避免存儲過多的數據。 JWT 通常在標頭中共享，並且它們有大小限制。

## 訪問
- [ ] 限制流量來防止 DDoS 攻擊和暴力攻擊.
- [ ] 在服務端使用 HTTPS 協議來防止 MITM 攻擊.
- [ ] 使用 `HSTS` 協議防止 SSLStrip 攻擊.
- [ ] 關閉目錄列表。
- [ ] 對於私有 API，僅允許從列入白名單的 IP/主機進行訪問。

## Authorization

### OAuth 授權或認證協議
- [ ] 始終在後台驗證 `redirect_uri`, 只允許白名單的 URL.
- [ ] 每次交換令牌的時候不要加 token (不允許 `response_type=token`).
- [ ] 使用 `state` 參數並填充隨機的哈希數來防止跨站請求偽造(CSRF).
- [ ] 對不同的應用分別定義默認的作用域和各自有效的作用域參數.

## 輸入
- [ ] 使用與操作相符的 HTTP 操作函數, `GET (讀取)`, `POST (創建)`, `PUT (替換/更新)` 以及 `DELETE (刪除記錄)`, 如果請求的方法不適用於請求的資源則返回 `405 Method Not Allowed`.
- [ ] 在請求頭中的 `content-type` 欄位使用內容驗證來只允許支持的格式 (如 `application/xml`, `application/json` 等等) 並在不滿足條件的時候返回 `406 Not Acceptable`.
- [ ] 驗證 `content-type` 的發佈數據和你收到的一樣 (如 `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` 等等).
- [ ] 驗證用戶輸入來避免一些普通的易受攻擊缺陷 (如 `XSS`, `SQL-注入`, `遠程代碼執行` 等等).
- [ ] 不要在 URL 中使用任何敏感的數據 (`credentials`, `Passwords`, `security tokens`, 或 `API keys`), 而是使用標準的認證請求頭.
- [ ] 僅使用服務器端加密。
- [ ] 使用一個 API Gateway 服務來啟用緩存、訪問速率限制 (如 `Quota`, `Spike Arrest`, `Concurrent Rate Limit`) 以及動態地部署 APIs resources.

## 處理
- [ ] 檢查是否所有的終端都在身份認證之後, 以避免被破壞了的認證體系.
- [ ] 避免使用特有的資源 id. 使用 `/me/orders` 替代 `/user/654321/orders`
- [ ] 使用 `UUID` 代替自增長的 id.
- [ ] 如果需要解析 XML 文件, 確保實體解析(entity parsing)是關閉的以避免 `XXE` 攻擊.
- [ ] 如果需要解析 XML 文件, 確保實體擴展(entity expansion)是關閉的以避免通過指數實體擴展攻擊實現的 `Billion Laughs/XML bomb`.
- [ ] 在文件上傳中使用 CDN.
- [ ] 如果需要處理大量的數據, 使用 Workers 和 Queues 來快速響應, 從而避免 HTTP 阻塞.
- [ ] 不要忘了把 DEBUG 模式關掉.
- [ ] 可用時使用不可執行的堆棧。

## 輸出
- [ ] 發送 `X-Content-Type-Options: nosniff` 頭.
- [ ] 發送 `X-Frame-Options: deny` 頭.
- [ ] 發送 `Content-Security-Policy: default-src 'none'` 頭.
- [ ] 刪除指紋頭 - `X-Powered-By`, `Server`, `X-AspNet-Version` 等等.
- [ ] 在響應中強制使用 `content-type`, 如果你的類型是 `application/json` 那麼你的 `content-type` 就是 `application/json`.
- [ ] 不要返回敏感的數據, 如 `credentials`, `Passwords`, `security tokens`.
- [ ] 在操作結束時返回恰當的狀態碼. (如 `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` 等等).

## 持續整合和持續部署
- [ ] 使用單元測試和整合測試來審計你的設計和實現.
- [ ] 引入代碼審查流程, 不要自行批准更改.
- [ ] 在推送到生產環境之前確保服務的所有組件都用殺毒軟件靜態地掃瞄過, 包括第三方庫和其它依賴.
- [ ] 對您的代碼持續運行安全測試（靜態/動態分析）。
- [ ] 檢查您的依賴項（軟件和操作系統）是否存在已知漏洞。
- [ ] 為部署設計一個回滾方案.

## Monitoring
- [ ] Use centralized logins for all services and components.
- [ ] Use agents to monitor all traffic, errors, requests, and responses.
- [ ] Use alerts for SMS, Slack, Email, Telegram, Kibana, Cloudwatch, etc.
- [ ] Ensure that you aren't logging any sensitive data like credit cards, passwords, PINs, etc.
- [ ] Use an IDS and/or IPS system to monitor your API requests and instances.


---

## 也可以看看：
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - 用於構建RESTful HTTP+JSON API的有用資源集合。


---

# 貢獻
為此存儲庫創建一個 fork, 進行修改, 並提交 pull request 來貢獻. 如果您有任何問題, 請發送郵件至 `team@shieldfy.io`.
