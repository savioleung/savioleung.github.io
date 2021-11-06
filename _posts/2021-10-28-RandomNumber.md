---
layout: post
title: [C#,Unity] Number Battle Royale
categories: [C#, Unity]
tags: [demo, dbyll, dbtek, setup]
---

タイトル：Number Battle Royale

制作期間:2018/2~2018/3 約10時間

課題：Randomを使って何かを作る

コンセプト：「最強」の数字を出す乱数ジェネレーター

Github:[goalf](https://github.com/savioleung/goalf)

動画：

![random1](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/randomNum/random_1.png)

![random2](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/randomNum/random_2.png)

![random3](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/randomNum/random_3.png)

![random4](https://raw.githubusercontent.com/savioleung/savioleung.github.io/master/images/randomNum/random_4.png)
説明↓

プレイヤーに乱数を作る最小値と最大値を入力し、その間の数だけ、

数字を背負うボールをランダムに生成、

ボールごとに攻撃、防御、HP、攻撃速度、攻撃存在時間、走行速度

全部ランダムに生成し、戦わせる。


↓ボールをランダムに生成する
{% highlight csharp %}
//ボールを生成
public void randomm()
{
	minn = int.Parse(min.text);
	maxn = int.Parse(max.text);
	n2 = maxn - minn + 1;
	for (int i = 0; i < n2; i++)
	{

		balltextc = balltextp.GetComponentInChildren<Text>();
		balltextc.text = minn.ToString();
		Instantiate(ball, new Vector3(Random.Range(-10.0f, 10.0f), Random.Range(-4.0f, 4.0f), 0), Quaternion.identity);
		minn++;
	}
	Debug.Log(n2);
	uui.SetActive(false);
	b2.SetActive(true);
}
{% endhighlight %}
最初に一番近いボールをターゲットにして、相手が消えてから、別のボールをターゲットにします。

{% highlight csharp %}
	//移動
	void FindingBall()
	{
		if (nearObj == null)
		{
			nearObj = serchTag(gameObject, "ball");
			if (nearObj == null)
			{
				nearObj = this.gameObject;
			}
		}
		target = nearObj.transform.position;
		Vector3 norTar = (target - transform.position).normalized;
		float angle = Mathf.Atan2(norTar.y, norTar.x) * Mathf.Rad2Deg;

		Quaternion rotation = new Quaternion();
		rotation.eulerAngles = new Vector3(0, 0, angle - 90);
		transform.rotation = rotation;

		//move
		v.x = Mathf.Cos(Mathf.Deg2Rad * angle) * SPD;
		v.y = Mathf.Sin(Mathf.Deg2Rad * angle) * SPD;
		body.velocity = v;

	}
    //タグを探して、一番近いオブジェクトを返す
	GameObject serchTag(GameObject nowObj, string tagName)
	{
		float tmpDis = 0;
		float nearDis = 0;
		GameObject[] balls = GameObject.FindGameObjectsWithTag("ball");
		GameObject[] oballs = new GameObject[balls.Length - 1];
		int index = 0;

		for (int i = 0; i < balls.Length; i++)
		{
			if (balls[i] == this.gameObject)
				continue;

			oballs[index] = balls[i];
			index++;
		}
		if (oballs != null)
		{
			foreach (GameObject obs in oballs)
			{
				tmpDis = Vector3.Distance(obs.transform.position, nowObj.transform.position);
				if (nearDis == 0 || nearDis > tmpDis)
				{
					nearDis = tmpDis;
					targetObj = obs;
				}

			}
		}
		return targetObj;

	}
{% endhighlight %}

戦う法則は簡単にHP=HP-(ATK-DEF)、だが最低でも1ダメージは食らいます。

戦い抜いた最後のボールが一番「強い」数字になる。

ボールにはランダムにステイタスが振るわれる為、めちゃくちゃなステイタスになる、時折HPと防御が高く、攻撃が低いボールが2つ以上残っていて、中々決着付けない、時間の無駄になる為、ボールに色んな隠し効果を与えます。

先程言った戦う法則、「HP=HP-(ATK-DEF)、最低でも1ダメージは食らう」、この時、敵の防御を剝がす攻撃とみなし、敵の防御力を減らします

攻撃速度攻撃消えてもう一回攻撃するまでの時間、攻撃存在時間は攻撃出した後に消えるまでの時間、

一度出した攻撃は同じ相手に一度しかダメージ与えしない、

もし攻撃速度と攻撃存在時間が両方とも遅いボールがあれば、そのボールは重戦士とみなし、攻撃力を2倍にします

逆にもし攻撃速度が速いボールを軽戦士とみなし、攻撃するたび、攻撃が速いなっていきます


↓ステイタスを設定する
{% highlight csharp %}
void Awake()
	{

		body = GetComponent<Rigidbody2D> ();
		nearObj = serchTag(gameObject, "ball");
		HP = Random.Range (1, 100);
		ATK = Random.Range (1, 100);
		DEF = Random.Range (1, 100);
		SPD = Random.Range (0.5f, 5.0f);
		ATS = Random.Range (0.1f, 3.0f);
		RSP = Random.Range (0.1f, 3.0f);
		if (ATS>= 1.7f&& RSP>=1.1f) {
			ATK*=2;
		}
		myatk.SetActive (false);

	}
{% endhighlight %}

↓攻撃するプログラム
{% highlight csharp %}
	//攻撃
	void attacking()
	{
		//atk

		atkTime += 1 * Time.deltaTime;
		if (atkTime >= ATS)
		{
			myatk.SetActive(true);
			reloadtime += 1 * Time.deltaTime;

			if (reloadtime >= RSP)
			{
				if (ATS <= 1.0f) { ATS -= 0.03f; }
				myatk.SetActive(false);
				reloadtime = 0;
				atkTime = 0;
			}
		}
	}
{% endhighlight %}

↓攻撃を受けるプログラム
{% highlight csharp %}
	//HP計算
	void OnTriggerEnter2D(Collider2D other){
		if (other.gameObject.tag == "atk") {
			Debug.Log ("hit");

			float damg=other.gameObject.GetComponentInParent<findball> ().ATK -this.DEF;
			if (damg <= 1) {
				damg = 1;
				DEF -= 5;
			}
			HP -= damg;
		}
	}
{% endhighlight %}


