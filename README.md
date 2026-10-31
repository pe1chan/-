<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>住吉食堂 予約画面</title>

  <style>
    body {
      font-family: "Helvetica", "Arial", sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f5f5f7;
      color: #333;
    }

    header {
      background-color: #000;
      color: white;
      padding: 15px;
      text-align: center;
      font-size: 20px;
      font-weight: bold;
    }

    .container {
      padding: 20px;
      max-width: 600px;
      margin: auto;
      padding-bottom: 90px;
    }

    .menu-card {
      background: white;
      border-radius: 16px;
      padding: 20px;
      margin: 15px 0;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
      cursor: pointer;
    }

    .section {
      display: none;
      background: white;
      border-radius: 16px;
      padding: 20px;
      margin-top: 20px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.08);
    }

    .item-button {
      background-color: #007aff;
      color: white;
      border: none;
      border-radius: 12px;
      padding: 12px 20px;
      font-size: 16px;
      margin: 10px 0;
      width: 100%;
      text-align: left;
      cursor: pointer;
    }

    .back-button {
      background-color: #333;
      color: white;
      border: none;
      border-radius: 12px;
      padding: 12px 20px;
      font-size: 16px;
      margin-bottom: 20px;
      width: 100%;
      cursor: pointer;
    }

    .ticket-box {
      text-align: center;
      background: #eef7ff;
      border: 2px dashed #007aff;
      border-radius: 12px;
      padding: 20px;
      margin-top: 15px;
    }

    .ticket-number {
      font-size: 48px;
      font-weight: bold;
      color: #007aff;
      margin: 10px 0;
    }

    /* フォーム用共通スタイル */
    .form-group {
      margin-bottom: 15px;
    }

    .form-group label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }

    .form-group input {
      width: 100%;
      padding: 10px;
      box-sizing: border-box;
      border: 1px solid #ccc;
      border-radius: 8px;
      font-size: 16px;
    }

    .link-btn {
      color: #007aff;
      text-decoration: underline;
      background: none;
      border: none;
      cursor: pointer;
      font-size: 14px;
      display: block;
      margin-top: 15px;
      text-align: center;
      width: 100%;
    }

    /* カート関係のスタイル */
    .cart-float-btn {
      position: fixed;
      bottom: 20px;
      left: 50%;
      transform: translateX(-50%);
      background-color: #34c759;
      color: white;
      border: none;
      border-radius: 30px;
      padding: 15px 30px;
      font-size: 18px;
      font-weight: bold;
      box-shadow: 0 4px 15px rgba(0,0,0,0.2);
      cursor: pointer;
      z-index: 1000;
    }

    .cart-item {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 12px 0;
      border-bottom: 1px solid #eee;
    }

    .delete-btn {
      background-color: #ff3b30;
      color: white;
      border: none;
      border-radius: 8px;
      padding: 6px 12px;
      cursor: pointer;
    }

    .qty-control-btn {
      padding: 4px 10px;
      margin: 0 2px;
      border: 1px solid #ccc;
      background: #fff;
      border-radius: 6px;
      cursor: pointer;
    }
  </style>
</head>

<body>
  <header id="header-title">ログイン</header>

  <div class="container">

    <!-- 1. ログイン画面（初期表示） -->
    <div id="login" class="section" style="display:block;">
      <h2>ログイン</h2>
      <div class="form-group">
        <label>学籍番号：</label>
        <input type="text" id="login-student-id" placeholder="例: 12345678">
      </div>
      <div class="form-group">
        <label>パスワード：</label>
        <input type="password" id="login-password" placeholder="パスワード">
      </div>
      <button class="item-button" style="background-color: #007aff; text-align: center;" onclick="handleLogin()">ログイン</button>
      <button class="link-btn" onclick="showSection('register')">アカウントをお持ちでない方はこちら（新規登録）</button>
    </div>

    <!-- 2. 新規会員登録画面 -->
    <div id="register" class="section">
      <h2>新規アカウント登録</h2>
      <div class="form-group">
        <label>学籍番号：</label>
        <input type="text" id="reg-student-id" placeholder="例: 12345678">
      </div>
      <div class="form-group">
        <label>パスワード（6文字以上）：</label>
        <input type="password" id="reg-password" placeholder="パスワード">
      </div>
      <button class="item-button" style="background-color: #34c759; text-align: center;" onclick="handleRegister()">登録してログイン</button>
      <button class="link-btn" onclick="showSection('login')">すでに登録済みの方はこちら（ログイン）</button>
    </div>

    <!-- 3. ホーム画面（メニュー一覧） -->
    <div id="home" class="section">
      <h2>メニュー</h2>

      <div class="menu-card" onclick="showSection('setmenu')"><h3>定食</h3></div>
      <div class="menu-card" onclick="showSection('rice')"><h3>ご飯もの</h3></div>
      <div class="menu-card" onclick="showSection('men1')"><h3>麺類①</h3></div>
      <div class="menu-card" onclick="showSection('men2')"><h3>麺類②</h3></div>
      <div class="menu-card" onclick="showSection('other')"><h3>その他</h3></div>
    </div>

    <!-- 定食 -->
    <div id="setmenu" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>定食</h2>

      <button class="item-button" onclick="reserve('スペシャル定食 520円', 520)">スペシャル定食 520円</button>
      <button class="item-button" onclick="reserve('スペシャル定食 大無し 540円', 540)">スペシャル定食 大無し 540円</button>
      <button class="item-button" onclick="reserve('スペシャル定食 大大無し 580円', 580)">スペシャル定食 大大無し 580円</button>
      <button class="item-button" onclick="reserve('スペシャル定食 大有り 560円', 560)">スペシャル定食 大有り 560円</button>
      <button class="item-button" onclick="reserve('スペシャル定食 大大有り 600円', 600)">スペシャル定食 大大有り 600円</button>

      <button class="item-button" onclick="reserve('からあげ定食 490円', 490)">からあげ定食 490円</button>
      <button class="item-button" onclick="reserve('からあげ定食 大無し 510円', 510)">からあげ定食 大無し 510円</button>
      <button class="item-button" onclick="reserve('からあげ定食 大大無し 550円', 550)">からあげ定食 大大無し 550円</button>
      <button class="item-button" onclick="reserve('からあげ定食 大有り 530円', 530)">からあげ定食 大有り 530円</button>
      <button class="item-button" onclick="reserve('からあげ定食 大大有り 570円', 570)">からあげ定食 大大有り 570円</button>
    </div>

    <!-- ご飯もの -->
    <div id="rice" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>ご飯もの</h2>

      <button class="item-button" onclick="reserve('からあげ丼 450円', 450)">からあげ丼 450円</button>
      <button class="item-button" onclick="reserve('からあげ丼 大 490円', 490)">からあげ丼 大 490円</button>
      <button class="item-button" onclick="reserve('からあげ丼 大大 530円', 530)">からあげ丼 大大 530円</button>
      <button class="item-button" onclick="reserve('からあげ丼 大大大 570円', 570)">からあげ丼 大大大 570円</button>

      <button class="item-button" onclick="reserve('味噌から丼 450円', 450)">味噌から丼 450円</button>
      <button class="item-button" onclick="reserve('味噌から丼 大 490円', 490)">味噌から丼 大 490円</button>
      <button class="item-button" onclick="reserve('味噌から丼 大大 530円', 530)">味噌から丼 大大 530円</button>
      <button class="item-button" onclick="reserve('味噌から丼 大大大 570円', 570)">味噌から丼 大大大 570円</button>

      <button class="item-button" onclick="reserve('親子丼 450円', 450)">親子丼 450円</button>
      <button class="item-button" onclick="reserve('親子丼 大 490円', 490)">親子丼 大 490円</button>
      <button class="item-button" onclick="reserve('親子丼 大大 530円', 530)">親子丼 大大 530円</button>
      <button class="item-button" onclick="reserve('親子丼 大大大 570円', 570)">親子丼 大大大 570円</button>

      <button class="item-button" onclick="reserve('カレーライス 350円', 350)">カレーライス 350円</button>
      <button class="item-button" onclick="reserve('カレーライス 大 390円', 390)">カレーライス 大 390円</button>
      <button class="item-button" onclick="reserve('カレーライス 大大 430円', 430)">カレーライス 大大 430円</button>
      <button class="item-button" onclick="reserve('カレーライス 大大大 470円', 470)">カレーライス 大大大 470円</button>
    </div>

    <!-- 麺類① -->
    <div id="men1" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>麺類①</h2>

      <button class="item-button" onclick="reserve('うどん 240円', 240)">うどん 240円</button>
      <button class="item-button" onclick="reserve('うどん 大 280円', 280)">うどん 大 280円</button>
      <button class="item-button" onclick="reserve('うどん 大大 320円', 320)">うどん 大大 320円</button>
      <button class="item-button" onclick="reserve('うどん 大大大 360円', 360)">うどん 大大大 360円</button>

      <button class="item-button" onclick="reserve('そば 240円', 240)">そば 240円</button>
      <button class="item-button" onclick="reserve('そば 大 280円', 280)">そば 大 280円</button>
      <button class="item-button" onclick="reserve('そば 大大 320円', 320)">そば 大大 320円</button>
      <button class="item-button" onclick="reserve('そば 大大大 360円', 360)">そば 大大大 360円</button>

      <button class="item-button" onclick="reserve('きつねうどん 260円', 260)">きつねうどん 260円</button>
      <button class="item-button" onclick="reserve('きつねうどん 大 300円', 300)">きつねうどん 大 300円</button>
      <button class="item-button" onclick="reserve('きつねうどん 大大 340円', 340)">きつねうどん 大大 340円</button>
      <button class="item-button" onclick="reserve('きつねうどん 大大大 380円', 380)">きつねうどん 大大大 380円</button>

      <button class="item-button" onclick="reserve('たぬきそば 260円', 260)">たぬきそば 260円</button>
      <button class="item-button" onclick="reserve('たぬきそば 大 300円', 300)">たぬきそば 大 300円</button>
      <button class="item-button" onclick="reserve('たぬきそば 大大 340円', 340)">たぬきそば 大大 340円</button>
      <button class="item-button" onclick="reserve('たぬきそば 大大大 380円', 380)">たぬきそば 大大大 380円</button>

      <button class="item-button" onclick="reserve('玉子うどん 270円', 270)">玉子うどん 270円</button>
      <button class="item-button" onclick="reserve('玉子うどん 大 310円', 310)">玉子うどん 大 310円</button>
      <button class="item-button" onclick="reserve('玉子うどん 大大 350円', 350)">玉子うどん 大大 350円</button>
      <button class="item-button" onclick="reserve('玉子うどん 大大大 390円', 390)">玉子うどん 大大大 390円</button>

      <button class="item-button" onclick="reserve('玉子そば 270円', 270)">玉子そば 270円</button>
      <button class="item-button" onclick="reserve('玉子そば 大 310円', 310)">玉子そば 大 310円</button>
      <button class="item-button" onclick="reserve('玉子そば 大大 350円', 350)">玉子そば 大大 350円</button>
      <button class="item-button" onclick="reserve('玉子そば 大大大 390円', 390)">玉子そば 大大大 390円</button>

      <button class="item-button" onclick="reserve('冷やしうどん 360円', 360)">冷やしうどん 360円</button>
      <button class="item-button" onclick="reserve('冷やしうどん 大 400円', 400)">冷やしうどん 大 400円</button>
      <button class="item-button" onclick="reserve('冷やしうどん 大大 440円', 440)">冷やしうどん 大大 440円</button>
      <button class="item-button" onclick="reserve('冷やしうどん 大大大 480円', 480)">冷やしうどん 大大大 480円</button>

      <button class="item-button" onclick="reserve('冷やしそば 360円', 360)">冷やしそば 360円</button>
      <button class="item-button" onclick="reserve('冷やしそば 大 400円', 400)">冷やしそば 大 400円</button>
      <button class="item-button" onclick="reserve('冷やしそば 大大 440円', 440)">冷やしそば 大大 440円</button>
      <button class="item-button" onclick="reserve('冷やしそば 大大大 480円', 480)">冷やしそば 大大大 480円</button>
    </div>

    <!-- 麺類② -->
    <div id="men2" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>麺類②</h2>

      <button class="item-button" onclick="reserve('温うどん 360円', 360)">温うどん 360円</button>
      <button class="item-button" onclick="reserve('温うどん 大 400円', 400)">温うどん 大 400円</button>
      <button class="item-button" onclick="reserve('温うどん 大大 440円', 440)">温うどん 大大 440円</button>
      <button class="item-button" onclick="reserve('温うどん 大大大 480円', 480)">温うどん 大大大 480円</button>

      <button class="item-button" onclick="reserve('温そば 360円', 360)">温そば 360円</button>
      <button class="item-button" onclick="reserve('温そば 大 400円', 400)">温そば 大 400円</button>
      <button class="item-button" onclick="reserve('温そば 大大 440円', 440)">温そば 大大 440円</button>
      <button class="item-button" onclick="reserve('温そば 大大大 480円', 480)">温そば 大大大 480円</button>

      <button class="item-button" onclick="reserve('天かすうどん 270円', 270)">天かすうどん 270円</button>
      <button class="item-button" onclick="reserve('天かすうどん 大 310円', 310)">天かすうどん 大 310円</button>
      <button class="item-button" onclick="reserve('天かすうどん 大大 350円', 350)">天かすうどん 大大 350円</button>
      <button class="item-button" onclick="reserve('天かすうどん 大大大 390円', 390)">天かすうどん 大大大 390円</button>

      <button class="item-button" onclick="reserve('天かすそば 270円', 270)">天かすそば 270円</button>
      <button class="item-button" onclick="reserve('天かすそば 大 310円', 310)">天かすそば 大 310円</button>
      <button class="item-button" onclick="reserve('天かすそば 大大 350円', 350)">天かすそば 大大 350円</button>
      <button class="item-button" onclick="reserve('天かすそば 大大大 390円', 390)">天かすそば 大大大 390円</button>

      <button class="item-button" onclick="reserve('カレーうどん 300円', 300)">カレーうどん 300円</button>
      <button class="item-button" onclick="reserve('カレーうどん 大 340円', 340)">カレーうどん 大 340円</button>
      <button class="item-button" onclick="reserve('カレーうどん 大大 380円', 380)">カレーうどん 大大 380円</button>
      <button class="item-button" onclick="reserve('カレーうどん 大大大 420円', 420)">カレーうどん 大大大 420円</button>

      <button class="item-button" onclick="reserve('カレーそば 300円', 300)">カレーそば 300円</button>
      <button class="item-button" onclick="reserve('カレーそば 大 340円', 340)">カレーそば 大 340円</button>
      <button class="item-button" onclick="reserve('カレーそば 大大 380円', 380)">カレーそば 大大 380円</button>
      <button class="item-button" onclick="reserve('カレーそば 大大大 420円', 420)">カレーそば 大大大 420円</button>

      <button class="item-button" onclick="reserve('チャンポン 320円', 320)">チャンポン 320円</button>
      <button class="item-button" onclick="reserve('チャンポン 大 360円', 360)">チャンポン 大 360円</button>
      <button class="item-button" onclick="reserve('チャンポン 大大 400円', 400)">チャンポン 大大 400円</button>
      <button class="item-button" onclick="reserve('チャンポン 大大大 440円', 440)">チャンポン 大大大 440円</button>

      <button class="item-button" onclick="reserve('鶏がら醤油ラーメン 390円', 390)">鶏がら醤油ラーメン 390円</button>
      <button class="item-button" onclick="reserve('鶏がら醤油ラーメン 大 430円', 430)">鶏がら醤油ラーメン 大 430円</button>
      <button class="item-button" onclick="reserve('鶏がら醤油ラーメン 大大 470円', 470)">鶏がら醤油ラーメン 大大 470円</button>
      <button class="item-button" onclick="reserve('鶏がら醤油ラーメン 大大大 510円', 510)">鶏がら醤油ラーメン 大大大 510円</button>
    </div>

    <!-- その他 -->
    <div id="other" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>その他</h2>

      <button class="item-button" onclick="reserve('サラダ 300円', 300)">サラダ 300円</button>
      <button class="item-button" onclick="reserve('デザート 400円', 400)">デザート 400円</button>
    </div>

    <!-- カート追加・数量選択フォーム -->
    <div id="reserve" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>選択メニュー確認</h2>

      <p id="selected-menu" style="font-size:18px; font-weight:bold;"></p>

      <label>数量：</label>
      <input type="number" id="quantity" value="1" min="1" style="width:100%; padding:10px; margin-bottom:15px; box-sizing: border-box;">

      <button class="item-button" style="background-color: #34c759;" onclick="addToCart()">カートに追加する</button>
    </div>

    <!-- カート確認・一括予約確定画面 -->
    <div id="cart" class="section">
      <button class="back-button" onclick="showSection('home')">ホームへ戻る</button>
      <h2>カート・予約確認</h2>

      <div id="cart-items-list" style="margin-bottom: 20px;">
        <!-- カートの中身がここにJavaScriptで動的表示されます -->
      </div>

      <p id="cart-total" style="font-size: 18px; font-weight: bold; text-align: right; margin-bottom: 20px;"></p>

      <div id="checkout-form">
        <label>学籍番号：</label>
        <input type="text" id="studentId" readonly style="width:100%; padding:10px; margin-bottom:15px; box-sizing: border-box; background-color: #f0f0f0;">

        <label>受取時間：</label>
        <input type="time" id="time" style="width:100%; padding:10px; margin-bottom:15px; box-sizing: border-box;">

        <button id="submit-btn" class="item-button" style="background-color: #007aff;" onclick="submitReservation()">予約を確定する</button>
      </div>
    </div>

    <!-- 完了画面（受付番号表示） -->
    <div id="completed" class="section">
      <h2>ご注文ありがとうございます！</h2>
      <p>以下の受付番号でお呼びいたします。</p>
      
      <div class="ticket-box">
        <div>受付番号</div>
        <div id="display-ticket-number" class="ticket-number">---</div>
      </div>

      <p style="margin-top:20px; font-size:14px; color:#666; text-align:center;">
        ※学食の受け取りカウンターにて番号が呼ばれましたら、お越しください。
      </p>

      <button class="back-button" onclick="showSection('home')" style="margin-top:20px;">トップに戻る</button>
    </div>

  </div> <!-- container -->

  <!-- カート表示用フロートボタン -->
  <button id="cart-float-btn" class="cart-float-btn" onclick="showSection('cart')" style="display:none;">
    🛒 カートを見る (<span id="cart-count">0</span>)
  </button>

  <!-- 画面切り替え ＆ カート管理スクリプト -->
  <script>
    let cart = [];
    let selectedMenuName = "";
    let selectedMenuPrice = 0;
    let loggedInStudentId = ""; // ログイン済みの学籍番号

    function showSection(id) {
      const sections = ["login", "register", "home", "setmenu", "rice", "men1", "men2", "other", "reserve", "cart", "completed"];
      sections.forEach(sec => {
        const el = document.getElementById(sec);
        if (el) el.style.display = "none";
      });

      document.getElementById(id).style.display = "block";
      
      const titleMap = {
        login: 'ログイン',
        register: '新規登録',
        home: 'ホーム',
        setmenu: '定食',
        rice: 'ご飯もの',
        men1: '麺類①',
        men2: '麺類②',
        other: 'その他',
        reserve: '数量選択',
        cart: 'カート確認',
        completed: '予約完了'
      };
      document.getElementById("header-title").innerText = titleMap[id] || id;

      if (id === 'cart') {
        renderCart();
      }
    }

    // 商品選択時に数量入力画面を開く
    function reserve(menuName, price = 0) {
      selectedMenuName = menuName;
      selectedMenuPrice = price;
      document.getElementById("selected-menu").innerText = "選択中のメニュー： " + menuName;
      document.getElementById("quantity").value = "1";
      showSection("reserve");
    }

    // カートに追加して自動的にホーム画面に戻る
    function addToCart() {
      const qty = parseInt(document.getElementById("quantity").value, 10);
      if (isNaN(qty) || qty <= 0) {
        alert("有効な数量を入力してください");
        return;
      }

      // カートに同一商品があれば数量を加算
      const existingItem = cart.find(item => item.name === selectedMenuName);
      if (existingItem) {
        existingItem.quantity += qty;
      } else {
        cart.push({
          name: selectedMenuName,
          price: selectedMenuPrice,
          quantity: qty
        });
      }

      updateCartBadge();
      alert(selectedMenuName + " をカートに追加しました！");
      
      // 一つ頼んだ後はホーム画面に戻る
      showSection("home");
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      updateCartBadge();
      renderCart();
    }

    function updateQuantity(index, delta) {
      cart[index].quantity += delta;
      if (cart[index].quantity <= 0) {
        cart.splice(index, 1);
      }
      updateCartBadge();
      renderCart();
    }

    function updateCartBadge() {
      const totalCount = cart.reduce((sum, item) => sum + item.quantity, 0);
      document.getElementById("cart-count").innerText = totalCount;
      const floatBtn = document.getElementById("cart-float-btn");

      // ログイン前（login, register画面）ではフロートボタンを非表示
      const currentTitle = document.getElementById("header-title").innerText;
      if (totalCount > 0 && currentTitle !== 'ログイン' && currentTitle !== '新規登録') {
        floatBtn.style.display = "block";
      } else {
        floatBtn.style.display = "none";
      }
    }

    function renderCart() {
      const listContainer = document.getElementById("cart-items-list");
      const totalContainer = document.getElementById("cart-total");
      const checkoutForm = document.getElementById("checkout-form");

      if (cart.length === 0) {
        listContainer.innerHTML = "<p style='text-align:center; color:#777;'>カートに商品は入っていません。</p>";
        totalContainer.innerText = "";
        checkoutForm.style.display = "none";
        return;
      }

      checkoutForm.style.display = "block";
      let html = "";
      let totalSum = 0;

      cart.forEach((item, index) => {
        const subtotal = item.price * item.quantity;
        totalSum += subtotal;
        html += `
          <div class="cart-item">
            <div>
              <strong>${item.name}</strong><br>
              <small>${item.quantity}個 ${subtotal > 0 ? ' (小計: ' + subtotal + '円)' : ''}</small>
            </div>
            <div>
              <button class="qty-control-btn" onclick="updateQuantity(${index}, 1)">＋</button>
              <button class="qty-control-btn" onclick="updateQuantity(${index}, -1)">－</button>
              <button class="delete-btn" onclick="removeFromCart(${index})">削除</button>
            </div>
          </div>
        `;
      });

      listContainer.innerHTML = html;
      totalContainer.innerText = totalSum > 0 ? "合計金額: " + totalSum + "円" : "";
    }
  </script>

  <!-- Firebase 認証・保存処理 -->
  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/12.16.0/firebase-app.js";
    import { 
      getFirestore, 
      collection, 
      addDoc,
      doc,
      runTransaction
    } from "https://www.gstatic.com/firebasejs/12.16.0/firebase-firestore.js";
    import {
      getAuth,
      createUserWithEmailAndPassword,
      signInWithEmailAndPassword
    } from "https://www.gstatic.com/firebasejs/12.16.0/firebase-auth.js";

    const firebaseConfig = {
      apiKey: "AIzaSyATA4tG9PZjawp2mqcTjbjO5daPRjEgAQc",
      authDomain: "sumiyoshi-cafeteria.firebaseapp.com",
      projectId: "sumiyoshi-cafeteria",
      storageBucket: "sumiyoshi-cafeteria.firebasestorage.app",
      messagingSenderId: "401250022282",
      appId: "1:401250022282:web:2c9f681fc4107a5f758e73",
      measurementId: "G-KNEZQ215HB"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const auth = getAuth(app);

    // 学籍番号をFirebase用の擬似メールアドレスに変換
    function idToEmail(studentId) {
      return `${studentId.trim()}@student.local`;
    }

    // --- 新規会員登録処理 ---
    window.handleRegister = async function() {
      const studentId = document.getElementById("reg-student-id").value.trim();
      const password = document.getElementById("reg-password").value;

      if (!studentId || !password) {
        alert("学籍番号とパスワードを入力してください");
        return;
      }
      if (password.length < 6) {
        alert("パスワードは6文字以上で入力してください");
        return;
      }

      try {
        const email = idToEmail(studentId);
        await createUserWithEmailAndPassword(auth, email, password);
        window.loggedInStudentId = studentId;
        document.getElementById("studentId").value = studentId;
        alert("アカウント登録が完了しました！");
        showSection("home");
      } catch (error) {
        console.error("登録エラー:", error);
        if (error.code === 'auth/email-already-in-use') {
          alert("この学籍番号はすでに登録されています。ログイン画面からログインしてください。");
        } else {
          alert("登録に失敗しました: " + error.message);
        }
      }
    };

    // --- ログイン処理 ---
    window.handleLogin = async function() {
      const studentId = document.getElementById("login-student-id").value.trim();
      const password = document.getElementById("login-password").value;

      if (!studentId || !password) {
        alert("学籍番号とパスワードを入力してください");
        return;
      }

      try {
        const email = idToEmail(studentId);
        await signInWithEmailAndPassword(auth, email, password);
        window.loggedInStudentId = studentId;
        document.getElementById("studentId").value = studentId;
        showSection("home");
      } catch (error) {
        console.error("ログインエラー:", error);
        alert("ログインに失敗しました。学籍番号またはパスワードが正しくありません。");
      }
    };

    // 本日の日付文字列(YYYY-MM-DD)を取得
    function getTodayString() {
      const today = new Date();
      const year = today.getFullYear();
      const month = String(today.getMonth() + 1).padStart(2, '0');
      const day = String(today.getDate()).padStart(2, '0');
      return `${year}-${month}-${day}`;
    }

    // 1日ごとにリセットされる連番（1, 2, 3...）を取得・インクリメント
    async function getNextTicketNumber() {
      const todayStr = getTodayString();
      const counterRef = doc(db, "daily_counters", todayStr);

      let nextNum = 1;

      await runTransaction(db, async (transaction) => {
        const counterDoc = await transaction.get(counterRef);
        if (!counterDoc.exists()) {
          transaction.set(counterRef, { count: 1 });
          nextNum = 1;
        } else {
          const currentCount = counterDoc.data().count || 0;
          nextNum = currentCount + 1;
          transaction.update(counterRef, { count: nextNum });
        }
      });

      return nextNum;
    }

    // カート全体の注文を確定する処理
    window.submitReservation = async function () {
      const studentId = document.getElementById("studentId").value.trim();
      const time = document.getElementById("time").value;
      const submitBtn = document.getElementById("submit-btn");

      if (!studentId || !time) {
        alert("学籍番号と受取時間を入力してください");
        return;
      }

      if (cart.length === 0) {
        alert("カートが空です");
        return;
      }

      // ボタン二重押し防止
      submitBtn.disabled = true;
      submitBtn.innerText = "送信中...";

      try {
        // 当日連番の発番
        const ticketNumber = await getNextTicketNumber();

        // 厨房用画面と互換性のある形式でFirestoreに一括保存
        await addDoc(collection(db, "reservations"), {
          studentId: studentId,
          time: time,
          items: cart, // カート内の全商品を配列で保持
          ticketNumber: ticketNumber,
          orderNumber: ticketNumber, // 互換性保持
          status: "pending",
          createdAt: new Date()
        });

        // 完了画面に受付番号を表示
        document.getElementById("display-ticket-number").innerText = ticketNumber + " 番";
        
        // フォーム＆カートの初期化
        cart = [];
        updateCartBadge();
        document.getElementById("time").value = "";

        showSection("completed");
      } catch (e) {
        console.error("注文エラー:", e);
        alert("予約に失敗しました。もう一度お試しください。");
      } finally {
        submitBtn.disabled = false;
        submitBtn.innerText = "予約を確定する";
      }
    };
  </script>

</body>
</html>
