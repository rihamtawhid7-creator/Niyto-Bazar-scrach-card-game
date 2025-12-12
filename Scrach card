<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Niyto Bazar – Premium 3D Scratch Game</title>
<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: linear-gradient(135deg, #f8b500, #fceabb);
    padding: 20px;
}

h1 { color:#ff6600; font-size:36px; margin-bottom:10px; }

.box {
    width: 320px;
    margin: 20px auto;
    background: white;
    border-radius: 20px;
    padding: 20px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.2);
    perspective: 1000px;
}

button {
    padding: 12px 25px;
    font-size: 18px;
    font-weight: bold;
    background: #ff6600;
    color: white;
    border: none;
    border-radius: 12px;
    cursor: pointer;
    margin: 10px;
    transition:0.3s;
}
button:hover { background: #e65c00; }

#winnerLog {
    width: 90%;
    max-width: 600px;
    margin: 20px auto;
    background: #fff3cd;
    border: 2px solid #ffeeba;
    border-radius: 15px;
    padding: 15px;
    text-align: left;
}

#winnerLog h3 { color: #856404; margin-bottom:10px; }
#winnerLog table { width:100%; border-collapse: collapse; }
#winnerLog th, #winnerLog td { border:1px solid #ffd; padding:8px; text-align:center; }
#winnerLog th { background:#ffeeba; color:#856404; }

#confettiCanvas {
    position: fixed; top:0; left:0;
    width:100%; height:100%;
    pointer-events:none;
}

#cardArea {
    perspective: 1000px;
    margin-top: 20px;
}

.card3D {
    width: 300px;
    height: 200px;
    margin: 0 auto;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 1s;
    cursor: pointer;
    border-radius: 15px;
    box-shadow: 0 15px 35px rgba(0,0,0,0.3);
}

.cardFront, .cardBack {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 15px;
    backface-visibility: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    font-weight: bold;
}

.cardFront { background: #C0C0C0; }
.cardBack { background: #fff; color: #28a745; transform: rotateY(180deg); }

#takeBtn, #shareBtn { display:none; margin-top:15px; padding:12px 25px; font-size:18px; border:none; border-radius:12px; cursor:pointer; }
#takeBtn { background:#28a745; color:white; }
#shareBtn { background:#007bff; color:white; }

#qrCode { margin-top:15px; }
</style>
</head>
<body>

<h1>Niyto Bazar</h1>

<!-- Mini Game -->
<div id="gameBox" class="box">
    <p>Click the button to collect a coin!</p>
    <button onclick="playGame()">Play Game</button>
    <p id="coinMsg" style="font-size:20px;font-weight:bold;color:#28a745;"></p>
</div>

<!-- Scratch Card -->
<div id="scratchArea" class="box" style="display:none;">
    <h3>🪙 You Collected 1 Coin -- Scratch Now!</h3>
    <div id="cardArea">
        <div id="card" class="card3D">
            <div class="cardFront">Scratch Here</div>
            <div class="cardBack"></div>
        </div>
    </div>
    <button id="takeBtn">Take Gift</button>
    <button id="shareBtn">Share Gift</button>
    <div id="qrCode"></div>
</div>

<!-- Winner Log -->
<div id="winnerLog">
    <h3>🏆 Winner Log</h3>
    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>Code</th>
                <th>Prize</th>
            </tr>
        </thead>
        <tbody id="winnerTable"></tbody>
    </table>
</div>

<!-- Admin Button -->
<div>
    <button onclick="showAdminPanel()">Admin Panel</button>
</div>

<!-- Confetti Canvas -->
<canvas id="confettiCanvas"></canvas>

<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script>
// Mini Game
let coin=0;
function playGame(){
    coin=1; 
    document.getElementById("coinMsg").innerHTML="✔ You collected 1 coin!";
    setTimeout(()=>{
        document.getElementById("gameBox").style.display="none";
        document.getElementById("scratchArea").style.display="block";
        setupCard();
    },800);
}

// Scratch Card + 3D
const gifts=["5% Discount","10% Discount","15% Discount","20% Discount","25% Discount","30% Discount","🎁 Free T-Shirt!"];
let winnerCount=0;
let winnersList=[];

function setupCard(){
    const card = document.getElementById("card");
    const cardBack = card.querySelector(".cardBack");

    // Generate prize and code
    const selectedGift = gifts[Math.floor(Math.random()*gifts.length)];
    const randomCode = Math.random().toString(36).substring(2,7);
    cardBack.innerHTML = `${selectedGift}<br>Code: ${randomCode}`;

    // Card flip on click
    card.addEventListener("click", function handler(){
        card.style.transform = "rotateY(180deg) scale(1.05)";
        document.getElementById("takeBtn").style.display="inline-block";
        document.getElementById("shareBtn").style.display="inline-block";
        launchConfetti();

        // Update winner log
        winnerCount++;
        winnersList.push({code: randomCode, prize: selectedGift});
        const tableBody=document.getElementById("winnerTable");
        const row=document.createElement("tr");
        row.innerHTML=`<td>${winnerCount}</td><td>${randomCode}</td><td>${selectedGift}</td>`;
        tableBody.appendChild(row);

        // Remove listener to prevent multiple flips
        card.removeEventListener("click", handler);
    });

    // QR Code
    new QRCode(document.getElementById("qrCode"), { text: window.location.href, width:128, height:128 });

    // Take & Share
    document.getElementById("takeBtn").onclick = ()=>alert(`✔ Your gift is saved! 🎁\nYour code: ${randomCode}`);
    document.getElementById("shareBtn").onclick = ()=>{
        const text=`I just won "${selectedGift}" at Niyto Bazar! My code: ${randomCode}\nPlay here: ${window.location.href}`;
        window.open(`https://api.whatsapp.com/send?text=${encodeURIComponent(text)}`,'_blank');
    }
}

// Confetti Effect
const confettiCanvas=document.getElementById("confettiCanvas");
const confCtx=confettiCanvas.getContext("2d");
confettiCanvas.width=window.innerWidth;
confettiCanvas.height=window.innerHeight;
let confettis=[];
function launchConfetti(){
    for(let i=0;i<100;i++){
        confettis.push({
            x: Math.random()*window.innerWidth,
            y: Math.random()*window.innerHeight,
            r: Math.random()*6+4,
            dx: (Math.random()-0.5)*4,
            dy: Math.random()*-5,
            color: `hsl(${Math.random()*360},100%,50%)`
        });
    }
}
function drawConfetti(){
    confCtx.clearRect(0,0,window.innerWidth,window.innerHeight);
    confettis.forEach((c,i)=>{
        confCtx.fillStyle=c.color;
        confCtx.beginPath();
        confCtx.arc(c.x,c.y,c.r,0,Math.PI*2);
        confCtx.fill();
        c.x+=c.dx;
        c.y+=c.dy+0.5;
        c.dy+=0.05;
        if(c.y>window.innerHeight) confettis.splice(i,1);
    });
    requestAnimationFrame(drawConfetti);
}
drawConfetti();

// Admin Panel
function showAdminPanel(){
    const pwd = prompt("Enter Admin Password:");
    if(pwd==="niyto123"){
        let log="";
        winnersList.forEach((w,i)=>{ log+=`${i+1}. Code: ${w.code}, Prize: ${w.prize}\n`; });
        if(!log) log="No winners yet!";
        alert("🏆 Winners List:\n\n"+log);

        // Download CSV option
        if(winnersList.length>0 && confirm("Download winners as CSV?")) downloadCSV();
    } else alert("❌ Wrong password!");
}

// CSV download
function downloadCSV(){
    let csv="data:text/csv;charset=utf-8,No,Code,Prize\n";
    winnersList.forEach((w,i)=>{ csv+=`${i+1},${w.code},${w.prize}\n`; });
    const encodedUri=encodeURI(csv);
    const link=document.createElement("a");
    link.setAttribute("href", encodedUri);
    const date=new Date();
    link.setAttribute("download", `niyto_winners_${date.getFullYear()}-${date.getMonth()+1}-${date.getDate()}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);
}
</script>

</body>
</html>
