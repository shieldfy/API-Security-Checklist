[English](./README.md) | [繁中版](./README-tw.md) | [简中版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md) | [Polski](./README-pl.md) | [Македонски](./README-mk.md) | [ລາວ](./README-lo.md)

# APIセキュリティチェックリスト
APIを設計、テスト、リリースするときの最も重要なセキュリティ対策のチェックリスト


---

## 認証
- [ ] `Basic認証`を利用せず、標準的な認証を利用する（例: [JWT](https://jwt.io/)、[OAuth](https://oauth.net/)）。
- [ ] `認証`、`トークンの生成`、`パスワードの保管`の車輪の再発明を行わず、標準化されているものを利用する。
- [ ] ログインでは`最大リトライ回数（Max Retry）`とjail機能を利用する。
- [ ] 全ての機密情報は暗号化する。

### JWT (JSON Web Token)
- [ ] ブルートフォース攻撃を困難にするため、ランダムで複雑なキー（`JWT Secret`）を使用する。
- [ ] ペイロードからアルゴリズムを抽出してはいけない。必ずバックエンドで暗号化する（`HS256`または`RS256`）。
- [ ] トークンの有効期限（`TTL`, `RTTL`）を可能な限り短くする。
- [ ] JWTのペイロードに機密情報を格納してはいけない。それは[簡単に](https://jwt.io/#debugger-io)復号できる。

### OAuth
- [ ] サーバサイドで常に`redirect_uri`を検証し、ホワイトリストに含まれるURLのみを許可する。
- [ ] 常にtokenではなくcodeを交換するようにする（`response_type=token`を許可しない）。
- [ ] `state`パラメータをランダムなハッシュと共に利用し、OAuth認証プロセスでのCSRFを防ぐ。
- [ ] デフォルトのscopeを定義し、アプリケーション毎にscopeパラメータを検証する。

## アクセス
- [ ] DDoSやブルートフォース攻撃を回避するため、リクエストを制限（スロットリング）する。
- [ ] MITM（Man in the Middle Attack）を防ぐため、サーバサイドではHTTPSを使用する。
- [ ] SSL Strip attackを防ぐため、SSL化とともに`HSTS`ヘッダを設定する。

## 入力
- [ ] 操作に応じて適切なHTTPメソッドを利用する。`GET（読み込み）`, `POST（作成）`, `PUT/PATCH（置き換え/更新）`, `DELETE（単一レコードの削除）`。リクエストメソッドがリソースに対して適切ではない場合、`405 Method Not Allowed`を返す。
- [ ] リクエストのAcceptヘッダ（コンテンツネゴシエーション）の`content-type`を検証する。サポートしているフォーマット（例: `application/xml`, `application/json`等）は許可し、そうでない場合は`406 Not Acceptable`を返す。
- [ ] POSTされたデータの`content-type`が受け入れ可能（例: `application/x-www-form-urlencoded`, `multipart/form-data`, `application/json`等）かどうかを検証する。
- [ ] ユーザーの入力に一般的な脆弱性が含まれていないことを検証する（例: `XSS`, `SQLインジェクション`, `リモートコード実行`等）。
- [ ] URLの中に機密情報（`認証情報`, `パスワード`, `セキュリティトークン`）を利用せず、標準的な認証ヘッダを使用する。
- [ ] キャッシュ、Rate Limit policies（例: `Quota`, `Spike Arrest`, `Concurrent Rate Limit`）を有効化し、APIリソースのデプロイを動的に行うため、APIゲートウェイサービスを利用する。

## 処理
- [ ] 壊れた認証プロセスを回避するため、全てのエンドポイントが認証により守られていることを確かめる。
- [ ] ユーザーに紐付いたリソースIDを使用してはならない。`/user/654321/orders`の代わりに`/me/orders`を利用する。
- [ ] オートインクリメントなIDを利用せず、代わりに`UUID`を利用する。
- [ ] XMLファイルをパースする場合、`XXE`（XML external entity attack）を回避するため、entity parsingが有効でないことを確認する。
- [ ] XMLファイルをパースする場合、exponential entity expansion attackによる`Billion Laughs/XML bomb`攻撃を回避するためentity expansion が有効でないことを確認する。
- [ ] ファイルアップロードにはCDNを利用する。
- [ ] 大量のデータを扱う場合、バックグラウンドでWorkerプロセスやキューを出来る限り使用し、レスポンスを速く返すことでHTTPブロッキングを避ける。
- [ ] デバッグ・モードを無効にすることを忘れないでください。

## 出力
- [ ] `X-Content-Type-Options: nosniff`をヘッダに付与する。
- [ ] `X-Frame-Options: deny`をヘッダに付与する。
- [ ] `Content-Security-Policy: default-src 'none'`をヘッダに付与する。
- [ ] フィンガープリントヘッダを削除する - `X-Powered-By`, `Server`, `X-AspNet-Version`等。
- [ ] `content-type`を必ず付与する。もし`application/json`を返す場合、`content-type`は`application/json`にする。
- [ ] `認証情報`, `パスワード`, `セキュリティトークン`といった機密情報を返さない。
- [ ] 処理の終了時に適切なステータスコードを返す（例: `200 OK`, `400 Bad Request`, `401 Unauthorized`, `405 Method Not Allowed`等）。

## CI & CD (継続的インテグレーションと継続的デリバリー)
- [ ] ユニットテスト/結合テストのカバレッジで、設計と実装を継続的に検査する。
- [ ] コードレビューのプロセスを採用し、自身による承認を無視する。
- [ ] プロダクションへプッシュする前に、ベンダのライブラリ、その他の依存関係を含め、サービスの全ての要素をアンチウイルスソフトで静的スキャンする。
- [ ] デプロイのロールバックを用意する。


---

## 参照：
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON APIを構築するための有用なリソースの集まり。


---

# コントリビューション
このリポジトリをforkして、変更し、プルリクエストを送信し、自由にコントリビューションしてください。何か質問があれば `team@shieldfy.io` まで電子メールを送ってください。
