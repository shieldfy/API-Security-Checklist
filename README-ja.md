[中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국의](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md)

# APIセキュリティチェックリスト
APIを設計、テスト、リリースするときの最も重要なセキュリティ対策のチェックリスト

------------------------------------------------------------------------------
## 認証
- [ ] Basic認証を利用せず、標準的な認証を利用する (例: JWT、OAuth)
- [ ] 「認証」、「トークンの生成」、「パスワードの保管」の車輪の再発明を行わず、標準のものを利用する
- [ ] ログインでは「最大再試行回数(Max Retry)」とjail機能を利用する
- [ ] 全ての機密データは暗号化する

### JWT (JSON Web Token)
- [ ] ランダムで複雑なキー (`JWT Secret`) を利用し、トークンに対するブルートフォース攻撃を困難にする
- [ ] ペイロードからアルゴリズムを取り出さない。バックエンドでアルゴリズムを強制する。(`HS256` か `RS256`)
- [ ] トークンの有効期限 (`TTL`, `RTTL`) を可能な限り短くする。
- [ ] 機密データをJWTペイロードに格納しない。それは[簡単に](https://jwt.io/#debugger-io)復号できる。

### OAuth
- [ ] 常に `redirect_uri` をサーバ側でホワイトリストされたURLのみを許可するよう検証する。
- [ ] Always try to exchange for code not tokens (don't allow `response_type=token`).
- [ ] `state` パラメータをランダムなハッシュと共に利用し、OAuth認証プロセスでのCSRFを防ぐ。
- [ ] デフォルトのscopeを定義し、アプリケーション毎にscopeパラメータを検証する。

## アクセス
- [ ] DDoS / ブルートフォース攻撃を防ぐためリクエストの制限 (スロットリング) を行う。
- [ ] HTTPSをサーバ側で利用しMITM (Man In The Middle Attack) を回避する。
- [ ] `HSTS`ヘッダをSSLと共に利用し、SSL Strip攻撃を回避する。

## 入力
- [ ] Use proper HTTP method according to operation, `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)` and `DELETE (to delete a record)` and respond with `405 Method Not Allowed` if requested method don't exists in resource.
- [ ] Validate `content-type` on request Accept header (Content Negotiation) to allow only your supported format (e.g. `application/xml`, `application/json`... etc) and respond with `406 Not Acceptable` response if not matched.
- [ ] Validate `content-type` of posted data as you accept (e.g. `application/x-www-form-urlencoded`, `multipart/form-data ,application/json`... etc ).
- [ ] Validate User input to avoid common vulnerabilities (e.g. `XSS`, `SQL-Injection` , `Remote Code Execution`... etc).
- [ ] Don't use any sensitive data (`credentials` , `Passwords`, `security tokens`, or `API keys`) in the URL, but use standard Authorization header.
- [ ] Use a API Gateway service to enable caching, Rate Limit, Spike Arrest and deploy API's resourses dynamically

## 処理
- [ ] Check if all the endpoints are protected behind authentication to avoid broken authentication process.
- [ ] User own resource id should be avoided. Use `/me/orders` instead of `/user/654321/orders`
- [ ] Don't use auto increment id's use `UUID` instead.
- [ ] If you are parsing XML files, make sure entity parsing is not enabled to avoid `XXE` (XML external entity attack).
- [ ] If you are parsing XML files, make sure entity expansion is not enabled to avoid `Billion Laughs/XML bomb` via exponential entity expansion attack.
- [ ] Use CDN for file uploads.
- [ ] If you are dealing with huge amount of data, use Workers and Queues to process as much as possible in background and return response fast to avoid HTTP Blocking.
- [ ] Do not forget to turn the DEBUG mode OFF.

## 出力
- [ ] Send `X-Content-Type-Options: nosniff` header.
- [ ] Send `X-Frame-Options: deny` header.
- [ ] Send `Content-Security-Policy: default-src 'none'` header.
- [ ] Remove fingerprinting headers - `X-Powered-By`, `Server`, `X-AspNet-Version` etc.
- [ ] Force `content-type` for your response, if you return `application/json` then your response `content-type` is `application/json`.
- [ ] Don't return sensitive data like `credentials`, `Passwords`, `security tokens`.
- [ ] Return the proper status code according to the operation completed. (e.g. `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` ... etc).

## CI & CD (継続的インテグレーションと継続的デリバリー)
- [ ] 設計と実装をユニットテスト、インテグレーションテストのカバレッジで監査する
- [ ] コードレビューのプロセスを採用し、自身による承認を無視する
- [ ] プロダクションへプッシュする前に、ベンダのライブラリ、その他の依存関係を含め、サービスの全ての要素がアンチウィルスソフトウェアで静的スキャンを確実に実施する
- [ ] デプロイについてロールバックソリューションを開発する


------------------------------------------------------------------------------

# コントリビューション
このリポジトリをforkして、変更し、プルリクエストを送信し、自由にコントリビューションしてください。何か質問があれば `team@shieldfy.io` まで電子メールを送ってください。
