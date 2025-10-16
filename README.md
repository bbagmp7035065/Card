# Card
<h1 style="text-align:center;">炫酷抽卡遊戲</h1>

<div style="text-align:center;margin-bottom:20px;">
  <button class="drawBtn" data-rarity="1" style="padding:12px 25px;margin:0 5px;">抽一星</button>
  <button class="drawBtn" data-rarity="2" style="padding:12px 25px;margin:0 5px;">抽二星</button>
  <button class="drawBtn" data-rarity="3" style="padding:12px 25px;margin:0 5px;">抽三星</button>
</div>

<div id="card" style="margin-top:40px;text-align:center;opacity:0;transition: all 0.6s ease;">
  <img id="cardImg" src="" alt="" style="width:200px;border-radius:10px;box-shadow:0 4px 10px rgba(0,0,0,0.2);">
  <div id="cardName" style="font-size:24px;margin-top:10px;"></div>
</div>

<script>
const cards = [
  {name:"一星", rarity:1, imgs:[
    "https://i.postimg.cc/PqsNtJzP/zip-4.jpg",
    "https://i.postimg.cc/1zh49XcF/zip-5.jpg",
    "https://i.postimg.cc/vmFDQTL9/zip-6.jpg"
  ]},
  {name:"二星", rarity:2, imgs:[
    "https://i.postimg.cc/bYH5gBBr/zip-24.jpg",
    "https://i.postimg.cc/HsYhh9qj/zip-25.jpg",
    "https://i.postimg.cc/3J8ccFsJ/zip-26.jpg"
  ]},
  {name:"三星", rarity:3, imgs:[
    "https://i.postimg.cc/SRDDhHB9/zip-49.jpg",
    "https://i.postimg.cc/1XJJQbkV/zip-50.jpg",
    "https://i.postimg.cc/4yBBgjD7/zip-51.jpg"
  ]}
];

const cardDiv = document.getElementById("card");
const cardImg = document.getElementById("cardImg");
const cardName = document.getElementById("cardName");

document.querySelectorAll(".drawBtn").forEach(btn=>{
  btn.addEventListener("click", ()=>{
    const rarity = parseInt(btn.dataset.rarity);
    const card = cards.find(c => c.rarity === rarity);

    // 隱藏卡牌做動畫
    cardDiv.style.opacity = 0;
    cardDiv.style.transform = "scale(0.5) translateY(-50px)";

    setTimeout(()=>{
      const randomImg = card.imgs[Math.floor(Math.random()*card.imgs.length)];
      cardImg.src = randomImg;
      cardName.textContent = `${card.name} - ${"⭐".repeat(card.rarity)}`;

      cardDiv.style.opacity = 1;
      cardDiv.style.transform = "scale(1) translateY(0)";
    }, 200);
  });
});
</script>