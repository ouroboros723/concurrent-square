+++
title="はじめよう"
weight=1
+++

## コンカレントってどんなところ？
コンカレントの基盤を用いたSNSの実装の1つとして、concrnt.worldが現在サービスを提供しています。
スクリーンショットを見て見ましょう

<div align="center">
    <img src="/images/overview.png" width="500px" >
    <p>図2 concrnt.worldの基本画面</p>
</div>

少し情報量が多いように見えますが、分かってしまえば意外とシンプルです。この章ではconcrnt.worldのUIを通してコンカレントの機能を紹介していきます。

### 登録
登録に対して特筆することはありませんが、ここで1つ説明しておくとすれば、コンカレントもNostrと同じく、ユーザーが秘密鍵を保有し、それを用いて自己の同一性をユーザー自身が証明するスタイルを取っています。
最初にローカルで秘密鍵を生成し、その公開鍵からなるアドレスを作成したのち、サーバーを1つ選んで自身のデータを保有することを申請(登録)します。

また、コンカレントでは秘密鍵そのものではなく、その秘密鍵を導出できる「シークレットコード」の利用を推進しています。よく仮想通過のウォレットで見かける通称リカバリーフレーズと同じ規格を用いており、例えば"banana hazard ancient settle kiss fire soap boss toy twist honey volume"といったものです。
コンカレントでは日本語話者にやさしいように、「いんよう しゃっきん いこく はったつ せんぱい こんなん ひはん えいえん みらい めいそう すあし よろしい」と日本語のシークレットコードも利用可能にしています。

シークレットコードは秘密鍵と等価(少なくともコンカレントにおいて)なので紛失しないように、誰にも知られないように十分注意して管理しましょう。

### ユーザーページ
特定のユーザーの情報がまとまっているユーザーページを見て見ましょう(図3左)。
ユーザーページではその人の投稿すべてを見ることができます(但し投稿者があえて除外しているものを除きます)。このように、その人の発言している投稿が流れているタイムラインのことをその人の「ホームタイムライン」と呼びます。

また、アクティビティタブではその人がつけたふぁぼ・リアクション等を見ることができます。

<div align="center">
    <img src="/images/basicpages.png" width='500px' >
    <p>図3 concrnt.worldのユーザータイムラインとコミュニティタイムライン</p>
</div>

### コミュニティタイムライン
コンカレントにはユーザーのホームタイムラインとはまた別に、コミュニティタイムラインといったものが存在しています。
探索ページを見て見ましょう(図3右)。

様々なカードが存在していますね。このように、コンカレントはホームタイムラインだけでなく、複数人で共有して使える「コミュニティタイムライン」が存在しています。

コミュニティタイムラインは設計意図としては「fediverseのそれぞれのサーバー」「Discordのサーバー」くらいの単位感を想定しています。
とはいえ、使い方は自由自在なので「自分のブックマーク用タイムライン」といったものも作成してみてもいいかもしれません。

### リスト
ここで、最初の画面を見て見ましょう(図2)。
画面左側にSlackやDiscordのような、チャンネル一覧のようなものがありますね。よく見ると、Homeという「リスト」のなかに"%arraival lounge"や"%気になる"等複数のコミュニティタイムラインと、@マークから始まるユーザーのホームタイムラインが複数追加されていることが分かります。

コンカレントでは特定の人だけを見る、特定のコミュニティだけを見るといったことはせず、これらをリストに詰め込んで、すべて同時に購読することができます。もちろんリアルタイムです。
人間は複数のコミュニティに所属する生き物ですから、これはできて当然の機能ですね。
リストをピン止めしておくと、画面上部にタブが表示されるので素早く切り替えることがきます。

### 投稿してみよう
コンカレントでは投稿先設定が画面上部にあります。
左側にあるのがコミュニティ選択UIです。同時に複数のコミュニティを選択できます。
全ての投稿は自分のホームタイムラインにも反映されるべきですが、一方でエアリプ等「この話題はあまりにもコミュニティ限定的すぎる...」といったケースもあると思います。
この時はこのボタンをオフにして投稿(Ctrl+Shift+Enter)するとコミュニティだけに投稿する...といったことができますが、ちょっと複雑ですよね。玄人向けの機能だと思います。

<div align="center">
    <img src="/images/editor.png" width="500px">
    <p>図4 投稿エディター</p>
</div>

ここまでのおさらいとして投稿範囲を図で見て見ましょう。

<div align="center">
    <img src="/images/timeline_explained.png" width="500px">
    <p>図5 投稿先の説明</p>
</div>

この図では送信者Aliceに対して、閲覧者のBobはコミュニティタイムラインAだけを、CharlieはコミュニティタイムラインBとAliceのホームを、DaveはコミュニティタイムラインBだけをリストに入れて購読している状態を示しています。

今、Aliceが新しくメッセージを、AliceがコミュニティタイムラインAと、Aliceのホームタイムラインにメッセージを送信しました。すると、コミュニティタイムラインAを見ているBobと、コミュニティの方はBしか購読していないが、それはそれとしてAlice本人のホームを見ているCharlieにAliceのメッセージが表示されます。

一方、無関係のコミュニティBだけを見ているDaveにはAliceのメッセージは表示されません。


### リルート
コンカレントもよくあるSNSと同様に、いいねやリアクション・リプライがありますが、その中で少し性質が異なるのが「リルート」です。
これは要するにリツイートなのですが、リツイートする際にどこのコミュニティ向けにリツイートするかを選択できます。
これにより、ある投稿をあるコミュニティから別のコミュニティへ伝搬させることができます。

<div align="center">
    <img src="/images/reroute_explained.png" width="500px">
    <p>図6 リルートの説明</p>
</div>

前節の図で、コミュニティBとAliceのホームをリストに入れているCharlieが、「AliceのこのメッセージはコミュニティBにも見てもらうべきだ！」と考え、AliceのホームからコミュニティBへリルートしています。

こうすることで、それまではAliceのメッセージが見えていなかったDaveにも、Aliceのメッセージが見えるようになりました。

concrnt.world上の実際のUIも見て見ましょう(図7)。
リルートされたメッセージの右下を見ると、"%devcentral → %のす民 %castella (参角さんのアイコン)"といった表記になっています。

この表記から、「元のメッセージは%devcentralコミュニティタイムラインのみに投稿されていたが、リルートによって%のす民と%castellaのコミュニティタイムラインと、参角さんのホームタイムラインからも見えるようになった」ということを読み取ることができます。

<div align="center">
    <img src="/images/reroute.png" width="500px">
    <p>図7 実際のリルートされたメッセージのUI</p>
</div>
