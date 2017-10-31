[English](./README.md) | [中文版](./README-zh.md) | [Português (Brasil)](./README-pt_BR.md) | [Français](./README-fr.md) | [한국어](./README-ko.md) | [Nederlands](./README-nl.md) | [Indonesia](./README-id.md) | [ไทย](./README-th.md) | [Русский](./README-ru.md) | [Українська](./README-uk.md) | [Español](./README-es.md) | [Italiano](./README-it.md) | [Deutsch](./README-de.md) | [Türkçe](./README-tr.md) | [Tiếng Việt](./README-vi.md) | [Монгол](./README-mn.md) | [हिंदी](./README-hi.md) | [العربية](./README-ar.md)

# APIセキュリティチェックリスト
APIを設計、テスト、リリースするときの最も重要なセキュリティ対策のチェックリスト


---

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
- [ ] 常に token ではなく code を交換するよう試行する (`response_type=token` を許可しない)。
- [ ] `state` パラメータをランダムなハッシュと共に利用し、OAuth認証プロセスでのCSRFを防ぐ。
- [ ] デフォルトのscopeを定義し、アプリケーション毎にscopeパラメータを検証する。

## アクセス
- [ ] DDoS / ブルートフォース攻撃を防ぐためリクエストの制限 (スロットリング) を行う。
- [ ] HTTPSをサーバ側で利用しMITM (Man In The Middle Attack) を回避する。
- [ ] `HSTS`ヘッダをSSLと共に利用し、SSL Strip攻撃を回避する。

## 入力
- [ ] 操作に準じて適切なHTTPメソッドを利用する、`GET (読み込み)`、`POST (作成)`、`PUT/PATCH (置き換え/更新)`、`DELETE (単一レコードの削除)。もし要求されたメソッドがリソースに存在しない場合は `405 Method Not Allowed` を返却する。
- [ ] リクエストのAcceptヘッダ (Content Negotiation) の `content-type` を検証し、サポートしているフォーマットのみを許可し (例: `application/xml`、`application/json` 等)、もし合致しなければ `406 Not Acceptable` レスポンスを応答する。
- [ ] 受け取るPOSTされたデータの`content-type` を検証する (例: `application/x-www-form-urlencoded`、`multipart/form-data ,application/json` 等)。
- [ ] 一般的な脆弱性を避けるためユーザ入力を検証する (例: `XSS`, `SQLインジェクション`, `リモートコード実行` 等)。
- [ ] URL中で機密データ (`クレデンシャル`、`パスワード`、`セキュリティトークン`) を利用せず、標準的な認証ヘッダで利用する。
- [ ] キャッシュ、レート制限、スパイク阻止、そしてAPIリソースのデプロイを動的に行うため、APIゲートウェイサービスを利用する。

## 処理
- [ ] 壊れた認証プロセスを回避するため、全てのエンドポイントが認証の背後で保護されているかを確認する。
- [ ] ユーザ所有リソースのIDの利用は避ける。`/user/654321/orders` の代わりに `/me/orders` を利用する。
- [ ] オートインクリメントのIDを利用せず、代わりに`UUID`を利用する。
- [ ] XMLファイルをパースする場合は、`XXE` (XML external entity attack) を回避するため entity parsing が有効でないことを確認する。
- [ ] XMLファイルをパースする場合は、exponential entity expansion attack による `Billion Laughs/XML bomb` 攻撃を回避するため entity expansion が有効でないことを確認する。
- [ ] ファイルアップロードにCDNを利用する。
- [ ] 非常に多量のデータを扱う場合は、ワーカーとキューを利用して可能な限りバックグラウンドで処理をするようにし、早く応答を返却し、HTTP Blockingを避ける。
- [ ] DEBUGモードをオフにするのを忘れない。

## 出力
- [ ] `X-Content-Type-Options: nosniff` ヘッダを送信する。
- [ ] `X-Frame-Options: deny` ヘッダを送信する。
- [ ] `Content-Security-Policy: default-src 'none'` ヘッダを送信する。
- [ ] フィンガープリントヘッダを削除する - `X-Powered-By`, `Server`, `X-AspNet-Version` 等。
- [ ] `content-type` を応答で強制する。もし `application/json` を返却するのなら、レスポンスの `content-type` は `application/json` にする。
- [ ] `認証情報`、`パスワード`、`セキュリティトークン` といった機密データを返却しない。
- [ ] 完了した操作に一致した適切なステータスコードを返却する (例: `200 OK`、`400 Bad Request`、`401 Unauthorized`、`405 Method Not Allowed` ... 等)。

## CI & CD (継続的インテグレーションと継続的デリバリー)
- [ ] 設計と実装をユニットテスト、インテグレーションテストのカバレッジで監査する。
- [ ] コードレビューのプロセスを採用し、自身による承認を無視する。
- [ ] プロダクションへプッシュする前に、ベンダのライブラリ、その他の依存関係を含め、サービスの全ての要素がアンチウィルスソフトウェアで静的スキャンを確実に実施する。
- [ ] デプロイについてロールバックソリューションを開発する。


---

## 参照：
- [yosriady/api-development-tools](https://github.com/yosriady/api-development-tools) - RESTful HTTP+JSON APIを構築するための有用なリソースの集まり。


---

# コントリビューション
このリポジトリをforkして、変更し、プルリクエストを送信し、自由にコントリビューションしてください。何か質問があれば `team@shieldfy.io` まで電子メールを送ってください。
