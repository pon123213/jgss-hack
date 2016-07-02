[English version](RTK1_Composite.md)

# [RTK1_Composite](RTK1_Composite.js) プラグイン

RPGツクール MV 用に作成した、合成機能を実現するプラグインです。

ダウンロード: [RTK1_Composite.js](https://raw.githubusercontent.com/yamachan/jgss-hack/master/RTK1_Composite.js)

## 概要

[RTK1_Core プラグイン](RTK1_Core.ja.md) を前提としていますので、先に読み込んでください。

![Screen shot - Pligin Manager](i/RTK1_Composite-01.png)

パラメータは全て初期値のままで、特に変更する必要はありません。

![Screen shot - Plugin](i/RTK1_Composite-02.png)

## 基本的な使い方

プラグインを導入すると、ゲーム中のメニューで「合成」が使えるようになります。

![Screen shot - Game menu](i/RTK1_Composite-03.ja.png)

ただし初期状態では合成のレシピが定義されていませんので、合成メニューの中は空です。

![Screen shot - Composite menu](i/RTK1_Composite-04.ja.png)

RPGツクールMVのデータベースを開き、合成メニューで生成したい対象(アイテム、武器、防具を)を選びます。今回はアイテムのPotion Ex(ポーション改) にしましょう。 そしてメモ欄に以下のように記入します。 これがこの対象の合成レシピとなります。

```
<composite:1,i1,2,0,0>
```
![Screen shot - Database item](i/RTK1_Composite-05.png)

はじめの 1 と、おわりの 0,0 はそのまま入力してください。 これらは次の章で説明します。

大事なのは2番目と3番目の値 i1,2 の部分です。 これは合成に必要な合成材料を指定しています。

値 i1,2 のうち、はじめの i1 は「IDが1のアイテム」を意味します。今回のケースですと、1番に登録されている Potion (ポーション) が該当します。 その後の 2 は必要な個数です。

つまりこの対象であるPotion Ex(ポーション改) を合成するためには、Portion(ポーション) が材料として2つ必要、だと定義されていることになります。

試しにこの合成レシピを、以下のように書き換えたらどうなるでしょうか？

```
<composite:1,i1,2,w1,1,a1,1,0,0>
```

合成材料の指定の部分が i1,2,w1,1,a1,1 と増えています。iがアイテムを示すように、wは武器を、aは防具を示します。よってこのレシピを設定した場合、合成にはPortion (ポーション) が2つに加え、武器のIDが1である Sword(ソード)が1つ、防具のIDが1であるShield(盾)が1つ、あわせて3種類の合成材料が必要ということになります。

合成材料は最大5種類まで指定できます。 最低でも1つの合成材料が指定されている必要があります。

さて合成レシピを i1,2 に戻して、次はプレイヤーに i6 であるPotion Ex(ポーション改)の合成レシピを覚えさせましょう。 マップに自動起動のイベントを追加し、プレイヤーに適当にアイテムを渡した後、以下のプラグインコマンドを使用します。

```
RTK1_Composite learn i6
```
※ カンマ区切りで複数まとめて指定することもできます

![Screen shot - Event edit](i/RTK1_Composite-06.png)

これでプレイヤーは i6、つまり ID 6のアイテムであるPotion Ex(ポーション改) の合成レシピを学びました。 さて、ゲーム中の合成メニューを実行してみましょう。

![Screen shot - Composite menu ja](i/RTK1_Composite-07.ja.png)

これでプレイヤーは冒険中はいつでも、Portion(ポーション)が2つあれば合成し、Potion Ex(ポーション改) を入手することができるようになりました。

なお本プラグインは、同じ RTK1シリーズの英語/日本語切り替えできる [RTK1_Option_EnJa プラグイン](RTK1_Option_EnJa.ja.md) にも対応しています。 以下が英語モードに切り替えたときの動作画面です。

![Screen shot - Composite menu en](i/RTK1_Composite-07.png)

## 合成レシピを忘れさせよう

いったん覚えた合成レシピはセーブファイルに自動保存され、以後、材料があればいつでも合成可能になります。

しかし、あるイベント限定の合成レシピなど、忘れてほしい場合もあるでしょう。 そんな場合には learn と同様に forget のプラグインコマンドで忘れさせることができます。

```
RTK1_Composite forget i6
```


## 費用を設定しよう

あまり気楽に合成されてしまうと、高価な品を売っている商店からクレームがくるかもしれませんね？そこで合成作業に費用を設定する方法を説明します。

```
<composite:1,i1,2,0,0>
```

前回、Potion Ex(ポーション改) のメモ欄に設定したこの合成レシピですが、最後の数字を 50 に変更してみましょう。

```
<composite:1,i1,2,0,50>
```

そう、この最後の数字が合成の費用なのです。 ゲームを実行してみると、合成に 50G の費用が取られるようになっているのが確認できます。

![Screen shot - Composite menu](i/RTK1_Composite-08.ja.png)

## 成功率を設定しよう

いつもいつも合成に成功していてはスリルがない？では合成レシピに成功率を設定してみましょう。

```
<composite:1,i1,2,0,50>
```

と設定されているメモ欄の最初の数値を 0.5 に減らしてみましょう。

```
<composite:0.5,i1,2,0,50>
```

そう、この最後の数字(0～1)が合成の成功率なのです。 ゲームを実行してみると、成功率が 50% に低下しているのがわかります。

![Screen shot - Composite menu](i/RTK1_Composite-09.ja.png)

なお今回の設定の場合、合成に失敗すると何も得られません。 丸損、ってやつですね。

## 合成失敗時に何か貰いたい

合成失敗したら何も貰えない設定、やっぱり評判悪いでしょうか？では、失敗しても何かアイテムを得ることができるようにしましょう。

```
<composite:0.5,i1,2,0,50>
```

はもうお馴染み、成功率 50% にしちゃったPotion Ex(ポーション改) の合成レシピです。 残ったひとつの謎の数字、最後から2番目の 0 を以下のようにアイテム指定に変更してみます。

```
<composite:0.5,i1,2,i1,50>
```

今回は地味に i1、つまり材料でもあるPortion(ポーション)を指定しています。 失敗しても、材料が1つ返ってくるから許してね、って感じでしょうか。

以下は合成に失敗した画面です。 何も出ないよりはマシ！って感じでしょうか。

![Screen shot - Composite menu](i/RTK1_Composite-10.ja.png)

この仕組みを前向きに利用して、成功率は非常に高いのですが、失敗すると高価なアイテムに化ける、という仕組みも提供できます。

例えば「刀」の成功率は99.9%なのですが、わずか1%の確率で「名刀」ができてしまう！というのも面白そうです。シティーハンターにおける「ワンオブサウザンド」な銃みたいなもの？

以上で、合成に関する基本的な利用方法は完了です。 うまく組み込んで、楽しいゲームを作ってください。

## ショップ機能

これからは中級者向けに、ショップ関連の機能を説明していきます。

合成のショップを開くには、以下のプラグインコマンドを使用します。

```
RTK1_Composite shop
```

すると以下のような「合成の店」が開きます。

![Screen shot - Composite shop](i/RTK1_Composite-11.ja.png)


表示される合成メニューですが、プレイヤーのようにいちいちレシピを覚える必要はなく、定義された合成レシピがすべて自動的に陳列されています。 冒険のキーアイテムが想定外にあっさり合成されてしまわぬよう、合成材料の設定には気をつけてください。

更に item/weapon/armor の引数を追加すると、以下のようにその項目の専門店になります。 合成レシピが多い時には店を分けると良いでしょう。

```
RTK1_Composite shop item
```

![Screen shot - Composite shop](i/RTK1_Composite-12.ja.png)

### ショップ名の変更

(書き途中)

```
RTK1_Composite shop item en_name
RTK1_Composite shop item en_name ja_name
RTK1_Composite shop all en_name ja_name
```

### カスタム・ショップ

チョイスした合成品だけを(書き途中)

```
RTK1_Composite clear
RTK1_Composite add i6
RTK1_Composite remove i6
RTK1_Composite shop custom
RTK1_Composite shop custom en_name ja_name
```

## ライセンス

[The MIT License (MIT)](https://opensource.org/licenses/mit-license.php) です。

提供されるjsファイルからコメント等を削除しないのであれば、著作権表示は不要です。 むろん表示いただくのは歓迎します！