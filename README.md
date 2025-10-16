<div id="game">
  <img id="background" src="https://i.postimg.cc/BbMSWcJZ/zip-1.jpg" alt="背景圖" />

  <div id="card" class="hidden">
    <img id="cardImg" src="" alt="卡片" />
  </div>

  <button id="backButton" style="display:none;">返回</button>

  <div id="buttons" class="hidden">
    <button class="draw-btn" data-rarity="1">抽一星卡</button>
    <button class="draw-btn" data-rarity="2">抽二星卡</button>
    <button class="draw-btn" data-rarity="3">抽三星卡</button>
  </div>
</div>

body {
  margin: 0;
  padding: 0;
  background: black;
  overflow: hidden;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

#game {
  position: relative;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

#background {
  width: 100%;
  height: 100%;
  object-fit: cover;
  cursor: pointer;
}

#card {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: none;
  z-index: 10;
  transition: transform 0.4s ease, opacity 0.4s ease;
}

#card img {
  width: 80vw;
  max-width: 450px;
  border-radius: 12px;
  box-shadow: 0 0 25px rgba(0, 0, 0, 0.7);
}

#card.fullscreen {
  width: 100vw;
  height: 100vh;
  max-width: none;
  max-height: none;
  top: 0;
  left: 0;
  transform: none;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: rgba(0,0,0,0.9);
  z-index: 100;
}

#card.fullscreen img {
  width: 95vw;
  height: auto;
  max-height: 95vh;
  object-fit: contain;
  border-radius: 10px;
  box-shadow: 0 0 20px rgba(255,255,255,0.6);
}

#backButton {
  position: absolute;
  bottom: 20px;
  right: 20px;
  z-index: 200;
  padding: 12px 20px;
  font-size: 18px;
  border: none;
  border-radius: 8px;
  background: rgba(255,255,255,0.85);
  color: #333;
  cursor: pointer;
}

#buttons {
  position: absolute;
  bottom: 40px;
  width: 100%;
  text-align: center;
  z-index: 20;
}

.draw-btn {
  margin: 0 10px;
  padding: 12px 24px;
  font-size: 18px;
  border: none;
  border-radius: 8px;
  background: rgba(255,255,255,0.85);
  cursor: pointer;
  transition: transform 0.2s ease;
}

.draw-btn:hover {
  transform: scale(1.1);
}

.hidden {
  display: none;
}

/* ---------- 資料與預載 ---------- */
// 說明卡（依序顯示）
const introImgs = [
  "https://i.postimg.cc/MTYxRbMV/zip-2.jpg",
  "https://i.postimg.cc/RFT9fQ61/zip-3.jpg"
];

// 卡牌資料（完整70張：一星/二星/三星）
const cards = [
  {name:"一星", rarity:1, imgs:[
    "https://i.postimg.cc/jj0Cs2Qy/zip-10.jpg","https://i.postimg.cc/mrfhbkNy/zip-11.jpg","https://i.postimg.cc/HLDjpn44/zip-12.jpg",
    "https://i.postimg.cc/9f5zW0tb/zip-13.jpg","https://i.postimg.cc/rpXz8sCQ/zip-14.jpg","https://i.postimg.cc/jj0Cs2QM/zip-15.jpg",
    "https://i.postimg.cc/LsKhH5By/zip-16.jpg","https://i.postimg.cc/QMGVXCky/zip-17.jpg","https://i.postimg.cc/pdby2rY1/zip-18.jpg",
    "https://i.postimg.cc/vmrTDy37/zip-19.jpg","https://i.postimg.cc/W4GztVX6/zip-20.jpg","https://i.postimg.cc/25Qy3Y27/zip-21.jpg",
    "https://i.postimg.cc/hGLvhnpr/zip-22.jpg","https://i.postimg.cc/Dzr0mhB5/zip-23.jpg","https://i.postimg.cc/PqsNtJzP/zip-4.jpg",
    "https://i.postimg.cc/1zh49XcF/zip-5.jpg","https://i.postimg.cc/vmFDQTL9/zip-6.jpg","https://i.postimg.cc/bvcdzrH2/zip-7.jpg",
    "https://i.postimg.cc/hGWhSvbd/zip-8.jpg","https://i.postimg.cc/d0PDq3mR/zip-9.jpg"
  ]},
  {name:"二星", rarity:2, imgs:[
    "https://i.postimg.cc/bYH5gBBr/zip-24.jpg","https://i.postimg.cc/HsYhh9qj/zip-25.jpg","https://i.postimg.cc/3J8ccFsJ/zip-26.jpg",
    "https://i.postimg.cc/HsYhh9qk/zip-27.jpg","https://i.postimg.cc/WbNHHwB3/zip-28.jpg","https://i.postimg.cc/PrXRRQgW/zip-29.jpg",
    "https://i.postimg.cc/DyvMMP9s/zip-30.jpg","https://i.postimg.cc/5N4RRShC/zip-31.jpg","https://i.postimg.cc/SNSvvfBD/zip-32.jpg",
    "https://i.postimg.cc/c4xbbBpT/zip-33.jpg","https://i.postimg.cc/CLhQQs3j/zip-34.jpg","https://i.postimg.cc/TYt4MWxr/zip-35.jpg",
    "https://i.postimg.cc/RVSDDLkg/zip-36.jpg","https://i.postimg.cc/Qx8yyJ26/zip-37.jpg","https://i.postimg.cc/xT64DbQy/zip-38.jpg",
    "https://i.postimg.cc/VLKVQbw9/zip-39.jpg","https://i.postimg.cc/ZKwMtym8/zip-40.jpg","https://i.postimg.cc/L6xbKZSW/zip-41.jpg",
    "https://i.postimg.cc/qM15VCr5/zip-42.jpg","https://i.postimg.cc/gkgTF6pt/zip-43.jpg","https://i.postimg.cc/rFfPXtTB/zip-44.jpg",
    "https://i.postimg.cc/xT64DbQW/zip-45.jpg","https://i.postimg.cc/L6xbKZSG/zip-46.jpg","https://i.postimg.cc/dt5xPTvF/zip-47.jpg",
    "https://i.postimg.cc/5NsRdF1M/zip-48.jpg"
  ]},
  {name:"三星", rarity:3, imgs:[
    "https://i.postimg.cc/SRDDhHB9/zip-49.jpg","https://i.postimg.cc/1XJJQbkV/zip-50.jpg","https://i.postimg.cc/4yBBgjD7/zip-51.jpg",
    "https://i.postimg.cc/WzXXvxBd/zip-52.jpg","https://i.postimg.cc/VvDD82xL/zip-53.jpg","https://i.postimg.cc/mk88sKvb/zip-54.jpg",
    "https://i.postimg.cc/cCXXSPpx/zip-55.jpg","https://i.postimg.cc/D0BBTR9n/zip-56.jpg","https://i.postimg.cc/2yz2gCK3/zip-57.jpg",
    "https://i.postimg.cc/XqVQtnzJ/zip-58.jpg","https://i.postimg.cc/nrH0NnPL/zip-59.jpg","https://i.postimg.cc/zvJ04qcG/zip-60.jpg",
    "https://i.postimg.cc/QCX0vjyN/zip-61.jpg","https://i.postimg.cc/6q9YFBmW/zip-62.jpg","https://i.postimg.cc/sxVwb3Ns/zip-63.jpg",
    "https://i.postimg.cc/zvJ04qcq/zip-64.jpg","https://i.postimg.cc/NFgpZQzq/zip-65.jpg","https://i.postimg.cc/7hxmjqWv/zip-66.jpg",
    "https://i.postimg.cc/6q9YFB1k/zip-67.jpg","https://i.postimg.cc/T1RCBTHZ/zip-68.jpg","https://i.postimg.cc/vTQ3CGSP/zip-69.jpg",
    "https://i.postimg.cc/Fz9Dwhnn/zip-70.jpg","https://i.postimg.cc/Wz2XxsfK/zip-71.jpg","https://i.postimg.cc/vTQ3CGSK/zip-72.jpg",
    "https://i.postimg.cc/3RjtLk9V/zip-73.jpg"
  ]}
];

// 預載所有圖片（說明卡 + 所有卡）
function preloadImages(list, onProgress, onComplete) {
  let loaded = 0;
  const total = list.length;
  list.forEach(src => {
    const img = new Image();
    img.onload = img.onerror = () => {
      loaded++;
      if (onProgress) onProgress(loaded, total, src);
      if (loaded === total && onComplete) onComplete();
    };
    img.src = src;
  });
}

// 建立要預載的完整列表
const preloadList = [...introImgs];
cards.forEach(c => c.imgs.forEach(u => preloadList.push(u)));

preloadImages(preloadList,
  (loaded, total, src) => {
    // 你可以在這裡顯示載入數字（若要做 Loading UI）
    // console.log(`loaded ${loaded}/${total}`, src);
  },
  () => {
    console.log("所有圖片已預載完成（包含70張卡與說明卡）");
  }
);

/* ---------- 元素並初始化狀態 ---------- */
const bg = document.getElementById("background");
const cardEl = document.getElementById("card");
const cardImg = document.getElementById("cardImg");
const backButton = document.getElementById("backButton");
const buttonsWrap = document.getElementById("buttons");

let introStep = -1;   // -1 = 還沒開始，0 = 第一張說明已顯示...
let inFullscreen = false;
let lastShownRarity = null;

/* ---------- 說明卡流程：點背景開始，之後點卡片切換 ---------- */
function showCardWithSrc(src, showImmediately = true) {
  cardImg.src = src;
  // 顯示卡片（淡入）
  cardEl.style.display = "inline-block";
  requestAnimationFrame(() => {
    cardEl.style.opacity = 1;
    cardEl.style.transform = "scale(1) translateY(0)";
  });
}

function hideCard() {
  cardEl.style.opacity = 0;
  cardEl.style.transform = "scale(0.8) translateY(-20px)";
  // 等動畫結束再隱藏（避免瞬間閃爍）
  setTimeout(() => { if(!inFullscreen) cardEl.style.display = "none"; }, 350);
}

bg.addEventListener("click", () => {
  if (introStep === -1) {
    // 第一次點背景，顯示第一張說明卡
    introStep = 0;
    showCardWithSrc(introImgs[introStep]);
    // 隱藏背景點擊（避免重複）
    // 背景仍可點，但行為由 introStep 控制
  }
});

// 點卡切換說明或在遊戲中放大
cardEl.addEventListener("click", (e) => {
  e.stopPropagation();
  // 如果目前是介紹流程中
  if (introStep >= 0 && introStep < introImgs.length - 1) {
    introStep++;
    showCardWithSrc(introImgs[introStep]);
    return;
  }
  if (introStep >= 0 && introStep === introImgs.length - 1) {
    // 最後一張說明，點卡進入遊戲（顯示按鈕）
    hideCard();
    setTimeout(() => {
      buttonsWrap.classList.remove("hidden");
      introStep = -2; // 表示已完成 intro 流程
    }, 350);
    return;
  }

  // 若已是遊戲流程，點卡則進入/維持放大模式
  if (!inFullscreen && cardImg.src) {
    // 進入放大
    cardEl.classList.add("fullscreen");
    backButton.style.display = "block";
    inFullscreen = true;
  }
});

/* ---------- 返回按鈕：退出放大 ---------- */
backButton.addEventListener("click", (e) => {
  e.stopPropagation();
  if (inFullscreen) {
    cardEl.classList.remove("fullscreen");
    backButton.style.display = "none";
    inFullscreen = false;
  }
});

/* ---------- 抽卡邏輯（按鈕） ---------- */
function showRandomFromRarity(rarity) {
  const group = cards.find(c => c.rarity === rarity);
  if (!group) return;
  // 明顯一點的切換動畫：先淡出縮小 -> 換圖 -> 放大淡入
  cardEl.style.opacity = 0;
  cardEl.style.transform = "scale(0.6) translateY(-40px)";
  // 隱藏 fullscreen 狀態（若之前在放大）
  if (inFullscreen) {
    cardEl.classList.remove("fullscreen");
    backButton.style.display = "none";
    inFullscreen = false;
  }

  setTimeout(() => {
    const idx = Math.floor(Math.random() * group.imgs.length);
    const imgSrc = group.imgs[idx];
    cardImg.src = imgSrc;
    cardNameText = `${group.name} - ${"⭐".repeat(group.rarity)}`;
    // 顯示名稱（如果 HTML 有顯示位置）
    // 如果你的 HTML 裡沒有 cardName 元素可以忽略這行
    const nameEl = document.getElementById("cardName");
    if (nameEl) nameEl.textContent = cardNameText;

    // 顯示卡片
    cardEl.style.display = "inline-block";
    requestAnimationFrame(() => {
      cardEl.style.opacity = 1;
      cardEl.style.transform = "scale(1) translateY(0)";
    });

    lastShownRarity = rarity;
  }, 260);
}

// 綁定三個抽卡按鈕（使用事件 delegation or selection）
document.querySelectorAll(".draw-btn").forEach(btn => {
  btn.addEventListener("click", (e) => {
    e.stopPropagation();
    const rarity = parseInt(btn.dataset.rarity, 10);
    // 若尚未完成 intro 流程，先把按鈕顯示保護
    if (introStep !== -2 && introStep !== -1) {
      // 尚未完成 intro，忽略或先完成 intro
      return;
    }
    // 若按下抽卡，確保按鈕容器可見
    buttonsWrap.classList.remove("hidden");
    showRandomFromRarity(rarity);
  });
});

/* ---------- 初始狀態（進入頁面） ---------- */
// 進入頁面：按背景開始（或直接可點背景）
buttonsWrap.classList.add("hidden");
cardEl.style.display = "none";
backButton.style.display = "none";

/* ---------- 防呆：如果點擊背景但流程已完成，則無動作 ---------- */
document.addEventListener("click", (e) => {
  // 如果點在背景但 intro 已完成且不是 fullscreen，不做任何事
});