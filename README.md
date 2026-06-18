<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🐍 Pastel Snake Deluxe</title>

<style>
body{
    margin:0;
    min-height:100vh;
    display:flex;
    flex-direction:column;
    justify-content:center;
    align-items:center;
    background:#FFF7F0;
    font-family:'Segoe UI',sans-serif;
    padding:20px;
    box-sizing:border-box;
}

h1{
    color:#8B8BAE;
    margin:10px 0;
}

.score-box{
    display:flex;
    gap:20px;
    margin-bottom:10px;
    color:#7D7D9C;
    font-size:18px;
    font-weight:bold;
}

canvas{
    background:#FFFDF8;
    border:6px solid #D8C4F1;
    border-radius:20px;
    box-shadow:0 8px 20px rgba(0,0,0,.1);
}

.controls{
    margin-top:20px;
    display:flex;
    flex-direction:column;
    align-items:center;
    gap:10px;
}

.middle{
    display:flex;
    gap:10px;
}

.controls button{
    width:70px;
    height:70px;
    border:none;
    border-radius:20px;
    background:#E6D6FF;
    color:#6D5A8C;
    font-size:28px;
    cursor:pointer;
    box-shadow:0 4px 10px rgba(0,0,0,.1);
    touch-action:manipulation;
}

.controls button:active{
    transform:scale(.95);
    background:#D8C4F1;
}

.info{
    margin-top:15px;
    color:#8B8BAE;
    text-align:center;
}
</style>
</head>
<body>

<h1>🐍 Pastel Snake Deluxe</h1>

<div class="score-box">
    <div>คะแนน: <span id="score">0</span></div>
    <div>🏆 สูงสุด: <span id="highScore">0</span></div>
</div>

<canvas id="game" width="400" height="400"></canvas>

<div class="controls">
    <button onclick="setDirection('UP')">⬆</button>

    <div class="middle">
        <button onclick="setDirection('LEFT')">⬅</button>
        <button onclick="setDirection('DOWN')">⬇</button>
        <button onclick="setDirection('RIGHT')">➡</button>
    </div>
</div>

<div class="info">
⌨️ คีย์บอร์ด • 📱 ปุ่มสัมผัส • 👆 ปัดหน้าจอ
</div>

<audio id="eatSound">
<source src="https://actions.google.com/sounds/v1/cartoon/pop.ogg" type="audio/ogg">
</audio>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

const box = 20;
const rows = canvas.width / box;

let snake = [{x:200,y:200}];
let direction = "RIGHT";

let food = randomFood();

let score = 0;
let speed = 120;

let highScore = localStorage.getItem("highScore") || 0;
document.getElementById("highScore").textContent = highScore;

document.addEventListener("keydown", changeDirection);

function changeDirection(e){
    if(e.key==="ArrowUp") setDirection("UP");
    if(e.key==="ArrowDown") setDirection("DOWN");
    if(e.key==="ArrowLeft") setDirection("LEFT");
    if(e.key==="ArrowRight") setDirection("RIGHT");
}

function setDirection(newDir){
    if(newDir==="UP" && direction!=="DOWN") direction="UP";
    if(newDir==="DOWN" && direction!=="UP") direction="DOWN";
    if(newDir==="LEFT" && direction!=="RIGHT") direction="LEFT";
    if(newDir==="RIGHT" && direction!=="LEFT") direction="RIGHT";
}

function randomFood(){
    return {
        x:Math.floor(Math.random()*rows)*box,
        y:Math.floor(Math.random()*rows)*box
    };
}

function collision(head,body){
    for(let i=0;i<body.length;i++){
        if(head.x===body[i].x && head.y===body[i].y){
            return true;
        }
    }
    return false;
}

function draw(){

    ctx.fillStyle="#FFFDF8";
    ctx.fillRect(0,0,canvas.width,canvas.height);

    // อาหาร
    ctx.fillStyle="#FFB6C1";
    ctx.beginPath();
    ctx.arc(food.x+10,food.y+10,8,0,Math.PI*2);
    ctx.fill();

    // งู
    snake.forEach((segment,index)=>{
        ctx.fillStyle=index===0 ? "#A8D8EA" : "#C7F0DB";

        ctx.beginPath();
        ctx.roundRect(segment.x,segment.y,20,20,6);
        ctx.fill();
    });

    let headX = snake[0].x;
    let headY = snake[0].y;

    if(direction==="UP") headY-=box;
    if(direction==="DOWN") headY+=box;
    if(direction==="LEFT") headX-=box;
    if(direction==="RIGHT") headX+=box;

    if(headX===food.x && headY===food.y){

        score++;
        document.getElementById("score").textContent = score;

        try{
            document.getElementById("eatSound").play();
        }catch(e){}

        food=randomFood();

        if(score % 5 === 0 && speed > 50){
            clearInterval(game);
            speed -= 10;
            game = startGame();
        }

    }else{
        snake.pop();
    }

    const newHead = {x:headX,y:headY};

    if(
        headX < 0 ||
        headY < 0 ||
        headX >= canvas.width ||
        headY >= canvas.height ||
        collision(newHead,snake)
    ){

        clearInterval(game);

        if(score > highScore){
            localStorage.setItem("highScore",score);
        }

        setTimeout(()=>{
            alert(
                "🐍 เกมจบ!\n\nคะแนน: " +
                score +
                "\n🏆 สูงสุด: " +
                Math.max(score,highScore)
            );

            location.reload();
        },100);

        return;
    }

    snake.unshift(newHead);
}

function startGame(){
    return setInterval(draw,speed);
}

let game = startGame();


// Swipe มือถือ
let touchStartX = 0;
let touchStartY = 0;

canvas.addEventListener("touchstart",e=>{
    touchStartX = e.touches[0].clientX;
    touchStartY = e.touches[0].clientY;
});

canvas.addEventListener("touchend",e=>{

    let touchEndX = e.changedTouches[0].clientX;
    let touchEndY = e.changedTouches[0].clientY;

    let dx = touchEndX - touchStartX;
    let dy = touchEndY - touchStartY;

    if(Math.abs(dx) > Math.abs(dy)){
        if(dx > 0) setDirection("RIGHT");
        else setDirection("LEFT");
    }else{
        if(dy > 0) setDirection("DOWN");
        else setDirection("UP");
    }

});
</script>

</body>
</html>
