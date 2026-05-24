# 発展的な取り組み

ここからはより発展的な技術をいくつか紹介します。
気になったものは Claude Code に頼んで動かしてみたり、コードを読んで学ぶ材料にしたりしてみましょう。
紹介する技術にこだわらず、作ってみたいものをそのまま Claude Code に頼んでみるのもおすすめです。

## p5.js

p5.js はJavaScriptでクリエイティブなアートを作成するためのライブラリです。
簡単なコードでダイナミックなアニメーションやグラフィックスを描画することができます。

以下は p5.js を使って五芒星を描画するシンプルな例です。

```javascript
<!DOCTYPE html>
<html lang="ja">
<head>
  <title>p5.js</title>
  <style>
    body {
      margin: 0; /* 余白を完全に削除 */
    }
  </style>
  <script src="https://cdn.jsdelivr.net/npm/p5@1.11.1/lib/p5.min.js"></script>
</head>
<body>

<script>
function setup() {
  createCanvas(windowWidth, windowHeight); // ウィンドウ全体をキャンバスに
  background(0);
  fill(255, 255, 0); // 星の色を黄色に設定
  noStroke();
  drawStar(width / 2, height / 2, 100); // 中央に五芒星を描画
}

function drawStar(x, y, size) {
  let angle = PI / 2; // 開始角度
  let step = TWO_PI / 5; // 1/5円の角度

  beginShape();
  // 外側と内側の頂点を個別に計算
  vertex(x + cos(angle) * size, y - sin(angle) * size);
  vertex(x + cos(angle + step / 2) * (size / 2), y - sin(angle + step / 2) * (size / 2));
  vertex(x + cos(angle + step) * size, y - sin(angle + step) * size);
  vertex(x + cos(angle + step * 1.5) * (size / 2), y - sin(angle + step * 1.5) * (size / 2));
  vertex(x + cos(angle + step * 2) * size, y - sin(angle + step * 2) * size);
  vertex(x + cos(angle + step * 2.5) * (size / 2), y - sin(angle + step * 2.5) * (size / 2));
  vertex(x + cos(angle + step * 3) * size, y - sin(angle + step * 3) * size);
  vertex(x + cos(angle + step * 3.5) * (size / 2), y - sin(angle + step * 3.5) * (size / 2));
  vertex(x + cos(angle + step * 4) * size, y - sin(angle + step * 4) * size);
  vertex(x + cos(angle + step * 4.5) * (size / 2), y - sin(angle + step * 4.5) * (size / 2));
  endShape(CLOSE);
}
</script>

</body>
</html>
```

また、ループや分岐などより複雑なコードとなりますが、以下のような簡潔なコードで花火を描画することもできます。

```javascript
let particles = [];

function setup() {
  createCanvas(windowWidth, windowHeight); // ウィンドウ全体をキャンバスに
  background(0);
}

function draw() {
  background(0, 50); // 残像効果

  // 粒子を更新・描画
  for (let i = particles.length - 1; i >= 0; i--) {
    let p = particles[i];
    fill(...p.color, p.alpha);
    noStroke();
    ellipse(p.x, p.y, 6);
    p.x += p.vx;
    p.y += p.vy;
    p.vx *= 0.98; // 摩擦
    p.vy = p.vy * 0.98 + 0.1; // 重力
    p.alpha -= 5; // 透明度減少
    if (p.alpha <= 0) particles.splice(i, 1);
  }

  // 定期的に花火を生成
  if (frameCount % 60 === 0) {
    let x = random(width), y = random(height / 2);
    for (let i = 0; i < 100; i++) {
      let angle = random(TWO_PI);
      let velocity = random(2, 6);
      particles.push({
        x: x,
        y: y,
        vx: cos(angle) * velocity,
        vy: sin(angle) * velocity,
        alpha: 255,
        color: [random(255), random(255), random(255)],
      });
    }
  }
}
```

<https://p5js.org/> には様々なサンプルコードやリファレンスが掲載されているので、興味がある方はぜひ試してみてください。

こうしたアニメーションも、Claude Code に頼めば気軽に試せます。

```
p5.js を使って、カラフルな花火が打ち上がるページを作って
```

## Local Storage

これまでのコードは、ページをリロードするとデータが消えてしまうものでした。Local Storage を使うことで、ページをリロードしてもデータを保持することができます。

以下の例はLocal Storageを使って訪問回数をカウントするコードです。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <title>訪問カウンター</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css">
</head>
<body>

<main class="container">
  <h1>ようこそ！</h1>
  <p id="visitMessage"></p>
</main>

<script>
  // localStorageから訪問回数を取得し、数値に変換（初回訪問時は0）
  let visits = parseInt(localStorage.getItem('visits')) || 0;

  // 訪問回数を1増やして保存
  visits += 1;
  localStorage.setItem('visits', visits);

  // メッセージを表示
  const visitMessage = document.getElementById('visitMessage');
  if (visits === 1) {
    visitMessage.textContent = '初めての訪問ありがとうございます！';
  } else {
    visitMessage.textContent = `これはあなたの${visits}回目の訪問です！`;
  }
</script>
</body>
</html>
```

Local Storage 単体でできることは限られていますが、ブラウザの他の様々な機能と組み合わせて、より高度なウェブアプリケーションを作成することができます。

たとえば Claude Code に、こんなページを頼んでみましょう。

```
Local Storage を使って、やることリストのページを作って。タスクを追加・削除できて、ページを閉じても消えないようにして
```

## クラウドサービスとの連携
Local Storage はブラウザ内にデータを保存するための機能ですが、クラウドサービスを使うことでデータをサーバーに保存することもできます。

例えば以下のようなクラウドサービスを使うことでウェブアプリケーションで更に複雑な機能を実装することができます。

- Firebase <https://firebase.google.com/>
  - Googleが提供する、認証、データベース、プッシュ通知などの機能を持つクラウドサービスです
- Supabase <https://supabase.io/>
  - データベースや認証、簡易APIなどの機能を提供する人気のクラウドサービスです

例えば、以下のようなコードでFirebase Firestoreを使ったアクセスカウンターを実装することができます。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <title>Example Website</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@picocss/pico@2/css/pico.min.css" >
</head>
<body>
  <main class="container">
    <h1>Example Website</h1>

    <div>
      <h2>アクセスカウンター: <span id="counter"></span></h2>
    </div>
  </main>

  <script type="module">
    // Import the functions you need from the SDKs you need
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.4.0/firebase-app.js";
    import { getFirestore, doc, runTransaction } from "https://www.gstatic.com/firebasejs/11.4.0/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "your-api-key",
      authDomain: "your-project-id.firebaseapp.com",
      projectId: "your-project-id",
      storageBucket: "your-project-id.appspot.com",
      messagingSenderId: "your-messaging-sender-id",
      appId: "your-app-id"
    };

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const counterDoc = doc(db, 'counters', 'access');

    // カウンターを増やして表示
    const incrementCounter = async () => {
      let newCount = 0;
      await runTransaction(db, async (transaction) => {
        const docSnap = await transaction.get(counterDoc);
        newCount = (docSnap.exists() ? docSnap.data().count : 0) + 1;
        transaction.set(counterDoc, { count: newCount });
      });
      document.getElementById('counter').textContent = newCount;
    };

    // ページロード時にカウンターを増やして表示
    incrementCounter();
  </script>
</body>
</html>
```

## 自作バックエンドアプリケーションとの連携
フロントエンドだけでなく、これまでに使ったWeb APIのようなものを自分で作成することもできます。

自作バックエンドアプリケーションの作成や運用の手間はかかりますが、自由度が高く、複雑なウェブアプリケーションを構築できます。

## 発展課題

ここで紹介した技術にこだわらず、「こんなものがほしい」というお題を Claude Code に頼んで作ってみましょう。
作り方がわからなくても大丈夫です。やりたいことを伝えれば Claude Code が形にしてくれます。

たとえばこんなお題があります。気になったものを「〇〇を作って」と Claude Code に頼んでみてください。

- じゃんけんゲーム
- 時間が来たら音が鳴るタイマー
- ビンゴの数字をランダムに出す抽選機

できあがったらブラウザで動かしてみて、気に入らないところはさらに Claude Code に頼んで直していきましょう。

## 様々なウェブ知識を学ぶ
これまでの内容ではウェブ開発の基礎的な部分に触れてきましたが、ウェブ開発にはさまざまな技術が存在します。

以下のリソースを参考に、より深くウェブ開発について学ぶことができます。

- MDN Web Docs <https://developer.mozilla.org/ja/docs/Learn>
  - ウェブ開発の基礎から応用まで幅広く学ぶことができるリソースです

そして、わからないことがあったら Claude Code などのAIに **わかるまで聞く** のがオススメです。
「○○ってどういう意味？」「なぜこう書くの？」「もっと簡単に言うと？」のように、納得できるまで何度でも質問してみましょう。
コードを書いてもらうだけでなく、自分の理解を深めるための先生としてもAIは頼りになります。
