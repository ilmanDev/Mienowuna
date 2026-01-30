
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MUNA BARAKATI</title>

<style>
:root{
  --blue:#0a66ff;
  --cyan:#00e0ff;
  --dark:#020c1b;
  --card:#071a33;
}
*{box-sizing:border-box;font-family:system-ui,-apple-system}
body{
  margin:0;min-height:100vh;
  background:
    radial-gradient(circle at top,#0a66ff33,transparent 40%),
    linear-gradient(135deg,#020c1b,#041a33);
  display:flex;justify-content:center;align-items:center;
  color:#eaf2ff;
}

/* SPLASH */
#splash{
  position:fixed;inset:0;
  background:linear-gradient(135deg,#020c1b,#0a66ff);
  display:flex;flex-direction:column;
  justify-content:center;align-items:center;
  z-index:999;
}
.loader{
  width:46px;height:46px;
  border:3px solid rgba(255,255,255,.15);
  border-top:3px solid var(--cyan);
  border-radius:50%;
  margin-top:20px;
  animation:spin 1s linear infinite;
}
@keyframes spin{to{transform:rotate(360deg);}}

/* CARD */
.card{
  background:linear-gradient(180deg,#071a33,#04142b);
  width:100%;max-width:420px;
  padding:26px;border-radius:30px;
  box-shadow:0 40px 90px rgba(0,50,150,.5);
  display:none;
}

/* LOGO */
.logo{
  text-align:center;
  font-size:28px;
  font-weight:900;
  letter-spacing:1px;
  background:linear-gradient(90deg,var(--blue),var(--cyan));
  -webkit-background-clip:text;
  color:transparent;
}
.subtitle{
  text-align:center;
  color:#7fa1c7;
  margin-bottom:14px;
}

/* FACE */
.face-wrap{position:relative}
video{
  width:100%;
  border-radius:22px;
  background:#000;
}
.scan-line{
  position:absolute;
  left:0;right:0;
  height:3px;
  background:linear-gradient(90deg,transparent,var(--cyan),transparent);
  animation:scan 2s linear infinite;
}
@keyframes scan{
  0%{top:0}
  100%{top:100%}
}
.face-frame{
  position:absolute;
  inset:12px;
  border:2px solid rgba(0,224,255,.6);
  border-radius:18px;
}
.scan-text{
  text-align:center;
  margin:10px 0;
  font-weight:700;
  color:#9bbcff;
}
.btn{
  width:100%;
  padding:15px;
  border:none;
  border-radius:18px;
  background:linear-gradient(135deg,var(--blue),var(--cyan));
  color:#fff;
  font-size:16px;
  font-weight:800;
  cursor:pointer;
  box-shadow:0 15px 40px rgba(0,224,255,.5);
}

/* MENU */
.menu{display:none}
.menu h3{text-align:center;margin-bottom:18px}
.grid{
  display:grid;
  grid-template-columns:repeat(2,1fr);
  gap:18px;
}
.item{
  background:linear-gradient(180deg,#0a254f,#071a33);
  border-radius:22px;
  padding:22px 10px;
  text-align:center;
  text-decoration:none;
  color:#eaf2ff;
  font-weight:800;
  box-shadow:0 20px 45px rgba(0,100,255,.35);
  transition:.35s;
}
.item:hover{
  transform:translateY(-8px) scale(1.05);
  box-shadow:0 35px 70px rgba(0,150,255,.6);
}
.icon svg{
  width:34px;height:34px;
  fill:var(--cyan);
  margin-bottom:8px;
}
.logout{
  margin-top:22px;
  background:linear-gradient(135deg,#ff4d4d,#ff7a7a);
  padding:15px;
  border-radius:18px;
  text-align:center;
  font-weight:800;
  cursor:pointer;
}
</style>
</head>

<body>

<!-- SPLASH -->
<div id="splash">
  <h1>MUNA BARAKATI</h1>
  <p>Advanced Face Security</p>
  <div class="loader"></div>
</div>

<div class="card" id="app">

<!-- LOGIN -->
<div id="loginBox">
  <div class="logo">MUNA BARAKATI</div>
  <div class="subtitle">Face ID Scan</div>

  <div class="face-wrap">
    <video id="video" autoplay muted></video>
    <div class="scan-line"></div>
    <div class="face-frame"></div>
  </div>

  <div class="scan-text" id="scanText">
    Posisikan wajah di dalam frame
  </div>
  <button class="btn" onclick="startScan()">SCAN WAJAH</button>
</div>

<!-- MENU -->
<div id="menuBox" class="menu">
  <h3>Menu Utama</h3>

  <div class="grid">
    <a href="https://droid10.my.id/" target="_blank" class="item">
      <div class="icon">
        <svg viewBox="0 0 24 24"><path d="M3 9l1-5h16l1 5H3z"/></svg>
      </div>LPP & VARIANCE
    </a>

    <a href="https://ilmandev.github.io/portoilman/" target="_blank" class="item">
      <div class="icon">
        <svg viewBox="0 0 24 24"><path d="M10 15l5-3-5-3v6z"/></svg>
      </div>Portofolio
    </a>

    <a href="https://wa.me/6287872921421" target="_blank" class="item">
      <div class="icon">
        <svg viewBox="0 0 24 24"><path d="M12 2a10 10 0 0 0-8.9 14.5"/></svg>
      </div>WhatsApp
    </a>

    <a href="https://maps.google.com" target="_blank" class="item">
      <div class="icon">
        <svg viewBox="0 0 24 24"><path d="M12 2c-4 0-7 3-7 7"/></svg>
      </div>Maps
    </a>
  </div>

  <div class="logout" onclick="logout()">Logout</div>
</div>

</div>

<script>
let stream;

setTimeout(()=>{
  splash.style.display="none";
  app.style.display="block";
  if(localStorage.getItem("login")==="true"){
    loginBox.style.display="none";
    menuBox.style.display="block";
  }
},1600);

async function startCamera(){
  stream = await navigator.mediaDevices.getUserMedia({video:true});
  video.srcObject = stream;
}

async function startScan(){
  await startCamera();
  scanText.innerText = "Scanning biometrik...";
  setTimeout(()=>{
    scanText.innerText = "Wajah terverifikasi ✔";
    localStorage.setItem("login","true");
    stream.getTracks().forEach(t=>t.stop());
    setTimeout(()=>{
      loginBox.style.display="none";
      menuBox.style.display="block";
    },700);
  },3000);
}

function logout(){
  localStorage.removeItem("login");
  menuBox.style.display="none";
  loginBox.style.display="block";
}
</script>

</body>
</html>
