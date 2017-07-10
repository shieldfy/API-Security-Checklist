# 开发安全的 API 所需要核对的清单
以下是当你在设计, 测试以及发布你的 API 的时候所需要核对的重要安全措施.

------------------------------------------------------------------------------
## 身份认证
- [ ] 不要使用 `Basic Auth` 使用标准的认证协议 (比如 JWT, OAuth).
- [ ] 不要再造 `Authentication`, `token generating`, `password storing` 这些轮子, 使用标准的.

### JWT (JSON Web Token)
- [ ] 使用随机复杂的秘钥 (`JWT Secret`) 以增加暴力破解的难度.
- [ ] 不要在请求体中直接提取数据, 要对数据进行加密 (`HS256` or `RS256`). 
- [ ] 是 token 的过期时间尽量的短 (`TTL`, `RTTL`) .
- [ ] 不要在 JWT 的请求体重存放敏感数据, 它是可破解的 [easily](https://jwt.io/#debugger-io).

### OAuth 授权或认证协议
- [ ] 始终在后台验证 `redirect_uri` 只允许白名单的 url.
- [ ] 每次交换令牌的时候不要加 token (不允许 `response_type=token`).
- [ ] 使用 `state` 参数并填充随机的哈希数来防止跨站请求伪造(CSRF).
- [ ] 对不同的应用分别定义默认的作用于和各自有效的作用域参数.

## 访问
- [ ] 限制流量来防止 DDos 攻击和暴力攻击.
- [ ] 在服务端使用 HTTPS 协议来防止 MITM 攻击.
- [ ] 使用 `HSTS` 协议防止 SSLStrip 攻击.

## 输入
- [ ] 使用与操作相符的 HTTP 操作函数 , `GET (读取)`, `POST (创建)`, `PUT (替换/更新)` and `DELETE (删除记录)`.
- [ ] 在请求头中的 `content-type` 字段使用内容验证来只允许支持的格式 (比如 `application/xml` , `application/json` ...) 并在不满足条件的时候返回 `406 Not Acceptable` .
- [ ] 验证 `content-type` 的发布数据和你收到的一样 (如 `application/x-www-form-urlencoded` , `multipart/form-data ,application/json` 等等... ).
- [ ] 验证用户输入来避免一些普通的易受攻击缺陷 (比如 `XSS`, `SQL-注入` , `远程代码执行` 等等...).
- [ ] 不要在 URL 中使用任何敏感的数据在 ( `credentials` , `Passwords`, `security tokens`, or `API keys`), 而是使用标准的认证请求头.

## 处理
- [ ] 检查是否所有的终端都在身份认证之后, 以防止破坏认证体系.
- [ ] 避免使用特有的资源标识符. 使用 `/me/orders` 替代 `/user/654321/orders`
- [ ] 使用 `UUID` 代替自增长的 id.
- [ ] 如果你正在解析 XML 文件, 确认确保外部实体是关闭的以避免`XXE`攻击.
- [ ] 在文件上传中使用 CDN.
- [ ] 如果你在处理大量的数据, 使用 Workers 和 Queues 来快速响应, 从而避免 HTTP 阻塞. 
- [ ] 不要忘了吧 DEBUG 模式关掉.

## 输出
- [ ] 发送 `X-Content-Type-Options: nosniff` 头.
- [ ] 发送 `X-Frame-Options: deny` 头.
- [ ] 发送 `Content-Security-Policy: default-src 'none'` 头.
- [ ] 在响应中强制使用 `content-type` , 如果你的类型是 `application/json` 那么你的 `content-type` 就是 `application/json`.
- [ ] 不要返回敏感的数据, 如 `credentials` , `Passwords`, `security tokens`.
- [ ] 在操作结束时返回恰当的状态码. (比如 `200 OK` , `400 Bad Request` , `401 Unauthorized`, `405 Method Not Allowed` 等等...).


------------------------------------------------------------------------------

# Contribution
Feel free to contribute , fork -> edit -> submit pull request. For any questions drop us an email at team@shieldfy.io.
