
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<meta name="theme-color" content="#102b46">
<title>GoDrive Car Rental</title>

<style>
*{box-sizing:border-box}
body{margin:0;background:#f3f6fb;color:#172b40;font-family:Arial,sans-serif}
header{background:#102b46;color:white;padding:20px 5%;display:flex;justify-content:space-between;align-items:center;flex-wrap:wrap;gap:12px}
.logo{font-size:27px;font-weight:bold}.logo span{color:#65b6ff}
.container{max-width:1200px;margin:auto;padding:20px}
.hero{background:linear-gradient(120deg,#102b46,#24649b);color:white;padding:30px;border-radius:18px;margin-bottom:24px}
.hero h1{font-size:clamp(28px,5vw,42px);margin:0 0 12px}
button{background:#1769e0;color:white;border:0;padding:12px 16px;border-radius:9px;font-weight:bold;cursor:pointer;margin:3px}
button:disabled{opacity:.5}
.light{background:white;color:#102b46}
.secondary{background:#e8f0fc;color:#164e92}
.panel,.car-card{background:white;border-radius:15px;padding:18px;margin:15px 0;box-shadow:0 4px 18px #102b4610}
.car-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:18px}
.car-card{padding:0;overflow:hidden;margin:0}
.car-image{width:100%;height:205px;object-fit:cover;background:#e1e8f0}
.car-body{padding:16px}
.car-body h3{margin:6px 0}
.info{font-size:13px;color:#62758b;line-height:1.9}
.desc{font-size:14px;line-height:1.6;min-height:44px}
.price{font-weight:bold;color:#145ac5;font-size:19px}
.price small{font-size:12px;color:#6b7c90}
.pill{display:inline-block;background:#e3f1ff;color:#15518a;padding:6px 10px;border-radius:20px;font-size:12px}
.car-bottom{border-top:1px solid #e8edf4;padding-top:12px;display:flex;justify-content:space-between;align-items:center;gap:6px}
input{width:100%;padding:12px;border:1px solid #ccd6e2;border-radius:8px;margin:7px 0 14px;font-size:15px}
label{font-size:14px;font-weight:bold}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
.scan-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px}
.scan-card{padding:20px;border-radius:14px;background:#eaf3ff}
.upload-card{background:#e9faf2}
.preview{width:100%;max-height:360px;object-fit:contain;background:#e8edf4;border-radius:12px;margin-top:12px}
video{width:100%;max-height:360px;background:#101820;border-radius:12px}
.booking{border:1px solid #e0e7f0;border-radius:10px;padding:14px;margin:12px 0}
.muted{font-size:14px;color:#68798b;line-height:1.6}
#notice{padding:10px 0;overflow-wrap:anywhere}
footer{text-align:center;background:#102b46;color:white;padding:22px}
[hidden]{display:none!important}
@media(max-width:850px){.car-grid{grid-template-columns:repeat(2,minmax(0,1fr))}}
@media(max-width:540px){
 .container{padding:13px}.car-grid,.scan-grid,.form-grid{grid-template-columns:1fr}
 .car-image{height:220px}.hero{padding:22px}.car-bottom{flex-wrap:wrap}
}
</style>
</head>

<body>
<header>
  <div>
    <div class="logo">🚘 Go<span>Drive</span></div>
    <div>Your journey, your way.</div>
  </div>
  <div>
    <button class="light" id="loginTop">Login / Register</button>
    <button id="logout" hidden>Sign Out</button>
  </div>
</header>

<main class="container">
  <section class="hero">
    <h1>Find Your Perfect Ride</h1>
    <p>Explore our cars, scan or upload a car photo, choose your dates and book easily.</p>
    <button class="light" id="explore">Explore Cars ↓</button>
  </section>

  <!-- LOGIN -->
  <section class="panel" id="authPanel" hidden>
    <h2 id="authTitle">Login to GoDrive</h2>
    <form id="authForm">
      <label for="email">Email</label>
      <input id="email" type="email" required placeholder="Email address">
      <label for="password">Password</label>
      <input id="password" type="password" minlength="6" required placeholder="At least 6 characters">
      <button id="authSubmit">Sign In</button>
      <button type="button" class="secondary" id="toggleRegister">Create Account</button>
    </form>
  </section>

  <!-- CAR SHOWROOM -->
  <section id="carsSection">
    <h2>Our Cars</h2>
    <p class="muted">Explore our collection and select your preferred vehicle.</p>
    <div class="car-grid" id="carGrid"></div>
  </section>

  <!-- SCAN OR UPLOAD -->
  <section class="panel" id="scanSection">
    <h2>Scan or Upload a Car</h2>
    <p class="muted">Take a photo with your camera or choose an image from your gallery.</p>

    <div class="scan-grid">
      <div class="scan-card">
        <h3>📷 Scan Your Car</h3>
        <p class="muted">Use your phone camera to capture a car photo.</p>
        <button id="startCamera">Open Camera</button>
        <div id="cameraArea" hidden>
          <video id="camera" autoplay playsinline></video>
          <button id="capture">Capture Photo</button>
          <button id="stopCamera" class="secondary">Close Camera</button>
        </div>
      </div>

      <div class="scan-card upload-card">
        <h3>🖼️ Upload from Gallery</h3>
        <p class="muted">Choose an existing car photograph from your phone.</p>
        <button id="chooseImage">Choose Photo</button>
        <input id="galleryInput" type="file" accept="image/*" hidden>
      </div>
    </div>

    <section id="previewPanel" hidden>
      <h3>Photo Preview</h3>
      <img id="photoPreview" class="preview" alt="Selected car photo">
      <p id="photoName" class="muted"></p>
      <button id="clearPhoto" class="secondary">Remove Photo</button>
      <p class="muted">The photo is previewed on this page. It is not automatically saved to Firebase or used to identify the car.</p>
    </section>

    <section id="detectedPanel" class="panel" hidden>
      <h3>Car Details</h3>
      <p class="muted">Enter the details manually. Automatic model recognition requires an additional image-recognition service.</p>
      <label for="scanModel">Car model</label>
      <input id="scanModel" placeholder="e.g. Hyundai Creta">
      <label for="scanColor">Car colour</label>
      <input id="scanColor" placeholder="e.g. White">
      <label for="scanYear">Model year</label>
      <input id="scanYear" type="number" min="1950" max="2099" placeholder="e.g. 2025">
      <button id="useDetails">Use These Details</button>
    </section>
  </section>

  <!-- BOOKING -->
  <section class="panel" id="bookingPanel" hidden>
    <h2>Book Your Car</h2>
    <div id="chosenCar"></div>
    <form id="bookingForm">
      <div class="form-grid">
        <div><label for="pickup">Pickup date</label><input id="pickup" type="date" required></div>
        <div><label for="returnDate">Return date</label><input id="returnDate" type="date" required></div>
      </div>
      <p id="estimate" class="price"></p>
      <p class="muted">This is a booking request, not a confirmed reservation.</p>
      <button id="confirmBtn">Send Booking Request</button>
      <button type="button" class="secondary" id="cancelBooking">Cancel</button>
    </form>
  </section>

  <!-- MY BOOKINGS -->
  <section class="panel" id="myBookings" hidden>
    <h2>My Bookings</h2>
    <button id="refreshBookings" class="secondary">Refresh</button>
    <div id="bookingList"></div>
  </section>

  <section class="panel" id="accountPanel" hidden>
    <p id="welcome"></p>
    <button id="showBookings">My Bookings</button>
  </section>

  <p id="notice" role="status" aria-live="polite"></p>
</main>

<footer>© 2026 GoDrive Car Rental · Drive safely, travel happily.</footer>

<script type="module">
import {initializeApp}
 from "https://www.gstatic.com/firebasejs/12.4.0/firebase-app.js";
import {
 getAuth,createUserWithEmailAndPassword,
 signInWithEmailAndPassword,onAuthStateChanged,signOut
} from "https://www.gstatic.com/firebasejs/12.4.0/firebase-auth.js";
import {
 getFirestore,collection,addDoc,getDocs,query,where,serverTimestamp
} from "https://www.gstatic.com/firebasejs/12.4.0/firebase-firestore.js";

// Replace with your actual Firebase web configuration.
const firebaseConfig = {
 apiKey:"PASTE_YOUR_API_KEY",
 authDomain:"PASTE_YOUR_AUTH_DOMAIN",
 projectId:"PASTE_YOUR_PROJECT_ID",
 storageBucket:"PASTE_YOUR_STORAGE_BUCKET",
 messagingSenderId:"PASTE_YOUR_MESSAGING_SENDER_ID",
 appId:"PASTE_YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

const cars = [
 {id:"swift",name:"Maruti Suzuki Swift",model:"Swift VXi",color:"White",fuel:"Petrol",trans:"Manual",seats:5,price:1500,
 img:"https://images.unsplash.com/photo-1503376780353-7e6692767b70?auto=format&fit=crop&w=900&q=80",
 desc:"Compact hatchback for city trips and weekend journeys."},
 {id:"city",name:"Honda City",model:"City V CVT",color:"Metallic Grey",fuel:"Petrol",trans:"Automatic",seats:5,price:2200,
 img:"https://images.unsplash.com/photo-1492144534655-ae79c964c9d7?auto=format&fit=crop&w=900&q=80",
 desc:"Comfortable sedan for business travel and family trips."},
 {id:"creta",name:"Hyundai Creta",model:"Creta SX",color:"White",fuel:"Petrol",trans:"Automatic",seats:5,price:2800,
 img:"https://images.unsplash.com/photo-1519641471654-76ce0107ad1b?auto=format&fit=crop&w=900&q=80",
 desc:"Versatile SUV with a spacious cabin and luggage room."},
 {id:"nexon",name:"Tata Nexon",model:"Nexon Creative",color:"Grey",fuel:"Petrol",trans:"Automatic",seats:5,price:2300,
 img:"https://images.unsplash.com/photo-1549317661-bd32c8ce0db2?auto=format&fit=crop&w=900&q=80",
 desc:"Practical compact SUV for everyday drives."},
 {id:"innova",name:"Toyota Innova Crysta",model:"Innova GX",color:"White",fuel:"Diesel",trans:"Manual",seats:7,price:3500,
 img:"https://images.unsplash.com/photo-1511919884226-fd3cad34687c?auto=format&fit=crop&w=900&q=80",
 desc:"Spacious people-mover for family and group travel."},
 {id:"thar",name:"Mahindra Thar",model:"Thar LX",color:"Red",fuel:"Petrol",trans:"Automatic",seats:4,price:4000,
 img:"https://images.unsplash.com/photo-1533473359331-0135ef1b58bf?auto=format&fit=crop&w=900&q=80",
 desc:"Lifestyle SUV for memorable road trips."}
];

let user=null,selectedCar=null,isRegister=false,stream=null,photoUrl=null;
const $=id=>document.getElementById(id);
const money=n=>"₹"+Number(n).toLocaleString("en-IN");
const today=()=>{
 const d=new Date();
 return d.getFullYear()+"-"+String(d.getMonth()+1).padStart(2,"0")+"-"+String(d.getDate()).padStart(2,"0");
};
$("pickup").min=today();
$("returnDate").min=today();

function say(t){$("notice").textContent=t;}

function renderCars(){
 $("carGrid").innerHTML="";
 cars.forEach(car=>{
  const card=document.createElement("article");
  card.className="car-card";

  const img=document.createElement("img");
  img.className="car-image";
  img.src=car.img;img.alt=car.name+" car photo";img.loading="lazy";
  img.onerror=()=>{img.alt="Car photo unavailable";img.style.display="none";};

  const body=document.createElement("div");
  body.className="car-body";
  const badge=document.createElement("span");
  badge.className="pill";badge.textContent="AVAILABLE MODEL";
  const title=document.createElement("h3");title.textContent=car.name;
  const info=document.createElement("div");info.className="info";
  info.textContent=`Variant: ${car.model} | Colour: ${car.color} | Fuel: ${car.fuel} | ${car.trans} | ${car.seats} seats`;
  const desc=document.createElement("p");desc.className="desc";desc.textContent=car.desc;
  const bottom=document.createElement("div");bottom.className="car-bottom";
  const price=document.createElement("div");price.className="price";price.textContent=money(car.price);
  const small=document.createElement("small");small.textContent=" / day";price.append(small);
  const btn=document.createElement("button");btn.textContent="Book Now";
  btn.addEventListener("click",()=>chooseCar(car.id));
  bottom.append(price,btn);body.append(badge,title,info,desc,bottom);
  card.append(img,body);$("carGrid").append(card);
 });
}

function chooseCar(id){
 selectedCar=cars.find(c=>c.id===id);
 if(!user){$("authPanel").hidden=false;$("authPanel").scrollIntoView({behavior:"smooth"});say("Sign in to book a car.");return;}
 $("chosenCar").textContent=`${selectedCar.name} — ${selectedCar.model}, ${selectedCar.color} — ${money(selectedCar.price)}/day`;
 $("bookingPanel").hidden=false;$("bookingPanel").scrollIntoView({behavior:"smooth"});
 updateEstimate();
}

function updateEstimate(){
 const a=$("pickup").value,b=$("returnDate").value;
 if(!selectedCar||!a||!b||b<=a){$("estimate").textContent="Choose valid pickup and return dates.";return;}
 const days=(Date.parse(b+"T00:00:00")-Date.parse(a+"T00:00:00"))/86400000;
 $("estimate").textContent=`${days} day(s) · Estimated total ${money(days*selectedCar.price)}`;
}
$("pickup").addEventListener("change",()=>{
 $("returnDate").min=$("pickup").value||today();
 if($("returnDate").value&&$("returnDate").value<=$("pickup").value)$("returnDate").value="";
 updateEstimate();
});
$("returnDate").addEventListener("change",updateEstimate);

$("explore").onclick=()=>$("carsSection").scrollIntoView({behavior:"smooth"});
$("loginTop").onclick=()=>{$("authPanel").hidden=!$("authPanel").hidden;$("authPanel").scrollIntoView({behavior:"smooth"});};

$("toggleRegister").onclick=()=>{
 isRegister=!isRegister;
 $("authTitle").textContent=isRegister?"Create Account":"Login to GoDrive";
 $("authSubmit").textContent=isRegister?"Register":"Sign In";
 $("toggleRegister").textContent=isRegister?"Already registered? Sign In":"Create Account";
};

$("authForm").addEventListener("submit",async e=>{
 e.preventDefault();$("authSubmit").disabled=true;
 try{
  const email=$("email").value.trim(),password=$("password").value;
  if(isRegister)await createUserWithEmailAndPassword(auth,email,password);
  else await signInWithEmailAndPassword(auth,email,password);
  say("Authentication successful.");
 }catch(err){say("Login error: "+err.message);}
 finally{$("authSubmit").disabled=false;}
});
$("logout").onclick=async()=>{try{await signOut(auth);say("Signed out.");}catch(e){say(e.message);}};

$("bookingForm").addEventListener("submit",async e=>{
 e.preventDefault();
 if(!user||!selectedCar)return;
 const pickup=$("pickup").value,ret=$("returnDate").value;
 const days=(Date.parse(ret+"T00:00:00")-Date.parse(pickup+"T00:00:00"))/86400000;
 if(pickup<today()||days<1){say("Choose valid dates. Return must be after pickup.");return;}
 $("confirmBtn").disabled=true;
 try{
  await addDoc(collection(db,"bookings"),{
   uid:user.uid,carId:selectedCar.id,carName:selectedCar.name,
   dailyRate:selectedCar.price,pickupDate:pickup,returnDate:ret,
   days,totalPrice:days*selectedCar.price,status:"requested",
   createdAt:serverTimestamp()
  });
  say("Booking request saved.");$("bookingPanel").hidden=true;selectedCar=null;
  await loadBookings();
 }catch(err){say("Booking error: "+err.message);}
 finally{$("confirmBtn").disabled=false;}
});
$("cancelBooking").onclick=()=>{$("bookingPanel").hidden=true;selectedCar=null;};

async function loadBookings(){
 if(!user){say("Please sign in first.");return;}
 $("myBookings").hidden=false;$("bookingList").textContent="Loading...";
 try{
  const result=await getDocs(query(collection(db,"bookings"),where("uid","==",user.uid)));
  $("bookingList").innerHTML="";
  if(result.empty){$("bookingList").textContent="No bookings yet.";return;}
  result.forEach(doc=>{
   const b=doc.data(),card=document.createElement("article");card.className="booking";
   const title=document.createElement("h3");title.textContent=b.carName;
   const dates=document.createElement("p");dates.textContent=`Pickup: ${b.pickupDate} · Return: ${b.returnDate}`;
   const total=document.createElement("p");total.textContent="Estimated total: "+money(b.totalPrice);
   const status=document.createElement("p");status.textContent="Status: "+b.status;
   card.append(title,dates,total,status);$("bookingList").append(card);
  });
  $("myBookings").scrollIntoView({behavior:"smooth"});
 }catch(err){$("bookingList").textContent="Unable to load: "+err.message;}
}
$("showBookings").onclick=loadBookings;
$("refreshBookings").onclick=loadBookings;

// CAMERA: works on HTTPS pages such as GitHub Pages.
$("startCamera").onclick=async()=>{
 try{
  if(!navigator.mediaDevices?.getUserMedia){
   say("Camera unavailable. Open the HTTPS website in Chrome or use Gallery Upload.");return;
  }
  stream=await navigator.mediaDevices.getUserMedia({video:{facingMode:"environment"},audio:false});
  $("camera").srcObject=stream;
  $("cameraArea").hidden=false;
  say("Camera ready. Allow camera access if prompted.");
 }catch(err){say("Cannot open camera: "+err.message+". You can upload a photo from Gallery instead.");}
};

function stopCamera(){
 if(stream){stream.getTracks().forEach(t=>t.stop());stream=null;}
 $("camera").srcObject=null;$("cameraArea").hidden=true;
}
$("stopCamera").onclick=stopCamera;

function showPhoto(file){
 if(!file||!file.type.startsWith("image/")){say("Please select a valid image.");return;}
 if(file.size>10*1024*1024){say("Choose an image smaller than 10 MB.");return;}
 if(photoUrl)URL.revokeObjectURL(photoUrl);
 photoUrl=URL.createObjectURL(file);
 $("photoPreview").src=photoUrl;
 $("photoName").textContent="Selected: "+file.name;
 $("previewPanel").hidden=false;
 $("detectedPanel").hidden=false;
 say("Photo loaded. Enter the car details manually if needed.");
}
$("chooseImage").onclick=()=>$("galleryInput").click();
$("galleryInput").addEventListener("change",e=>{
 const file=e.target.files[0];if(file)showPhoto(file);e.target.value="";
});

$("capture").onclick=()=>{
 const video=$("camera");
 if(!video.videoWidth){say("Wait until the camera preview appears.");return;}
 const canvas=document.createElement("canvas");
 canvas.width=video.videoWidth;canvas.height=video.videoHeight;
 canvas.getContext("2d").drawImage(video,0,0);
 canvas.toBlob(blob=>{
  if(blob)showPhoto(new File([blob],"camera-car.jpg",{type:"image/jpeg"}));
  else say("Could not capture photo.");
 },"image/jpeg",0.9);
};

$("clearPhoto").onclick=()=>{
 if(photoUrl)URL.revokeObjectURL(photoUrl);
 photoUrl=null;$("photoPreview").removeAttribute("src");
 $("previewPanel").hidden=true;$("detectedPanel").hidden=true;
 $("scanModel").value="";$("scanColor").value="";$("scanYear").value="";
 $("photoName").textContent="";say("Photo removed.");
};

$("useDetails").onclick=()=>{
 const model=$("scanModel").value.trim();
 const color=$("scanColor").value.trim();
 const year=$("scanYear").value.trim();
 if(!model){say("Enter the car model first.");return;}
 say("Details entered: "+model+(color?" · "+color:"")+(year?" · "+year)+". Select a car below to make a booking.");
 $("carsSection").scrollIntoView({behavior:"smooth"});
};

onAuthStateChanged(auth,currentUser=>{
 user=currentUser;
 $("logout").hidden=!user;$("loginTop").hidden=!!user;
 $("accountPanel").hidden=!user;
 if(user){
  $("welcome").textContent="Signed in as "+user.email;
  $("authPanel").hidden=true;
 }else{
  $("myBookings").hidden=true;$("bookingPanel").hidden=true;
 }
});

renderCars();
</script>
</body>
</html>
