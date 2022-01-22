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
<iframe width="560" height="315" src="https://www.youtube.com/embed/XR9hMZEjMHo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

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

<details>
    <summary>＋ビットのプログラマ</summary>
    {% highlight csharp %}
    private void Start()
    {
        //プレイヤーを探す
        player = GameObject.FindGameObjectWithTag("Player").GetComponent<Player>();
        //バックパックを探す       
        bitBag = GameObject.Find("bitBag");
        //プレイヤーがスキル「ビット」を選んでいる場合
        if (player.skill == 2)
        {   //目的地をクリックした座標に、ビームが撃てるように
            vec = Camera.main.ScreenToWorldPoint(Input.mousePosition);
            this.canShoot = true;
        }
        else
        {//「ビット」以外の場合、打たない
            this.canShoot = false;
            
        }
        //初期化
        this.afterUse = false;
        this.onTarget = false;
        upMoveTime = 2.0f;
        rotPow = 5.0f;

    }
    void Update()
    {
        //ビットを常に回転する
        rotateBit();
        //目的地チェック
        if(this.transform.position.x==vec.x&& this.transform.position.y == vec.y)
        {
            onTarget = true;
        }
        //プレイヤーがビットを選んでいる場合にマウスクリックで射撃
        if (player.skill == 2)
        {
            ShootBeam();
        }
        //使い終わったらバックパックに戻る
        if (afterUse)
        {
            vec = bitBag.transform.position;

        }
        //移動
        transform.position = Vector2.MoveTowards(transform.position, new Vector2(vec.x, vec.y), bitSpeed * Time.deltaTime);

    }
    //射出したビットを回転する動きをつける
     void rotateBit()
    {
        if (upMoveTime > 1)
        {
            upMoveTime *= 0.98f;
            transform.Translate(0, 0.1f, 0);
        }
        transform.Rotate(0, 0, rotPow);
    }
    //射撃処理
    void ShootBeam()
    {
        //マウスクリック
        if (Input.GetMouseButtonDown(0) && canShoot)
        {
            GameObject laser = Instantiate(Beam, transform.position, Quaternion.identity);
            // クリックした座標の取得（スクリーン座標からワールド座標に変換）
            Vector3 mouseWorldPos = Camera.main.ScreenToWorldPoint(Input.mousePosition);

            // 向きの生成（Z成分の除去と正規化）
            Vector3 shotForward = Vector3.Scale((mouseWorldPos - transform.position), new Vector3(1, 1, 0)).normalized;

            // 弾に速度を与える
            laser.GetComponent<Rigidbody2D>().velocity = shotForward * laserSpeed;

            Destroy(laser, 2);
        }//撃ったら戻る処理
        if (Input.GetMouseButtonUp(0) && canShoot)
        {
            canShoot = false;
            upMoveTime = 2.0f;
            afterUse = true;
        }

    }
    private void OnTriggerEnter2D(Collider2D collision)
    {

        //戻っていく時、バックパックに触れて初めてチャージする
        if (collision.gameObject == bitBag && afterUse)
        {
            //3秒チャージして、使用可能になる
            player.invokeFunc("chargeBit", 3);
            Destroy(gameObject);
        }
    }
{% endhighlight %}
</details>



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

<details>
<summary>＋スキルのプログラマ</summary>
{% highlight csharp %}
    
 #region スロット変更
        var key = Input.inputString;
        switch (key)
        {
            case "1":
                skill = 1;
                break;
            case "2":
                skill = 2;
                break;
            case "3":
                skill = 3;

                for (int i = 0; i < 4; i++)
                {
                    multiBit[i].transform.localPosition = new Vector3(-0.9f + 0.6f * i, -0.1f, 0);
                }
                break;
            case "4":
                multiBit[0].transform.localPosition = new Vector3(-0.6f, 1f);
                multiBit[1].transform.localPosition = new Vector3(-0.6f, -1f);
                multiBit[2].transform.localPosition = new Vector3(0.6f, -1f);
                multiBit[3].transform.localPosition = new Vector3(0.6f, 1f);
                skill = 4;
                break;
            default:
                break;
        }
        skillSlotSelect(skill);
        #endregion

        //銃
        if (Input.GetMouseButtonDown(0) && skill == 1)
        {
            GameObject laser = Instantiate(Beamlaser, handGunBit.transform.position, Quaternion.identity);
            // クリックした座標の取得（スクリーン座標からワールド座標に変換）
            Vector3 mouseWorldPos = Camera.main.ScreenToWorldPoint(Input.mousePosition);

            // 向きの生成（Z成分の除去と正規化）
            Vector3 shotForward = Vector3.Scale((mouseWorldPos - transform.position), new Vector3(1, 1, 0)).normalized;

            // 弾に速度を与える
            laser.GetComponent<Rigidbody2D>().velocity = shotForward * gunLaserSpeed;

            Destroy(laser, 1);
        }
        //ビット発射
        if (Input.GetMouseButtonDown(1) && bitCount > 0 && skill == 2)
        {
            GameObject cloneBit = Instantiate(gunBit, bitSpwan.position, Quaternion.identity);
            bitCount--;
        }
        //足場/シールド
        if (skill == 3 || skill == 4)
        {
            skill34(skill);
        }
        else { steper.SetActive(false); }

        bitText.text = bitCount+"";
        maxBitText.text=bitMaxCount+"";
    }




{% endhighlight %}
</details>

<details>
<summary>＋シールドと足場の目的地に到着チェックのプログラマ</summary>
{% highlight csharp %}
    private void skill34(int s)
    {
        Vector2 aimSpot = Camera.main.ScreenToWorldPoint(Input.mousePosition);
        steper.SetActive(true);
        steper.transform.position = aimSpot;

        if (Input.GetMouseButtonDown(0) && bitCount >= 4)
        {
            //ビットー4
            bitCount -= 4;
            //ビットをすべてリストから排除する
            bitList.Clear();
            for (int i = 0; i < 4; i++)
            {
                cloneBit2[i] = Instantiate(gunBit, bitSpwan.position, Quaternion.identity);
                cloneBit2[i].GetComponent<BitController>().vec = multiBit[i].transform.position;
                cloneBit2[i].GetComponent<BitController>().canShoot = false;
                //一段ビットをリストに入れる
                bitList.Add(cloneBit2[i]);
            }
            //狙い先の場所でオブジェを生成
            GameObject targetObj = Instantiate(floorEff, aimSpot, Quaternion.identity) as GameObject;

        }
    }
{% endhighlight %}
</details>


<details>
<summary>＋シールドと足場のエフェクト管理のプログラマ</summary>
{% highlight csharp %}
 
    private void Start()
    {   
        player = GameObject.FindGameObjectWithTag("Player").GetComponent<Player>();
        skillNum = player.skill;
        //呼び出された時のプレイヤーのスキルに応じてエフェクト生成
        if (skillNum == 3)
        {
            effObj = floorEff.gameObject ;
           }
        else if (skillNum == 4)
        {
            effObj = shieldEff;
        }
        eff = Instantiate(effObj, this.transform.position, Quaternion.identity) as GameObject;
        //そのエフェクトを子オブジェクトにする
        eff.transform.parent = this.transform;
        //存在を一時的消す
        eff.SetActive(false);
    }
    private void Update()
    {
        //生成時に自分に使うビットをリストに入る
        if (useingBit.Count == 0 && stage == 0)
        {
            foreach (GameObject i in player.bitList)
            {
                useingBit.Add(i);
            }
            stage++;
        }
        //ビット全部リストに入ったらこのステージに入る
        if (stage == 1)
        {
            //ビット全部目的地に到達したらエフェクト始動
            skiller(useingBit);
        }
        //エフェクト始動して、プレイヤーがエフェクトのスキルを選んでいる場合、マウス右クリックでビット回収
        if (Input.GetMouseButtonDown(1)&&stage==2&&player.skill==skillNum)
        {
            bitReturn(useingBit);
            Destroy(this.gameObject);
        }
        
    }
    //スキル使用
    public void skiller(List<GameObject> cloneBit)
    {
        //ビット何個目的地に到達したかチェック
        int pointCheck = 0;
        //ビットがいる場合
        if (cloneBit != null)
        {
            for (int i = 0; i < cloneBit.Count; i++)
            {   //目的地に到達したかチェック
                if (cloneBit[i].GetComponent<BitController>().onTarget)
                {
                    pointCheck++;

                }
            }
        }
        //ビットが全部到達したらエフェクト始動
        if (pointCheck == 4) {
            eff.SetActive(true);
            stage = 2;
        }
    }
    //ビット回収
    void bitReturn(List<GameObject> cloneBit)
    {
        eff.SetActive(false);
        for (int i = 0; i < cloneBit.Count; i++)
        {
            cloneBit[i].GetComponent<BitController>().afterUse = true;

        }
    }
{% endhighlight %}
</details>


敵です

敵はプレイヤーが近くに行くと起動します

敵はジャンプしながらビーム撃ってくる、敵がプレイヤーのビームを受けたらノックバックが発生してダメージ処理する。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_20.png)<br>

敵は数回撃たれたら弱点が出現します、このように正面の銃では撃てない位置です

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_21.png)<br>

敵を殺すにはビットを張って

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_22.png)<br>

弱点撃ったら死にます。

![Starlighter](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/starlighter/starlighter_23.png)<br>

もしくは通り抜けての一瞬でも撃つ機会があります（厳しい）


<details>
    <summary>＋敵のプログラマ</summary>
    {% highlight csharp %}
 virtual protected void Start()
    {
        //プレイヤー
        player = player = GameObject.FindGameObjectWithTag("Player");
        rb = GetComponent<Rigidbody2D>();
        //弱点の位置初期化
        weakPoint.transform.position = weakPointPos.transform.position;
        //弱点露出まで弱点を消す
        weakPoint.gameObject.SetActive(false);

    }

    // Update is called once per frame
    virtual protected void Update()
    {
        //プレイヤーとの距離
        var dis = Vector3.Distance(player.transform.position, this.transform.position);
        //プレイヤーが距離内で始動
        if (dis < r && !move)
        {
            move = true;
        }
        if (move)
        {
            t += Time.deltaTime;
            //タイムごとに動く
            if (t >= reloadTime)
            {
                GameObject laser = Instantiate(beamLaser, shootPos.transform.position, Quaternion.identity);
                // プレイヤーの座標
                Vector3 targetPos = player.transform.position;

                // 向きの生成
                Vector3 shotForward = Vector3.Scale((targetPos - transform.position), new Vector3(1, 1, 0)).normalized;

                // 弾に速度を与える
                laser.GetComponent<Rigidbody2D>().velocity = shotForward * laserSpeed;

                turn();
                movement();

                Destroy(laser, goneTime);
                t = 0;

            }
            //弱点露出処理
            if (HP <= 0)
            {
                if (!breakable)
                {
                    weakPoint.gameObject.SetActive(true);
                    if (weakPoint.GetComponent<weakPoint>().isDeath)
                    {
                        Destroy(this.gameObject);
                    }
                }
                else
                {
                    Destroy(this.gameObject);
                }
            }
        }
    }
    void turn()
    {
        if (player.transform.position.x > transform.position.x)
        {
            this.transform.localScale = new Vector3(1.35f, this.transform.localScale.y, this.transform.localScale.z);
            h = 1;
        }
        else
        {
            this.transform.localScale = new Vector3(-1.35f, this.transform.localScale.y, this.transform.localScale.z);
            h = -1;
        }
    }
    void movement()
    {
        //ジャンプ処理
        if (onGround)
        {
            onGround = false;
            rb.AddForce(new Vector2(Random.Range(0.4f, 1.2f) * h, Random.Range(0.4f, 1.2f)) * jumpSpeed, ForceMode2D.Force);
        }
    }

    virtual protected void knockBack(GameObject g)
    {
        Vector3 hitPos = g.transform.position;

        // 向きの生成
        Vector3 hitForward = Vector3.Scale((hitPos - transform.position), new Vector3(1, 1, 0)).normalized;
        rb.velocity = Vector2.zero;
        rb.AddForce(new Vector2(hitForward.x, hitForward.y>0? hitForward.y*-1  : hitForward.y)*400*-1, ForceMode2D.Force);
    }
{% endhighlight %}
</details>


<details>
    <summary>制作した感想</summary>
人が遊べるには早すぎた、色々と、カメラとか、戦闘の流れとか、時代とか
</details>