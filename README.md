<!DOCTYPE html>
<html lang="zh-TW">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1.0" />
<title>在愛中看見</title>
<style>
  :root{
    --ui-bg: rgba(255,255,255,0.92);
    --accent: #ffd700;
  }
  html,body{height:100%;margin:0;}
  body{
    font-family: "Noto Sans TC", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
    background: url("https://i.postimg.cc/BbMSWcJZ/zip-1.jpg") center/cover no-repeat fixed;
    display:flex;
    align-items:center;
    justify-content:center;
    color:#111;
    -webkit-font-smoothing:antialiased;
  }

  .wrap{
    width:100%;
    max-width:1100px;
    padding:20px;
    box-sizing:border-box;
    text-align:center;
  }

  h1{
    margin:6px 0 14px 0;
    color:#fff;
    text-shadow:0 4px 18px rgba(0,0,0,0.6);
    font-size:28px;
  }

  /* intro card */
  .intro {
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:10px;
    margin-top:6vh;
  }
  .intro .info-img{
    width:76vw;
    max-width:700px;
    border-radius:12px;
    box-shadow:0 8px 30px rgba(0,0,0,0.55);
    cursor:pointer;
    transition:transform .25s ease;
  }
  .intro .info-img:active{ transform:scale(.98) }

  /* main game area */
  .game {
    display:none;
    margin-top:24px;
  }

  .card-wrap{
    margin: 18px 0;
    display:flex;
    justify-content:center;
  }

  /* card (thumbnail) */
  .card {
    display:inline-block;
    cursor:pointer;
    transition: transform .45s cubic-bezier(.2,.9,.2,1), opacity .3s ease;
    transform-origin:center center;
    opacity:0;
  }

  .card img{
    width:75vw;            /* 未放大時約螢幕3/4 */
    max-width:720px;      /* 桌面最大寬度 */
    height:auto;
    border-radius:12px;
    box-shadow:0 8px 30px rgba(0,0,0,0.45);
    display:block;
    will-change:transform;
  }

  .card.show{ opacity:1; transform:scale(1) translateY(0); }

  /* 卡片切換顯著動畫：flip + fade */
  .flip-out{
    animation:flipOut .36s forwards;
  }
  .flip-in{
    animation:flipIn .36s forwards;
  }
  @keyframes flipOut {
    0% { transform: rotateY(0deg) scale(.95); opacity:1; }
    100% { transform: rotateY(90deg) scale(.6); opacity:0; }
  }
  @keyframes flipIn {
    0% { transform: rotateY(-90deg) scale(.6); opacity:0; }
    100% { transform: rotateY(0deg) scale(1); opacity:1; }
  }

  /* control buttons */
  .controls{
    display:flex;
    gap:12px;
    justify-content:center;
    flex-wrap:wrap;
    margin-top:14px;
  }
  .btn{
    background:var(--ui-bg);
    border-radius:10px;
    border:0;
    padding:12px 18px;
    font-weight:700;
    cursor:pointer;
    box-shadow:0 6px 18px rgba(0,0,0,0.25);
    transition:transform .12s ease, box-shadow .12s ease;
  }
  .btn:active{ transform:translateY(2px) }
  .btn.primary{ background: linear-gradient(90deg,var(--accent), #ffb84d); color:#111; }

  /* fullscreen preview */
  .preview-overlay{
    position:fixed; inset:0;
    display:none;
    align-items:center;
    justify-content:center;
    background:rgba(0,0,0,0.88);
    z-index:9999;
  }
  .preview-overlay.show{ display:flex; }
  .preview-img{
    width:96vw; max-width:980px; max-height:96vh;
    object-fit:contain; border-radius:10px;
    box-shadow:0 20px 50px rgba(0,0,0,0.7);
    transition:transform .2s ease;
  }
  .preview-close{
    position:fixed; top:18px; right:18px;
    background:var(--ui-bg); border-radius:8px; padding:8px 12px;
    cursor:pointer; font-weight:700; box-shadow:0 6px 18px rgba(0,0,0,0.25);
  }

  /* small loader text */
  .loader{
    color:#fff; margin-top:12px; font-size:14px; text-shadow:0 3px 8px rgba(0,0,0,0.6);
  }

  /* responsive tweaks */
  @media(min-width:900px){
    h1{font-size:34px;}
    .intro .info-img{ width:58vw; }
    .card img{ width:56vw; max-width:720px; }
  }
</style>
</head>
<body>
  <div class="wrap">
    <h1>在愛中看見</h1>

    <!-- Intro flow -->
    <div class="intro" id="intro">
      <img id="introImg" class="info-img" src="https://i.postimg.cc/MTYxRbMV/zip-2.jpg" alt="說明卡（點擊繼續）">
      <div class="loader" id="introHint">點擊卡片閱讀說明 →</div>
    </div>

    <!-- Game area -->
    <div class="game" id="game">
      <div class="card-wrap">
        <div class="card" id="card">
          <img id="cardImg" src="" alt="抽到的卡">
        </div>
      </div>

      <div class="controls" id="startControls">
        <button id="startBtn" class="btn primary">開始遊戲</button>
      </div>

      <div class="controls" id="drawControls" style="display:none; margin-top:18px;">
        <button class="btn" data-rarity="1">抽一星</button>
        <button class="btn" data-rarity="2">抽二星</button>
        <button class="btn" data-rarity="3">抽三星</button>
      </div>

      <div class="loader" id="status" style="display:none;color:#fff;"></div>
    </div>
  </div>

  <!-- Fullscreen preview -->
  <div class="preview-overlay" id="preview">
    <img id="previewImg" class="preview-img" src="" alt="檢視卡片">
    <div class="preview-close" id="previewClose">返回</div>
  </div>

<script>
/* =========================
   完整70張卡圖陣列（直接使用你給的 Postimages 直鏈）
   一星 / 二星 / 三星
   ========================= */
const oneStar = [
  "https://i.postimg.cc/jj0Cs2Qy/zip-10.jpg","https://i.postimg.cc/mrfhbkNy/zip-11.jpg","https://i.postimg.cc/HLDjpn44/zip-12.jpg",
  "https://i.postimg.cc/9f5zW0tb/zip-13.jpg","https://i.postimg.cc/rpXz8sCQ/zip-14.jpg","https://i.postimg.cc/jj0Cs2QM/zip-15.jpg",
  "https://i.postimg.cc/LsKhH5By/zip-16.jpg","https://i.postimg.cc/QMGVXCky/zip-17.jpg","https://i.postimg.cc/pdby2rY1/zip-18.jpg",
  "https://i.postimg.cc/vmrTDy37/zip-19.jpg","https://i.postimg.cc/W4GztVX6/zip-20.jpg","https://i.postimg.cc/25Qy3Y27/zip-21.jpg",
  "https://i.postimg.cc/hGLvhnpr/zip-22.jpg","https://i.postimg.cc/Dzr0mhB5/zip-23.jpg","https://i.postimg.cc/PqsNtJzP/zip-4.jpg",
  "https://i.postimg.cc/1zh49XcF/zip-5.jpg","https://i.postimg.cc/vmFDQTL9/zip-6.jpg","https://i.postimg.cc/bvcdzrH2/zip-7.jpg",
  "https://i.postimg.cc/hGWhSvbd/zip-8.jpg","https://i.postimg.cc/d0PDq3mR/zip-9.jpg"
];

const twoStar = [
  "https://i.postimg.cc/bYH5gBBr/zip-24.jpg","https://i.postimg.cc/HsYhh9qj/zip-25.jpg","https://i.postimg.cc/3J8ccFsJ/zip-26.jpg",
  "https://i.postimg.cc/HsYhh9qk/zip-27.jpg","https://i.postimg.cc/WbNHHwB3/zip-28.jpg","https://i.postimg.cc/PrXRRQgW/zip-29.jpg",
  "https://i.postimg.cc/DyvMMP9s/zip-30.jpg","https://i.postimg.cc/5N4RRShC/zip-31.jpg","https://i.postimg.cc/SNSvvfBD/zip-32.jpg",
  "https://i.postimg.cc/c4xbbBpT/zip-33.jpg","https://i.postimg.cc/CLhQQs3j/zip-34.jpg","https://i.postimg.cc/TYt4MWxr/zip-35.jpg",
  "https://i.postimg.cc/RVSDDLkg/zip-36.jpg","https://i.postimg.cc/Qx8yyJ26/zip-37.jpg","https://i.postimg.cc/xT64DbQy/zip-38.jpg",
  "https://i.postimg.cc/VLKVQbw9/zip-39.jpg","https://i.postimg.cc/ZKwMtym8/zip-40.jpg","https://i.postimg.cc/L6xbKZSW/zip-41.jpg",
  "https://i.postimg.cc/qM15VCr5/zip-42.jpg","https://i.postimg.cc/gkgTF6pt/zip-43.jpg","https://i.postimg.cc/rFfPXtTB/zip-44.jpg",
  "https://i.postimg.cc/xT64DbQW/zip-45.jpg","https://i.postimg.cc/L6xbKZSG/zip-46.jpg","https://i.postimg.cc/dt5xPTvF/zip-47.jpg",
  "https://i.postimg.cc/5NsRdF1M/zip-48.jpg"
];

const threeStar = [
  "https://i.postimg.cc/SRDDhHB9/zip-49.jpg","https://i.postimg.cc/1XJJQbkV/zip-50.jpg","https://i.postimg.cc/4yBBgjD7/zip-51.jpg",
  "https://i.postimg.cc/WzXXvxBd/zip-52.jpg","https://i.postimg.cc/VvDD82xL/zip-53.jpg","https://i.postimg.cc/mk88sKvb/zip-54.jpg",
  "https://i.postimg.cc/cCXXSPpx/zip-55.jpg","https://i.postimg.cc/D0BBTR9n/zip-56.jpg","https://i.postimg.cc/2yz2gCK3/zip-57.jpg",
  "https://i.postimg.cc/XqVQtnzJ/zip-58.jpg","https://i.postimg.cc/nrH0NnPL/zip-59.jpg","https://i.postimg.cc/zvJ04qcG/zip-60.jpg",
  "https://i.postimg.cc/QCX0vjyN/zip-61.jpg","https://i.postimg.cc/6q9YFBmW/zip-62.jpg","https://i.postimg.cc/sxVwb3Ns/zip-63.jpg",
  "https://i.postimg.cc/zvJ04qcq/zip-64.jpg","https://i.postimg.cc/NFgpZQzq/zip-65.jpg","https://i.postimg.cc/7hxmjqWv/zip-66.jpg",
  "https://i.postimg.cc/6q9YFB1k/zip-67.jpg","https://i.postimg.cc/T1RCBTHZ/zip-68.jpg","https://i.postimg.cc/vTQ3CGSP/zip-69.jpg",
  "https://i.postimg.cc/Fz9Dwhnn/zip-70.jpg","https://i.postimg.cc/Wz2XxsfK/zip-71.jpg","https://i.postimg.cc/vTQ3CGSK/zip-72.jpg",
  "https://i.postimg.cc/3RjtLk9V/zip-73.jpg"
];

/* =========== 元素 =========== */
const intro = document.getElementById('intro');
const introImg = document.getElementById('introImg');
const introHint = document.getElementById('introHint');

const game = document.getElementById('game');
const startBtn = document.getElementById('startBtn');
const drawControls = document.getElementById('drawControls');
const drawButtons = document.querySelectorAll('.btn[data-rarity]');
const card = document.getElementById('card');
const cardImg = document.getElementById('cardImg');
const status = document.getElementById('status');

const preview = document.getElementById('preview');
const previewImg = document.getElementById('previewImg');
const previewClose = document.getElementById('previewClose');

const startControls = document.getElementById('startControls');
const drawControlsDiv = document.getElementById('drawControls');
const buttonsDiv = document.getElementById('drawControls');

/* =========== flow control =========== */
let introStage = 0; // 0=show info1, 1=info2, 2=game ready
let inPreview = false;

/* =========== 預載所有圖片（背景與說明與70張） =========== */
const preloadList = [
  "https://i.postimg.cc/MTYxRbMV/zip-2.jpg",
  "https://i.postimg.cc/RFT9fQ61/zip-3.jpg",
  ...oneStar, ...twoStar, ...threeStar
];

function preloadAll(list, onProgress, onDone){
  let loaded=0;
  list.forEach(src=>{
    const img=new Image();
    img.onload = img.onerror = ()=>{
      loaded++;
      if(onProgress) onProgress(loaded, list.length, src);
      if(loaded===list.length && onDone) onDone();
    };
    img.src=src;
  });
}
status.style.display='block';
status.textContent='載入資源中…';
preloadAll(preloadList, (l,t)=>{ status.textContent = `載入中 ${l}/${t}`; }, ()=>{
  status.style.display='none';
  introHint.textContent = '點擊卡片閱讀說明 →';
});

/* =========== Intro 流程 (點 introImg 依序兩張) =========== */
introImg.addEventListener('click', ()=>{
  if(introStage === 0){
    // 顯示第二張說明
    introImg.src = "https://i.postimg.cc/RFT9fQ61/zip-3.jpg";
    introStage = 1;
    introHint.textContent = '再點一次繼續 →';
  } else if(introStage === 1){
    // 完成 intro，進入遊戲介面
    intro.style.display = 'none';
    game.style.display = 'block';
    startControls.style.display = 'flex';
    introStage = 2;
  }
});

/* =========== Start 按鈕顯示抽卡按鈕 =========== */
startBtn.addEventListener('click', ()=>{
  startControls.style.display = 'none';
  drawControlsDiv.style.display = 'flex';
  // 顯示一張預設卡（可自訂）
  showCardSample();
});

/* 顯示預設卡 */
function showCardSample(){
  // 顯示一張隨機一星作示範（或可設為說明圖）
  const src = oneStar[Math.floor(Math.random()*oneStar.length)];
  cardImg.src = src;
  showCardVisual();
}

/* 顯示卡片的視覺（淡入） */
function showCardVisual(){
  card.classList.remove('flip-in','flip-out');
  card.style.display='inline-block';
  requestAnimationFrame(()=>{ card.classList.add('show'); });
}

/* 隱藏卡片（淡出） */
function hideCardVisual(){
  card.classList.remove('show');
  card.style.display='none';
}

/* =========== 抽卡邏輯（按鈕） =========== */
function drawFromPool(pool){
  // 切換動畫：flip out -> 換圖 -> flip in
  card.classList.remove('flip-in');
  card.classList.add('flip-out');

  setTimeout(()=> {
    // 更換圖片
    const idx = Math.floor(Math.random()*pool.length);
    const newSrc = pool[idx];
    cardImg.src = newSrc;
    // 進入 flip-in
    card.classList.remove('flip-out');
    card.classList.add('flip-in');
    // ensure visible
    card.style.display='inline-block';
  }, 320);
}

// 綁定按鈕（用 data-rarity）
document.querySelectorAll('.btn').forEach(btn=>{
  btn.addEventListener('click', (e)=>{
    e.stopPropagation();
    const rarity = btn.getAttribute('data-rarity');
    if(rarity === '1') drawFromPool(oneStar);
    else if(rarity === '2') drawFromPool(twoStar);
    else drawFromPool(threeStar);
  });
});

/* =========== 點擊卡片放大預覽 =========== */
card.addEventListener('click', (e)=>{
  e.stopPropagation();
  if(!cardImg.src) return;
  // show preview
  previewImg.src = cardImg.src;
  preview.classList.add('show');
  preview.style.display='flex';
  inPreview = true;
});

/* close preview */
previewClose.addEventListener('click', (e)=>{
  e.stopPropagation();
  preview.classList.remove('show');
  preview.style.display='none';
  inPreview = false;
});

/* 任何空白處點擊，如果在 preview 就關閉 */
document.addEventListener('click', (e)=>{
  if(inPreview){
    preview.classList.remove('show');
    preview.style.display='none';
    inPreview=false;
  }
});

/* 防止按鈕點擊觸發 document click */
previewImg.addEventListener('click', (e)=>e.stopPropagation());
cardImg.addEventListener('click', (e)=>e.stopPropagation());

/* allow keyboard ESC close preview */
document.addEventListener('keydown', (e)=>{
  if(e.key === 'Escape' && inPreview){
    preview.classList.remove('show');
    preview.style.display='none';
    inPreview=false;
  }
});
</script>
</body>
</html>