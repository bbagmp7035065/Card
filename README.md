<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>抽卡遊戲</title>
  <style>
    body {
      margin: 0;
      padding: 0;
      background: url("https://i.postimg.cc/BbMSWcJZ/zip-1.jpg") no-repeat center center fixed;
      background-size: cover;
      font-family: "Noto Sans TC", sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
    }

    .card-container {
      text-align: center;
    }

    .card img {
      width: 90vw; /* 寬度佔螢幕 90% */
      max-width: 600px; /* 電腦上最多 600px */
      height: auto;
      border-radius: 12px;
      box-shadow: 0 4px 20px rgba(0,0,0,0.3);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      cursor: pointer;
    }

    .card img:hover {
      transform: scale(1.05);
      box-shadow: 0 6px 30px rgba(0,0,0,0.5);
    }

    .buttons {
      margin-top: 20px;
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      justify-content: center;
    }

    button {
      background: rgba(255, 255, 255, 0.8);
      border: none;
      padding: 12px 20px;
      border-radius: 8px;
      font-size: 18px;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s ease;
    }

    button:hover {
      background: rgba(255, 255, 255, 1);
      transform: translateY(-2px);
    }

    /* 放大模式 */
    .fullscreen {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      background-color: rgba(0, 0, 0, 0.85);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 9999;
    }

    .fullscreen img {
      width: 90vw;
      max-width: 700px;
      height: auto;
      border-radius: 12px;
    }

    .back-btn {
      position: absolute;
      top: 20px;
      right: 20px;
      background: rgba(255, 255, 255, 0.9);
      padding: 10px 16px;
      border-radius: 8px;
      cursor: pointer;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <div class="card-container">
    <div class="card" id="card">
      <img src="https://i.postimg.cc/MTYxRbMV/zip-2.jpg" alt="卡片" id="cardImage">
    </div>

    <div class="buttons">
      <button onclick="drawCard('one')">抽一星卡</button>
      <button onclick="drawCard('two')">抽二星卡</button>
      <button onclick="drawCard('three')">抽三星卡</button>
    </div>
  </div>

  <div id="fullscreen" class="fullscreen" style="display:none;">
    <img id="fullscreenImage" src="" alt="放大圖">
    <div class="back-btn" onclick="closeFullscreen()">返回</div>
  </div>

  <script>
    const oneStarCards = [
      "https://i.postimg.cc/jj0Cs2Qy/zip-10.jpg","https://i.postimg.cc/mrfhbkNy/zip-11.jpg","https://i.postimg.cc/HLDjpn44/zip-12.jpg","https://i.postimg.cc/9f5zW0tb/zip-13.jpg","https://i.postimg.cc/rpXz8sCQ/zip-14.jpg","https://i.postimg.cc/jj0Cs2QM/zip-15.jpg","https://i.postimg.cc/LsKhH5By/zip-16.jpg","https://i.postimg.cc/QMGVXCky/zip-17.jpg","https://i.postimg.cc/pdby2rY1/zip-18.jpg","https://i.postimg.cc/vmrTDy37/zip-19.jpg","https://i.postimg.cc/W4GztVX6/zip-20.jpg","https://i.postimg.cc/25Qy3Y27/zip-21.jpg","https://i.postimg.cc/hGLvhnpr/zip-22.jpg","https://i.postimg.cc/Dzr0mhB5/zip-23.jpg","https://i.postimg.cc/PqsNtJzP/zip-4.jpg","https://i.postimg.cc/1zh49XcF/zip-5.jpg","https://i.postimg.cc/vmFDQTL9/zip-6.jpg","https://i.postimg.cc/bvcdzrH2/zip-7.jpg","https://i.postimg.cc/hGWhSvbd/zip-8.jpg","https://i.postimg.cc/d0PDq3mR/zip-9.jpg"
    ];

    const twoStarCards = [
      "https://i.postimg.cc/bYH5gBBr/zip-24.jpg","https://i.postimg.cc/HsYhh9qj/zip-25.jpg","https://i.postimg.cc/3J8ccFsJ/zip-26.jpg","https://i.postimg.cc/HsYhh9qk/zip-27.jpg","https://i.postimg.cc/WbNHHwB3/zip-28.jpg","https://i.postimg.cc/PrXRRQgW/zip-29.jpg","https://i.postimg.cc/DyvMMP9s/zip-30.jpg","https://i.postimg.cc/5N4RRShC/zip-31.jpg","https://i.postimg.cc/SNSvvfBD/zip-32.jpg","https://i.postimg.cc/c4xbbBpT/zip-33.jpg","https://i.postimg.cc/CLhQQs3j/zip-34.jpg","https://i.postimg.cc/TYt4MWxr/zip-35.jpg","https://i.postimg.cc/RVSDDLkg/zip-36.jpg","https://i.postimg.cc/Qx8yyJ26/zip-37.jpg","https://i.postimg.cc/xT64DbQy/zip-38.jpg","https://i.postimg.cc/VLKVQbw9/zip-39.jpg","https://i.postimg.cc/ZKwMtym8/zip-40.jpg","https://i.postimg.cc/L6xbKZSW/zip-41.jpg","https://i.postimg.cc/qM15VCr5/zip-42.jpg","https://i.postimg.cc/gkgTF6pt/zip-43.jpg","https://i.postimg.cc/rFfPXtTB/zip-44.jpg","https://i.postimg.cc/xT64DbQW/zip-45.jpg","https://i.postimg.cc/L6xbKZSG/zip-46.jpg","https://i.postimg.cc/dt5xPTvF/zip-47.jpg","https://i.postimg.cc/5NsRdF1M/zip-48.jpg"
    ];

    const threeStarCards = [
      "https://i.postimg.cc/SRDDhHB9/zip-49.jpg","https://i.postimg.cc/1XJJQbkV/zip-50.jpg","https://i.postimg.cc/4yBBgjD7/zip-51.jpg","https://i.postimg.cc/WzXXvxBd/zip-52.jpg","https://i.postimg.cc/VvDD82xL/zip-53.jpg","https://i.postimg.cc/mk88sKvb/zip-54.jpg","https://i.postimg.cc/cCXXSPpx/zip-55.jpg","https://i.postimg.cc/D0BBTR9n/zip-56.jpg","https://i.postimg.cc/2yz2gCK3/zip-57.jpg","https://i.postimg.cc/XqVQtnzJ/zip-58.jpg","https://i.postimg.cc/nrH0NnPL/zip-59.jpg","https://i.postimg.cc/zvJ04qcG/zip-60.jpg","https://i.postimg.cc/QCX0vjyN/zip-61.jpg","https://i.postimg.cc/6q9YFBmW/zip-62.jpg","https://i.postimg.cc/sxVwb3Ns/zip-63.jpg","https://i.postimg.cc/zvJ04qcq/zip-64.jpg","https://i.postimg.cc/NFgpZQzq/zip-65.jpg","https://i.postimg.cc/7hxmjqWv/zip-66.jpg","https://i.postimg.cc/6q9YFB1k/zip-67.jpg","https://i.postimg.cc/T1RCBTHZ/zip-68.jpg","https://i.postimg.cc/vTQ3CGSP/zip-69.jpg","https://i.postimg.cc/Fz9Dwhnn/zip-70.jpg","https://i.postimg.cc/Wz2XxsfK/zip-71.jpg","https://i.postimg.cc/vTQ3CGSK/zip-72.jpg","https://i.postimg.cc/3RjtLk9V/zip-73.jpg"
    ];

    function drawCard(star) {
      let pool = star === 'one' ? oneStarCards :
                 star === 'two' ? twoStarCards : threeStarCards;
      const randomIndex = Math.floor(Math.random() * pool.length);
      document.getElementById("cardImage").src = pool[randomIndex];
    }

    document.getElementById("cardImage").addEventListener("click", () => {
      const imgSrc = document.getElementById("cardImage").src;
      document.getElementById("fullscreenImage").src = imgSrc;
      document.getElementById("fullscreen").style.display = "flex";
    });

    function closeFullscreen() {
      document.getElementById("fullscreen").style.display = "none";
    }
  </script>
</body>
</html>
