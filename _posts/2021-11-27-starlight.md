---
layout: post
title: Starlighter[C#,Unity][更新中]
categories: [C#,Unity]
tags: []
description: ! オールレンジ攻撃体感シューティングアクション
---
タイトル：Starlighter

制作期間:2020/7~2020/9 約50時間

コンセプト：オールレンジ攻撃シューティングアクション

Github:[Starlighter](https://github.com/savioleung/Starlighter_Project)

動画：

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_1.png)

説明：

個人で自由に制作したゲーム

目的地にビットを放って、放たれたビットをコントロールし射撃するゲームを作ってみたいの欲望のままに作ったゲーム



![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_2.png)<br>

ビット以外で本体も射撃出来る、放たれたビームはビットと同じビーム

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_3.png)<br>

右下のUIです、緑色は現在使用出来るビット、オレンジ色はビットの最大値となります、

同時にビットはこのゲームのHPとなります、

設定上、敵から攻撃を受ける前に、ビットが自動的に飛び出て防衛を行う、プレイヤーは守られたが、ビット1機失う設定

ビットの最大値が減って、スキルの必要ビット数に達さない場合、スキルが使えなくなる、

ビットがの最大値0になったら、プレイヤーはスキル一切使えなくなって、銃のみでステージ続行、攻撃されると即死です。

使用出来るビットが0になってもチャージされるまで一時的に同じ状態になります。

ビットは放たれたら、数が減ります、ビットが使用されて戻ったら、3秒後に現在使用出来るビットの数が戻る、その3秒は設定上エネルギーチャージの時間


![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_5.png)<br>

ゲームの左下のUI

武器／スキルスロットです、キーボードの1,2,3,4で切り替え出来ます

１は銃、本体の銃、これを選んでいる時にマウス左クリックで射撃を行う、弾薬は無限、設定上バックパックに無限核エネルギー装置搭載


２はビット、マウス右クリックでビットを右クリックした位置まで放つ、
![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_6.png)<br>

ビットを放たれた状態で左クリックしたら、左クリックした位置に向かってビームを撃つ、その後プレイヤーに戻ってチャージを行う

ビットがない状態、ビットが使用済みで戻ってくる途中しかいない状態で左クリックしても何も発生しません。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_7.png)<br>

３は足場、３を選んだ状態になったら、マウス位置に足場の配置予定地が表示

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_8.png)<br>

左クリックしたら足場に向かってビットを4つ発射

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_9.png)<br>

ビットが目的地に到着したら足場を生成、

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_10.png)<br>

上に乗る事ができます

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_11.png)<br>

足場は下からすり抜けて乗る事もできます

3の足場を選んだ状態で右クリック押したら、足場を解除して戻ってチャージする

3を選ばないと戻ってこないので忘れると使用出来るビットが4個減ったままになります。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_12.png)<br>

最後の４はシールド、その前に

↓それは次のステージに行く為ののゲート

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_13.png)<br>

その前に赤いビーム撃ち続けている処があります、

入ると触ったビームの数だけダメージが入って弾かれる、右下のビーム最大値で確認出来る。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_14.png)<br>

シールドを選んだ状態では、足場をと同じ、シールド配置予定地を確認出来る


![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_15.png)<br>

足場をと同じ、ビットが向かって、シールドが生成、シールドの大きさは大体プレイヤーと同じくらい、

シールドは敵のビーム攻撃を防衛出来るが、敵本体の侵入は防衛出来ない

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_16.png)
<br>

このようにシールド張ればゲートに入れる、

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_17.png)
<br>

足場をと同じ、シールドを選んだ状態で右クリックでシールドを解除して戻ってチャージする

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_18.png)<br>

そのまま右に進んだらクリアです

以下はクリアUI

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_19.png)<br>

敵です

敵はジャンプしながらビーム撃ってくる

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_20.png)<br>

敵は数回撃たれたら弱点が出現します、このように正面の銃では撃てない位置です

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_21.png)<br>

敵を殺すにはビットを張って

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_22.png)<br>

弱点撃ったら死にます。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_23.png)<br>

もしくは通り抜けての一瞬でも撃つ機会があります（厳しい）

<details>
    <summary>＋</summary>
    {% highlight csharp %}

{% endhighlight %}
</details>

