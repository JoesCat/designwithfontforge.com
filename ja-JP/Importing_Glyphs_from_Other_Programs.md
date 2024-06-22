---
published: true
layout: bookpage_ja-JP
weight: 75
category: Appendices
title: 他のプログラムからグリフをインポートする
---

汎用のイラスト作成アプリケーション（Inkscape、Adobe Illustrator など）でグリフを描きし、EPS または SVG ファイルとしてデータをインポートすることができます。

+ EPS = アドビ社の Photoshop や Illustrator などのデータを保存する画像ファイル形式。
+ SVG = Scalble Vector Graphics。解像度をまったく損なわずに拡大縮小できる画像データ保存形式。

## SVG データの直接編集（Hand coding）

>> 《※ 訳注： 以下文中での各基準ラインの位置表記は「y = ...」と書かれていますが、座標表記ではないと思われますのでご注意ください。座標系の場合は「y座標」のように表示します。》　

### 事前準備

* SVGファイルは次のように設定する必要があります：  `viewBox="0 0 1000 1000"`.

* 横幅は、実際にはグリフの幅より幅が広ければ、問題ではありません。ただし、インポートを容易にするためには、「1000」の高さが重要です。

* `y=0` がアセンダーライン、`y=1000` がディセンダーラインになります。

* （この二つのラインを超えるグリフがいくつかある可能性もあります。おそらく FontForge は正しい処理を行なうでしょうが、これはテストされていません。）

* デフォルトでは、FontForge はベースラインを `y=800` に設定します。 FontForge 座標系では、ベースラインは垂直軸の `0` 点〔y 座標 = 0〕にあります。

* FontForge でベースラインを希望の位置に設定するには、SVG データでのベースラインの y 座標を取得します。
これが、FontForge の座標系におけるアセンダーラインの垂直位置になります。 ディセンダーの場合は「1000 - y」です。「**エレメント**」⇒「**フォント情報**」に移動し、その中の「**一般情報**」項目で、アセンダー値を「**高さ**」欄に、ディセンダー値を「**深さ**」欄に入力します。 どちらの数値も「正の数」で入力します。「**EM の大きさ**」（フォントの高さ Em Size）は 1000 のままにする必要があります（SVG データの既定の高さであるためです）。

* グリフを描く場合は、相対座標を使用するのが一般的です。グリフを `<path d="M Xvalue,Yvalue` で開始します。もしグリフが、ある点から初めて、すべてその点の左側に描かれのであれば、`Xvalue` が FontForge で用いるべき「左サイドベアリング LeftBearing」の既定値になります。この値は、グリフをインポートし終わった後で簡単に調整できます（し、いずれにせよ、フォントの検証後に調整する必要がありえます）。また、`Yvalue` をベースライン値として用いるのも便利です。

* 常に、輪郭線（パス path）の `d` 属性を `z` で終了してください。そうしなくてもインポートはされますが、輪郭線の最後の点の後ろに `z` がないと、FontForge を再起動しないかぎり、グリフがメイン・ウィンドウに正しく表示されません。

* （文字「P」のように）穴を描く場合には、新しい輪郭線（パス）の点で開始せず、先に描いた輪郭線の末尾に `z` を使用し、`mNewX,NewY` で新しい輪郭線を開始して穴の部分を描きます。輪郭線（パス）に `fill-rule="evenodd"` 属性を指定すると正しく機能します。

### 作業手順

Web ブラウザを使用して、作業中の SVG を描画します。`template.svg` という名前のファイルを使用できます。このファイルは 1200 x 1200 ですが、ブラウザ・ウィンドウ内でスクロールしないように 800 x 800 でレンダリングされます。

このテンプレートでは、`y=100, y=1100, y=(100 + {baseline, capheight, etc.}, x=100, x=1100` の位置にガイドラインを描きます。

次に、作業中の SVG グリフをその文書にインポートするのに、<image xlink:href="LC_p.svg" x="100" y="100" width="1000" height="1000" /> を使用します。

これで、一方のウィンドウで文字を手動で編集しつつ、もう一方のウィンドウのブラウザを更新すると、各ガイドラインの上にその文字が描画されるのを見ることができます。

## 個人用グリフ・リスト

Unicode のコードポイントとグリフ名を記載した `namelist.txt` ファイルを、スプレッドシートなどを利用して、作成します。たとえば、

```
0xEC00 octDotDhe
0xEC01 octDotDheDbl
0xEC02 octDotDheTrpl
0xEC03 octDotDheQdrpl
0xEC04 octDotLik
0xEC05 octDotLikDbl
0xEC06 octDotLikTrpl
0xEC07 minirLik
0xEC08 minirDhe
0xEC09 minirBawah
0xEC0A soroganDhe
0x-001 soroganLik
```
Unicode のコードポイントがないグリフの場合は、上の例の最後の行のように、コードポイントに 「-1」のような形式を使用します。

次に、FontForge を起動し、「**エンコーディング**」⇒「**名前リストを読み込み**」を選択、次に「**グリフ名を変更**」を使用します。「名前リストを読み込み」コマンドでは、後に続く名前変更コマンドでの選択肢に、個人用の名前リストを追加するだけであるためです。