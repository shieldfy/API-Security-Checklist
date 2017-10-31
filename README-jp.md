[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# API Security Checklist
これはAPIの設計, テスト, リリース時における、重要なセキュリティ対策チェックリストです。


---

## 認証(Authentication)
- [ ] `Basic認証`を使用してはならない。標準的な認証を使う。(例 JWT, OAuth)
- [ ] `認証`, `トークン生成`, `パスワードの保管`において車輪の再発明をしてはならない。
- [ ] `最大ログイン試行回数` (Max Retry) と、jail featuresを使用する。
- [ ] 全ての秘匿情報を暗号化する。

### JWT (JSON Web Token)
- [ ] ブルートフォース攻撃を困難にするため、ランダムで複雑なキー (`JWT Secret`) を使用する。
- [ ] ペイロードからアルゴリズムを抽出してはならない。必ずバックエンドで暗号化する。(`HS256`若しくは`RS256`)
- [ ] トークンの有効期限 (`TTL`, `RTTL`) は、可能な限り短くする。
- [ ] JWTのペイロードに秘匿情報を含めてはならない。それは[簡単に](https://jwt.io/#debugger-io)復号化される。

### OAuth
- [ ] サーバサイドで常に`redirect_uri`を検証し、ホワイトリストに含まれるURLのみを許可する。
- [ ] tokenではなく、codeでのやり取りを心がける。(`response_type=token`を許可しない)
- [ ] OAuthの認証プロセスでのCSRFを防ぐため、`state`パラメータはランダムなハッシュと合わせて使用する。
- [ ] デフォルトのscopeを指定し、各アプリケーションでscopeパラメータを検証する。

## 通信 (Access)
- [ ] DDoSやブルートフォース攻撃を避けるため、リクエストの制限 (スロットリング) を設ける。
- [ ] MITM (Man In The Middle Attack) を防ぐため、サーバサイドではHTTPSを使用する。
- [ ] SSL Strip attackを防ぐため、SSL化して`HSTS`を設定する。

## 入力 (Input)
- [ ] 処理内容に応じて、適切なHTTPメソッドを使用する。: `GET (read)`, `POST (create)`, `PUT/PATCH (replace/update)`, `DELETE (レコード削除)`。リクエストメソッドがリソースに対して適切ではない場合、`405 Method Not Allowed`を返す。
- [ ] HTTPヘッダーのAccept (コンテンツネゴシエーション) の`content-type`を検証する。サポートしているフォーマット (例 `application/xml`, `application/json` 等) は許可し、そうでない場合は`406 Not Acceptable`を返す。
- [ ] 受け取ったデータの`content-type`が、受け入れ可能かどうか検証する。(例 `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json` 等)
- [ ] ユーザーの入力に一般的な脆弱性が含まれていないことを検証する。(例 `XSS`, `SQL-Injection`, `Remote Code Execution` 等)
- [ ] 秘匿情報 (`クレデンシャル情報`, `パスワード`, `セキュリティトークン`, `APIキー`) をURLに使用してはならない。標準のAuthorizationヘッダを使用する。
- [ ] APIゲートウェイを使用し、キャッシュ, Rate Limit policies (例 `Quota`, `Spike Arrest`, `Concurrent Rate Limit`), 動的なAPIリソースのデプロイを有効化する。

## 処理 (Processing)
- [ ] 壊れた認証プロセスを避けるため、全てのエンドポイントが認証により守られていることを確かめる。
- [ ] ユーザーに紐付いたリソースIDを使用してはならない。`/user/654321/orders`ではなく`/me/orders`を使用する。
- [ ] auto-incrementなIDではなく、`UUID`を使用する。
- [ ] XMLファイルをパースする時、エンティティのパースが有効ではないことを確認する。`XXE攻撃` (XML external entity attack) を避けるため。
- [ ] XMLファイルをパースする時、エンティティ参照が有効ではないことを確認する。指数関数的エンティティ展開 (exponential entity expansion) による`Billion Laughs/XML bomb`を避けるため。
- [ ] ファイルのアップロードにはCDNを使用する。
- [ ] 大量のデータを扱う場合、バックグラウンドでWorkerプロセスやキューを出来る限り沢山使用し、返答を速く返すことでHTTPブロッキングを避ける。
- [ ] デバッグモードをOFFにすることを忘れてはならない。

## 出力 (Output)
- [ ] `X-Content-Type-Options: nosniff`をヘッダに付与する。
- [ ] `Send X-Frame-Options: deny`をヘッダに付与する。
- [ ] `Content-Security-Policy: default-src 'none'`をヘッダに付与する。
- [ ] fingerpringing headersを削除する。 - `X-Powered-By`, `Server`, `X-AspNet-Version` 等
- [ ] `content-type`を必ず付与する。`application/json`を返す時は、レスポンスの`content-type`を`application/json`にする。
- [ ] 秘匿情報 (`クレデンシャル情報`, `パスワード`, `セキュリティトークン`) を返してはならない。
- [ ] 処理の終了時に適切なステータスコードを返す。(例 `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed` 等)

## CI & CD
- [ ] ユニットテスト/結合テストのカバレッジで、設計と実装を継続的に検査する。
- [ ] コードレビュープロセスを導入し、自画自賛を止める。
- [ ] productionにpushする前に、そのサービスの全てのコンポーネントをアンチウイルスソフトで静的スキャンする。ベンダーのライブラリやその他の依存するものも含めて。
- [ ] デプロイのロールバックを用意する。


---

## 参照：
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON APIを構築するための有用なリソースの集まり。


---

# コントリビュート (Contribution)
お気軽にこのリポジトリをフォークし、変更を加え、プルリクエストを送って下さい。ご質問はこちらのメールアドレスまでお願い致します。`team@shieldfy.io`
