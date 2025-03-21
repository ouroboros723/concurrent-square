+++
title="Docker Compose"
weight=2
+++

このページではDocker Composeを使ったConcrntドメインのセットアップ方法について解説します。

{{< notice info >}}
Concrntでは公式のドメイン動作環境としてKubernetesが推奨されています。Docker Composeを利用した構築も可能ですが、Compose環境特有の問題が発生する可能性があります。コミュニティに質問をするときは、Composeを使っていることを併記してください。
{{< /notice >}}

### 1. リポジトリを取得

Composeの構成ファイルが入ったリポジトリをクローンします。

```
git clone https://github.com/concrnt/concrnt-compose
```

起動する前に各種設定を完了してください。

### 2. 各種設定

必要に応じてファイルを編集し各種設定を変更してください。

#### **concrnt-compose/etc/config/config.yaml**

このファイルでConcrntドメインの基本設定を行います。

`concrnt.fqdn`

コンカレントのFQDN≒ドメイン名を指定します。この情報は後で変更することが非常に困難なため、よく考えて定義してください。HTTPSといったスキームは不要です。 `fqdn: cc1gb.anyfrog.net` のように指定してください。

`concrnt.registration`

アカウント作成の制限について設定します。`open`は誰でも登録が可能、 `invite` は招待が必要、 `close` は新規のアカウント作成ができなくなります。広く公開を考えていない場合、 `invite` で作成するのが良いでしょう。

`concrnt.privatekey`

サーバーが内部で利用する暗号鍵の設定を行います。 `compose.yaml` のあるディレクトリで下記コマンドを実行することで取得できる `privatekey` を貼り付けてください。他の各種情報は `privatekey` から復元できるため廃棄可能です。

```
docker compose run api conctl generate identity
// ... //
ccid:		 con1u0099ujr8g9m// ... //c6hgyxzp
mnemonic:	 avocado chimney // ... // faith excess blur gun
privatekey:	 69cc17321e531c6d// ... //46db5166d92ab56e0c69f4d5e0
publickey:	 0290a6b90887c3da// ... //93dafe6016b221173d8416f0a28a2f0568e19
```

`profile.*`

サーバープロフィールを設定します。後から変更できます。

| **Key**           | **意味**                    |
|-------------------|---------------------------|
| `nickname`        | サーバー名                     |
| `description`     | サーバーの紹介文                  |
| `logo`            | ロゴの画像                     |
| `wordmark`        | 登録画面に表示される Powered by の画像 |
| `themeColor`      | テーマカラー                    |
| `maintainerName`  | メンテナの名前                   |
| `maintainerEmail` | メンテナのメールアドレス              |

`profile` に設定された値は Concrnt の中で表示され、いわばサーバーの看板のように表示されます。  

<div align="center">
    <img src="/images/compose/cw-profiles.png" width="500px" >
    <p>探索に並ぶサーバー名とアイコン</p>
</div>


#### **concrnt-compose/etc/config/gateway.yaml**

`cctgateway` の設定ファイルが含まれます。基本的に設定を変更する必要はありません。

#### **concrnt-compose/etc/config/apconfig.yaml**

ActivityPubブリッジを利用する際に必要な設定項目が含まれます。

#### **concrnt-compose/etc/static/\*.yaml**

アカウント新規作成時に表示される各種注意事項や規約、管理者が収集したいデータについて定義するファイルが含まれます。

#### **concrnt-compose/compose.yaml**

このファイルでConcrntの各サービスの立ち上げと構成を行います。デフォルトで最小限のサービスが起動するようになっています。必要に応じてコメントを外し、追加サービスを有効化することが可能です。

{{< notice info >}}
各サービスは後から任意で有効化することができますので、初めての構築の場合は最小限でのセットアップを一度試してみるのがおすすめです。
{{< /notice >}}

`services.cloudflared`

Cloudflare Tunnelを利用する場合、コメントを外してトークンを置き換えてください。

{{< notice warning >}}
固定IPアドレスやポート解放を行えない環境でドメインを起動する場合、この項目の設定は必須です。 「Cloudflare Tunnelを利用する」ページに移動し、各種設定を済ませてください。
{{< /notice >}}

`services.apbridge`

ActivityPubブリッジの有効化を行います。ConcrntからActivityPubを実装しているサービスのユーザーと相互にフォローすることが可能になります。この設定を行う場合、必ず `etc/config/apconfig.yaml` の設定が必要です。また、 `etc/config/gateway.yaml` から `world.concrnt.ap-bridge` と `world.concrnt.webfinger` の項目をコメントから外し有効化する必要があります。

`services.mediaserver`

デフォルトで利用が可能な `concrnt.world` のメディアアップロード機能の代わりにConcrntサーバーでメディアを管理する `cc-media-server` を有効化します。 `etc/config/gateway.yaml` から `world.concrnt.mediaserver` の項目をコメントから外し有効化する必要があります。

`services.search` && `searvices.meilisearch`

コミュニティ内検索を有効にします。 `etc/config/gateway.yaml` から `net.concrnt.search` の項目をコメントから外し有効化する必要があります。

### 3. 起動

下記コマンドで起動します。 `compose.yaml` が存在する場所で下記コマンドを実行してください。

```
docker compose up -d
```

`concrnt.fqdn` にブラウザでアクセスし、下記画面が表示されCSIDが表示されていれば準備は完了です。

<div align="center">
    <img src="/images/compose/cc-top.png">
    <p>Concrntドメインのトップページ</p>
</div>

何かおかしい時はFAQを確認してください。

### 4. アカウント作成と連合の追加

Webクライアント [https://concrnt.world](https://concrnt.world) を用いて自身のドメインでアカウントを作成してください。


`concrnt.registration` をデフォルト設定の `invite` で作成した場合、一度コンテナでコマンドを実行し招待コードを取得する必要があります。

```
docker compose run api conctl generate invite
eyJhbGciO//...//UzNj_f7pj8a0nIwA
```

アカウント作成後は何もないまっさらなタイムラインが表示されますが心配ありません。
下記のようなタイムラインにアクセスし、何か投稿してみましょう！

- [#arrival lounge](https://concrnt.world/timeline/tar69vv26r5s4wk0r067v20bvyw@ariake.concrnt.net)
- [#Concrnt Fun](https://concrnt.world/timeline/ty3b4tc0eamm333pp0683gw1ndg@ariake.concrnt.net)

違うドメインのタイムラインに投稿したとしても、投稿のデータはすべてあなたのドメインだけに保存されます。Concrntを楽しみましょう！

## FAQ
よくある質問を掲載しています。

### コンテナを起動したがアクセスできない・エラーが表示される

問題を切り分ける必要があります。

#### 『このサイトにアクセスできません』と表示される

- DNSにConcrntのFQDNが正しく登録されていますか？
- ポートは正しく開放されていますか？
- サーバーとクライアントのグローバルIPが同じ場合、正しくアクセスできない場合があります。Cloudflare Tunnelの利用を検討してみてください。

#### CSIDが表示されない・APIがエラーを返す

- `concrnt-compose/etc/config/config.yaml` は正しく設定されていますか？
- データベースとの接続はきちんとできていますか？DBの設定を変えた場合、DNSの値は一致していますか？

