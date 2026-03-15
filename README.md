<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>AI Analyst Market Terminal</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
margin:0;
font-family:monospace;
background:black;
color:#e6edf3;
overflow-x:hidden;
}

#matrix{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
z-index:-1;
}

header{
display:flex;
justify-content:space-between;
align-items:center;
background:#0d1117;
padding:15px;
border-bottom:1px solid #30363d;
}

nav{
display:flex;
gap:10px;
flex-wrap:wrap;
}

nav button{
background:#111;
color:white;
border:1px solid #30363d;
padding:6px 10px;
cursor:pointer;
}

#newsTicker{
background:#111;
padding:8px;
border-bottom:1px solid #30363d;
white-space:nowrap;
overflow:hidden;
}

.container{
display:grid;
grid-template-columns:2fr 1fr 1fr;
gap:20px;
padding:20px;
}

.panel{
background:#0d1117;
padding:20px;
border-radius:10px;
border:1px solid #30363d;
}

.section{display:none;}
.active{display:block;}

table{
width:100%;
border-collapse:collapse;
}

th,td{
padding:8px;
border-bottom:1px solid #30363d;
text-align:center;
}

.good{color:#00ff9c;}
.bad{color:#ff4d4d;}

.heatmap{
display:grid;
grid-template-columns:repeat(3,1fr);
gap:8px;
}

.heat{
padding:20px;
text-align:center;
font-weight:bold;
border-radius:6px;
}

.terminal{
background:black;
color:#00ff9c;
height:200px;
overflow:auto;
padding:10px;
}

#marketMap{
display:grid;
grid-template-columns:repeat(8,1fr);
gap:8px;
}

.marketStock{
padding:12px;
text-align:center;
font-size:12px;
font-weight:bold;
border-radius:6px;
color:white;
}

#signals{
height:120px;
overflow:auto;
color:#00ff9c;
}

#volatilityBar{
height:20px;
width:10%;
background:green;
transition:0.5s;
}

</style>
</head>

<body>

<canvas id="matrix"></canvas>

<header>

<div>Analyst Terminal</div>

<nav>

<button onclick="show('home')">Home</button>
<button onclick="show('portfolio')">Portfolio</button>
<button onclick="show('stocks')">Stocks</button>
<button onclick="show('news')">News</button>
<button onclick="show('ai')">AI</button>
<button onclick="show('settings')">Settings</button>
<button onclick="logout()">Logout</button>

</nav>

<div id="clock"></div>

</header>

<div id="newsTicker"></div>

<div id="home" class="section active container">

<div class="panel">
<h2>User Info</h2>
<p>Name: Analyst</p>
<p>Email: analyst@gmail.com</p>
</div>

<div class="panel">
<h2>Market Chart</h2>
<canvas id="chart"></canvas>
</div>

<div class="panel">
<h2>AI Prediction</h2>
<div id="prediction">Analyzing market...</div>
</div>

</div>

<script>

/* CLOCK */

setInterval(()=>{
document.getElementById("clock").innerHTML=new Date().toLocaleTimeString()
},1000)

/* NAVIGATION */

function show(id){
document.querySelectorAll(".section").forEach(s=>s.classList.remove("active"))
document.getElementById(id).classList.add("active")
}

function logout(){
alert("Logged out successfully")
}

/* RANDOM PRICE */

function random(){
return Math.random()*200
}

/* CHART */

window.onload=function(){

const ctx=document.getElementById("chart")

const chart=new Chart(ctx,{
type:"line",
data:{labels:[],datasets:[{data:[],borderColor:"#00e5ff",borderWidth:2,tension:0.4}]},
options:{plugins:{legend:{display:false}},animation:false}
})

let t=0

setInterval(()=>{

t++

chart.data.labels.push(t)
chart.data.datasets[0].data.push(random())

if(chart.data.labels.length>30){
chart.data.labels.shift()
chart.data.datasets[0].data.shift()
}

chart.update()

},800)

}

/* NEWS */

const news=[
"Tech stocks rally amid AI boom",
"Federal Reserve hints rate pause",
"Nvidia earnings exceed expectations",
"Electric vehicle stocks surge globally"
]

function updateNews(){

let ticker=""

news.forEach(n=>{
ticker+=n+" | "
})

document.getElementById("newsTicker").innerHTML=ticker

}

updateNews()

/* MATRIX BACKGROUND */

const canvas=document.getElementById("matrix")
const ctx=canvas.getContext("2d")

canvas.height=window.innerHeight
canvas.width=window.innerWidth

const letters="01".split("")
const fontSize=14
const columns=canvas.width/fontSize

const drops=[]

for(let x=0;x<columns;x++){
drops[x]=1
}

function drawMatrix(){

ctx.fillStyle="rgba(0,0,0,0.05)"
ctx.fillRect(0,0,canvas.width,canvas.height)

ctx.fillStyle="#00ff9c"
ctx.font=fontSize+"px monospace"

for(let i=0;i<drops.length;i++){

const text=letters[Math.floor(Math.random()*letters.length)]

ctx.fillText(text,i*fontSize,drops[i]*fontSize)

if(drops[i]*fontSize>canvas.height && Math.random()>0.975){
drops[i]=0
}

drops[i]++

}

}

setInterval(drawMatrix,33)

</script>

</body>
</html>
