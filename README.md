<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta
name="viewport"
content="width=device-width, initial-scale=1.0">

<title>BÍ MẬT QUỐC GIA</title>

<script src="https://unpkg.com/@supabase/supabase-js@2"></script>

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Segoe UI,sans-serif;
}

body{
background:#050816;
color:white;
overflow:hidden;
}

/* BACKGROUND */

body::before{

content:"";

position:fixed;
inset:0;

background:
linear-gradient(rgba(0,255,255,.05) 1px, transparent 1px),
linear-gradient(90deg, rgba(0,255,255,.05) 1px, transparent 1px);

background-size:40px 40px;

animation:gridMove 8s linear infinite;

z-index:-3;

}

@keyframes gridMove{

0%{
transform:translateY(0);
}

100%{
transform:translateY(40px);
}

}

body::after{

content:"";

position:fixed;

width:700px;
height:700px;

border-radius:50%;

background:
radial-gradient(circle,
rgba(0,255,255,.2),
transparent 70%);

filter:blur(100px);

animation:glowMove 8s infinite alternate;

z-index:-2;

}

@keyframes glowMove{

0%{
transform:translate(0,0);
}

100%{
transform:translate(500px,300px);
}

}

/* PARTICLES */

.particle{

position:fixed;

width:3px;
height:3px;

background:cyan;

border-radius:50%;

box-shadow:0 0 10px cyan;

animation:float linear infinite;

z-index:-1;

}

@keyframes float{

from{
transform:translateY(100vh);
}

to{
transform:translateY(-120vh);
}

}

/* STATUS */

#status{

position:fixed;

top:20px;
right:20px;

padding:15px 25px;

background:
rgba(0,0,0,.7);

border:
1px solid cyan;

border-radius:15px;

display:none;

z-index:9999;

}

/* LOGIN */

#loginScreen{

position:fixed;
inset:0;

display:flex;
justify-content:center;
align-items:center;

background:
rgba(0,0,0,.85);

backdrop-filter:blur(20px);

z-index:9999;

}

.loginBox{

width:400px;

padding:40px;

border-radius:30px;

background:
rgba(255,255,255,.05);

border:
1px solid rgba(0,255,255,.2);

backdrop-filter:blur(20px);

box-shadow:
0 0 40px rgba(0,255,255,.15);

}

.loginBox h1{

text-align:center;

font-size:40px;

margin-bottom:25px;

text-shadow:
0 0 20px cyan;

}

.loginBox input{

width:100%;

padding:18px;

margin-top:15px;

border:none;

border-radius:15px;

background:
rgba(255,255,255,.08);

color:white;

font-size:16px;

outline:none;

}

/* BUTTON */

button{

padding:15px 22px;

border:none;

border-radius:15px;

background:
linear-gradient(
45deg,
cyan,
#00ffd5
);

color:black;

font-weight:bold;

cursor:pointer;

transition:.3s;

box-shadow:
0 0 15px cyan;

}

button:hover{

transform:
translateY(-2px)
scale(1.03);

}

/* SIDEBAR */

.sidebar{

position:fixed;

left:0;
top:0;

width:250px;
height:100vh;

padding:25px;

background:
rgba(255,255,255,.05);

border-right:
1px solid rgba(255,255,255,.08);

backdrop-filter:blur(20px);

overflow:auto;

}

.logo{

text-align:center;

font-size:32px;
font-weight:bold;

margin-bottom:20px;

text-shadow:
0 0 20px cyan;

}

#userEmail{

padding:15px;

border-radius:15px;

background:
rgba(255,255,255,.05);

margin-bottom:20px;

text-align:center;

word-break:break-all;

}

.menuBtn{

width:100%;

margin-top:15px;

}

/* CONTENT */

.content{

margin-left:270px;

padding:40px;

height:100vh;

overflow:auto;

}

.section{

display:none;

animation:fade .4s;

}

@keyframes fade{

from{
opacity:0;
transform:translateY(20px);
}

to{
opacity:1;
transform:translateY(0);
}

}

.section h1{

font-size:42px;

margin-bottom:25px;

text-shadow:
0 0 15px cyan;

}

/* TEXTAREA */

textarea{

width:100%;
height:350px;

padding:25px;

border:none;

border-radius:25px;

background:
rgba(255,255,255,.08);

color:white;

font-size:18px;

outline:none;

}

/* GALLERY */

.gallery{

display:grid;

grid-template-columns:
repeat(auto-fill,minmax(300px,1fr));

gap:25px;

margin-top:30px;

}

.card{

padding:15px;

border-radius:25px;

background:
rgba(255,255,255,.05);

border:
1px solid rgba(255,255,255,.08);

backdrop-filter:blur(20px);

}

.card img,
.card video{

width:100%;

border-radius:20px;

}

.actions{

display:flex;
justify-content:center;
gap:10px;

margin-top:15px;

}

/* MOBILE */

@media(max-width:768px){

.sidebar{

width:100%;
height:auto;
position:relative;

}

.content{

margin-left:0;
padding:20px;

}

.gallery{

grid-template-columns:1fr;

}

.loginBox{

width:90%;

}

}

</style>

</head>

<body>

<div id="status"></div>

<!-- LOGIN -->

<div id="loginScreen">

<div class="loginBox">

<h1>LOGIN</h1>

<input
type="email"
id="email"
placeholder="Email">

<input
type="password"
id="password"
placeholder="Password">

<br><br>

<button onclick="register()">

REGISTER

</button>

<br><br>

<button onclick="login()">

LOGIN

</button>

</div>

</div>

<!-- SIDEBAR -->

<div class="sidebar">

<div class="logo">

BÍ MẬT QUỐC GIA

</div>

<div id="userEmail">

NOT LOGIN

</div>

<button
class="menuBtn"
onclick="openSection('note')">

📖 GHI CHÚ

</button>

<button
class="menuBtn"
onclick="openSection('photo')">

🖼️ ẢNH

</button>

<button
class="menuBtn"
onclick="openSection('video')">

🎥 VIDEO

</button>

<button
class="menuBtn"
onclick="logout()">

🚪 LOGOUT

</button>

</div>

<!-- CONTENT -->

<div class="content">

<!-- NOTE -->

<div
id="note"
class="section">

<h1>📖 GHI CHÚ</h1>

<textarea id="noteText"></textarea>

<br><br>

<button onclick="saveNote()">

LƯU ONLINE

</button>

</div>

<!-- PHOTO -->

<div
id="photo"
class="section">

<h1>🖼️ ẢNH</h1>

<input
type="file"
id="photoInput"
accept="image/*">

<div
class="gallery"
id="photoGallery"></div>

</div>

<!-- VIDEO -->

<div
id="video"
class="section">

<h1>🎥 VIDEO</h1>

<input
type="file"
id="videoInput"
accept="video/*">

<div
class="gallery"
id="videoGallery"></div>

</div>

</div>

<script>

/* PARTICLES */

for(let i=0;i<40;i++){

const p =
document.createElement("div");

p.className="particle";

p.style.left=
Math.random()*100+"vw";

p.style.top=
Math.random()*100+"vh";

p.style.opacity=
Math.random();

p.style.animationDuration=
5+Math.random()*10+"s";

document.body.appendChild(p);

}

/* SUPABASE */

const supabaseClient =
window.supabase.createClient(

"https://nbzdkwuxttrexvolzhmy.supabase.co",

"sb_publishable_PSp_w_99W4x_c74hgf3Suw_MdT8MU0M"

);

/* STATUS */

function showStatus(text){

const status =
document.getElementById("status");

status.style.display="block";

status.innerText=text;

setTimeout(()=>{

status.style.display="none";

},3000);

}

/* OPEN */

function openSection(id){

document.querySelectorAll(".section")
.forEach(sec=>{

sec.style.display="none";

});

document.getElementById(id)
.style.display="block";

}

/* REGISTER */

async function register(){

const email =
document.getElementById("email").value.trim();

const password =
document.getElementById("password").value.trim();

if(password.length < 6){

showStatus("Mật khẩu tối thiểu 6 ký tự");

return;

}

showStatus("Đang đăng ký...");

const { error } =
await supabaseClient.auth.signUp({

email,
password

});

if(error){

showStatus(error.message);

}else{

showStatus("Đăng ký thành công");

}

}

/* LOGIN */

async function login(){

const email =
document.getElementById("email").value.trim();

const password =
document.getElementById("password").value.trim();

showStatus("Đang đăng nhập...");

const { data,error } =
await supabaseClient.auth.signInWithPassword({

email,
password

});

if(error){

showStatus(error.message);

}else{

document.getElementById(
"loginScreen"
).style.display="none";

document.getElementById(
"userEmail"
).innerText =
data.user.email;

showStatus("Đăng nhập thành công");

openSection("note");

loadNote();
loadPhotos();
loadVideos();

}

}

/* SESSION */

async function checkSession(){

const {
data:{session}
} =
await supabaseClient.auth.getSession();

if(session){

document.getElementById(
"loginScreen"
).style.display="none";

document.getElementById(
"userEmail"
).innerText =
session.user.email;

openSection("note");

loadNote();
loadPhotos();
loadVideos();

}

}

checkSession();

/* LOGOUT */

async function logout(){

await supabaseClient.auth.signOut();

showStatus("Đã đăng xuất");

setTimeout(()=>{

location.reload();

},1200);

}

/* SAVE NOTE */

async function saveNote(){

const text =
document.getElementById(
"noteText"
).value;

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const { error } =
await supabaseClient
.from("notes")
.upsert({

user_id:user.id,
content:text

},{
onConflict:"user_id"
});

if(error){

showStatus(error.message);

}else{

showStatus("Đã lưu ghi chú");

}

}

/* LOAD NOTE */

async function loadNote(){

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const { data } =
await supabaseClient
.from("notes")
.select("*")
.eq("user_id",user.id)
.maybeSingle();

if(data){

document.getElementById(
"noteText"
).value =
data.content || "";

}

}

/* PHOTO */

document.getElementById(
"photoInput"
).addEventListener(
"change",
async(e)=>{

const file =
e.target.files[0];

if(!file) return;

showStatus("Đang upload ảnh...");

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const fileName =
user.id + "/" +
Date.now() + "_" +
file.name;

const { error } =
await supabaseClient
.storage
.from("photos")
.upload(fileName,file);

if(error){

showStatus(error.message);

}else{

showStatus("Upload ảnh thành công");

loadPhotos();

}

});

/* LOAD PHOTO */

async function loadPhotos(){

const gallery =
document.getElementById(
"photoGallery"
);

gallery.innerHTML =
"<h2>Đang tải ảnh...</h2>";

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const { data,error } =
await supabaseClient
.storage
.from("photos")
.list(user.id);

if(error){

gallery.innerHTML =
"<h2>Lỗi tải ảnh</h2>";

return;

}

gallery.innerHTML="";

if(data.length===0){

gallery.innerHTML =
"<h2>Chưa có ảnh</h2>";

return;

}

for(const item of data){

const path =
user.id + "/" + item.name;

const { data:urlData } =
supabaseClient
.storage
.from("photos")
.getPublicUrl(path);

const url =
urlData.publicUrl;

const card =
document.createElement("div");

card.className="card";

card.innerHTML=`

<img src="${url}">

<div class="actions">

<a href="${url}" target="_blank">

<button>TẢI</button>

</a>

<button onclick="deletePhoto('${item.name}')">

XÓA

</button>

</div>

`;

gallery.appendChild(card);

}

}

/* DELETE PHOTO */

async function deletePhoto(name){

const {
data:{user}
} =
await supabaseClient.auth.getUser();

await supabaseClient
.storage
.from("photos")
.remove([

user.id + "/" + name

]);

showStatus("Đã xóa ảnh");

loadPhotos();

}

/* VIDEO */

document.getElementById(
"videoInput"
).addEventListener(
"change",
async(e)=>{

const file =
e.target.files[0];

if(!file) return;

showStatus("Đang upload video...");

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const fileName =
user.id + "/" +
Date.now() + "_" +
file.name;

const { error } =
await supabaseClient
.storage
.from("videos")
.upload(fileName,file);

if(error){

showStatus(error.message);

}else{

showStatus("Upload video thành công");

loadVideos();

}

});

/* LOAD VIDEO */

async function loadVideos(){

const gallery =
document.getElementById(
"videoGallery"
);

gallery.innerHTML =
"<h2>Đang tải video...</h2>";

const {
data:{user}
} =
await supabaseClient.auth.getUser();

const { data,error } =
await supabaseClient
.storage
.from("videos")
.list(user.id);

if(error){

gallery.innerHTML =
"<h2>Lỗi tải video</h2>";

return;

}

gallery.innerHTML="";

if(data.length===0){

gallery.innerHTML =
"<h2>Chưa có video</h2>";

return;

}

for(const item of data){

const path =
user.id + "/" + item.name;

const { data:urlData } =
supabaseClient
.storage
.from("videos")
.getPublicUrl(path);

const url =
urlData.publicUrl;

const card =
document.createElement("div");

card.className="card";

card.innerHTML=`

<video controls>

<source src="${url}">

</video>

<div class="actions">

<a href="${url}" target="_blank">

<button>TẢI</button>

</a>

<button onclick="deleteVideo('${item.name}')">

XÓA

</button>

</div>

`;

gallery.appendChild(card);

}

}

/* DELETE VIDEO */

async function deleteVideo(name){

const {
data:{user}
} =
await supabaseClient.auth.getUser();

await supabaseClient
.storage
.from("videos")
.remove([

user.id + "/" + name

]);

showStatus("Đã xóa video");

loadVideos();

}

</script>

</body>
</html>

