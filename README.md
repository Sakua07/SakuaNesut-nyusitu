# SakuaNesut-nyusitu
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <title>定期カウント</title>
  <style>
    body {
      font-family: sans-serif;
      background-color: #fdeff2;
      text-align: center;
      margin: 0;
      padding: 0 10px;
    }

    h1, h3, h5 {
      margin: 10px;
    }

    input[type="text"], textarea {
      width: 90%;
      margin: 10px 0;
      padding: 10px;
      font-size: 1rem;
      border-radius: 8px;
      border: 1px solid #ccc;
    }

    input[type="number"] {
      padding: 5px;
      font-size: 1rem;
      border-radius: 5px;
      border: 1px solid #ccc;
    }

    button {
      font-size: 1rem;
      border-radius: 10px;
      border: none;
      cursor: pointer;
    }

    .count {
      font-size: 1.5rem;
      margin: 20px 0;
      font-weight: bold;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 20px;
    }

    .counter-buttons {
      display: flex;
      flex-direction: column;
      gap: 10px;
    }

    #plusBtn {
      background-color: #ffcccc;
      padding: 0.5rem 1rem;
    }

    #minusBtn {
      background-color: #c1e1ff;
      padding: 0.5rem 1rem;
    }

    .edit-group {
      margin-bottom: 10px;
    }

    .edit-group input {
      width: 60px;
      margin-bottom: 4px;
    }

    .btn-group {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }

    .action-button {
      padding: 1rem 2rem;
      font-size: 1.2rem;
    }

    #outputText {
      width: 90%;
      height: 200px;
      margin-top: 20px;
    }

    .copy-share {
      margin-top: 20px;
    }

    .copy-share button {
      padding: 1.2rem 2.5rem;
      font-size: 1.2rem;
      margin: 10px;
      border-radius: 10px;
      border: none;
      cursor: pointer;
      background-color: #ffe4f2;
    }

    #shareAlert {
      display: none;
      background: #fff0f5;
      padding: 20px;
      border: 2px solid #ff69b4;
      border-radius: 10px;
      position: fixed;
      top: 20%;
      left: 50%;
      transform: translateX(-50%);
      z-index: 9999;
      text-align: center;
    }
  </style>
</head>
<body>

  <h2>🍎さくらアリの入室カウンターツール🌸</h2>
  <h5>🐝 上部分で文章の編集、ボタン上の数字入力で数値変更をしてね！カウンター横の＋、−、で間違いなどの修正ができるよ！🌸</h5>

  <h2><input type="text" id="titleInput" value="【定期】🌸おはアリー！！🍎🐝" oninput="updateOutput()"></h2>

  <textarea id="mainTextInput" rows="6" oninput="updateOutput()">本日デビューの『さくらアリ』です！
本日入室100人耐久〜
配信ポストでフォロー、配ポスで+3、バッチ取得で+5、バッチ進化で+10です🍎！
来てくれてすごーーく！ありがとう！
ゆっくりしてくれると嬉しいな</textarea>

  <div>
    <h4>🌸目標人数を設定↓</h4>
    <input type="number" id="goalInput" value="100" oninput="updateOutput()">
  </div>

  <div class="count">
    現在 <span id="count">0 / 100</span>
    <div class="counter-buttons">
      <button id="plusBtn" onclick="addCount(1)">＋</button>
      <button id="minusBtn" onclick="addCount(-1)">−</button>
    </div>
  </div>

  <div class="btn-group">
    <div class="edit-group">
      <input type="number" id="value1" value="3"><br>
      <button class="action-button" style="background-color:#ffd1dc;" onclick="addCountById('value1')">フォロー</button>
    </div>
    <div class="edit-group">
      <input type="number" id="value2" value="3"><br>
      <button class="action-button" style="background-color:#c1e1ff;" onclick="addCountById('value2')">配ポス</button>
    </div>
    <div class="edit-group">
      <input type="number" id="value3" value="5"><br>
      <button class="action-button" style="background-color:#d3ffc1;" onclick="addCountById('value3')">バッチ獲得</button>
    </div>
    <div class="edit-group">
      <input type="number" id="value4" value="10"><br>
      <button class="action-button" style="background-color:#ffe4b5;" onclick="addCountById('value4')">バッチ進化</button>
    </div>
    <div class="edit-group">
      <input type="number" id="value5" value="1"><br>
      <button class="action-button" style="background-color:#e0d0ff;" onclick="addCountById('value5')">特定ギフト</button>
    </div>
  </div>

  <textarea id="outputText" readonly></textarea>

  <div class="copy-share">
    <button onclick="copyText()">コピー</button>
    <button onclick="nativeShare()">共有</button>
  </div>

  <div id="shareAlert">
    <p style="font-size: 1.2rem;">🍎下記リンクを♡と🔁してから使ってね！<br>じゃないとさくらアリがしょんぼりしちゃうよ…(*ˊ˘ˋ*)</p>
    <input type="text" id="fixedLink" value="https://x.com/sakuraari_info" readonly style="width: 80%; padding: 10px; margin: 10px 0; font-size: 1rem; border-radius: 5px; border: 1px solid #ccc;">
    <br>
    <button onclick="copyFixedLink()" style="padding: 0.5rem 1.5rem; font-size: 1rem; border-radius: 8px; background-color: #ffe4f2; border: none; cursor: pointer;">リンクをコピー</button>
    <br><br>
    <button onclick="document.getElementById('shareAlert').style.display='none'" style="margin-top: 10px; font-size: 0.9rem; background-color: #ffcccc; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer;">OK</button>
  </div>

  <script>
    let count = 0;
    let goalReached = false;

    function updateOutput() {
      const title = document.getElementById("titleInput").value;
      const mainText = document.getElementById("mainTextInput").value;
      const goal = parseInt(document.getElementById("goalInput").value, 10) || 100;
      const output = `${title}\n${mainText}\n\n現在${count}/${goal}`;
      document.getElementById("outputText").value = output;
      document.getElementById("count").innerText = `${count} / ${goal}`;
    }

    function addCount(num) {
      const goal = parseInt(document.getElementById("goalInput").value, 10) || 100;
      count = Math.max(0, count + num);
      updateOutput();
      if (count >= goal && !goalReached) {
        alert("🌸おめでとう！達成したよ🍎🐝〜");
        goalReached = true;
      }
    }

    function addCountById(id) {
      const value = parseInt(document.getElementById(id).value, 10);
      if (!isNaN(value)) {
        addCount(value);
      }
    }

    function copyText() {
      const text = document.getElementById("outputText");
      text.select();
      document.execCommand("copy");
      alert("コピーしました！");
    }

    function nativeShare() {
      const text = document.getElementById("outputText").value;
      if (navigator.share) {
        navigator.share({
          title: "定期カウント",
          text: text,
        });
      } else {
        alert("このブラウザでは共有機能がサポートされていません。");
      }
    }

    function copyFixedLink() {
      const linkInput = document.getElementById("fixedLink");
      linkInput.select();
      linkInput.setSelectionRange(0, 99999);
      document.execCommand("copy");
      alert("リンクをコピーしました！");
    }

    // 修正：初期表示時にも updateOutput を呼ぶ
    window.onload = function () {
      document.getElementById("shareAlert").style.display = "block";
      updateOutput();
    };
  </script>
</body>
</html>
