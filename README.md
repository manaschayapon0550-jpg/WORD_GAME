<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Smart Bag Dashboard</title>

<style>
body{
    font-family: Arial, sans-serif;
    background:#f4f6f9;
    margin:0;
    padding:20px;
}

.container{
    max-width:900px;
    margin:auto;
}

h1{
    text-align:center;
    color:#333;
}

.card{
    background:white;
    border-radius:15px;
    padding:20px;
    margin-bottom:15px;
    box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

.status{
    display:flex;
    justify-content:space-between;
    flex-wrap:wrap;
}

.item{
    margin:10px;
}

button{
    padding:12px 20px;
    border:none;
    border-radius:10px;
    cursor:pointer;
    color:white;
    font-size:16px;
}

.find{
    background:#2196F3;
}

.sos{
    background:#f44336;
}

.connected{
    color:green;
    font-weight:bold;
}

.map{
    width:100%;
    height:300px;
    background:#ddd;
    border-radius:10px;
    display:flex;
    align-items:center;
    justify-content:center;
    color:#555;
}
</style>
</head>

<body>

<div class="container">

<h1>🎒 Smart Bag Dashboard</h1>

<div class="card">
    <h2>สถานะอุปกรณ์</h2>

    <div class="status">
        <div class="item">
            <p>🔋 แบตเตอรี่</p>
            <h3 id="battery">87%</h3>
        </div>

        <div class="item">
            <p>📡 การเชื่อมต่อ</p>
            <h3 class="connected">ออนไลน์</h3>
        </div>

        <div class="item">
            <p>🌤️ สภาพอากาศ</p>
            <h3 id="weather">32°C</h3>
        </div>
    </div>
</div>

<div class="card">
    <h2>📍 ตำแหน่งกระเป๋า</h2>

    <div class="map">
        แผนที่จะแสดงตรงนี้
    </div>

    <p id="location">
        Latitude: 13.7563 <br>
        Longitude: 100.5018
    </p>
</div>

<div class="card">
    <h2>🎒 ควบคุมกระเป๋า</h2>

    <button class="find" onclick="findBag()">
        🔍 ค้นหากระเป๋า
    </button>

    <button class="sos" onclick="sendSOS()">
        🚨 SOS
    </button>
</div>

</div>

<script>

function findBag(){
    alert("กำลังส่งเสียงเตือนที่กระเป๋า...");
}

function sendSOS(){
    alert("ส่งพิกัดฉุกเฉินเรียบร้อย!");
}

// จำลองข้อมูลอัปเดต
setInterval(() => {

    let battery = Math.floor(Math.random() * 20) + 80;

    document.getElementById("battery").innerText =
        battery + "%";

}, 5000);

</script>

</body>
</html>
