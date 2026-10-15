
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#102b46">
<title>GoDrive | Car Rental</title>

<style>
*{box-sizing:border-box}
body{
  margin:0;background:#f3f6fb;color:#172b40;
  font-family:Arial,Helvetica,sans-serif
}
header{
  background:#102b46;color:white;padding:22px 5%;
  display:flex;justify-content:space-between;
  align-items:center;gap:15px;flex-wrap:wrap
}
.logo{font-size:27px;font-weight:bold}
.logo span{color:#65b6ff}
header p{margin:6px 0;color:#d1dfef}
button{
  cursor:pointer;border:0;border-radius:9px;
  background:#1769e0;color:white;padding:12px 16px;
  font-weight:bold;font-size:14px
}
button:disabled{opacity:.5;cursor:wait}
button.light{background:white;color:#102b46}
button.outline{background:#eaf1ff;color:#1456ad}
.container{max-width:1200px;margin:auto;padding:22px}
.hero{
  background:linear-gradient(110deg,#102b46,#20578a);
  color:white;padding:30px;border-radius:18px;margin-bottom:25px
}
.hero h1{font-size:clamp(27px,5vw,42px);margin:0 0 10px}
.hero p{line-height:1.6;color:#e1edfa}
.pill{
  display:inline-block;background:#d9edff;color:#124e86;
  border-radius:30px;padding:7px 12px;font-size:12px;
  font-weight:bold;margin-bottom:12px
}
h2{margin-top:28px}
.muted{color:#6b7c90;font-size:14px;line-height:1.6}
.car-grid{
  display:grid;grid-template-columns:repeat(3,minmax(0,1fr));
  gap:20px
}
.car-card{
  background:white;border-radius:16px;overflow:hidden;
  box-shadow:0 5px 22px #102b4610;
  border:1px solid #e8edf4;min-width:0
}
.car-image{
  width:100%;height:205px;object-fit:cover;
  background:#e3eaf2;display:block
}
.car-body{padding:17px}
.car-body h3{margin:0 0 8px;font-size:20px}
.car-info{font-size:13px;color:#66788c;line-height:1.7}
.car-desc{font-size:14px;line-height:1.6;min-height:44px}
.car-bottom{
  display:flex;justify-content:space-between;align-items:center;
  gap:10px;border-top:1px solid #e8edf4;padding-top:14px;
  margin-top:14px
}
.price{font-size:19px;font-weight:bold;color:#145ac5}
.price small{font-size:12px;font-weight:normal;color:#6b7c90}
.car-bottom button{padding:11px 13px}
.panel{
  background:white;padding:22px;border-radius:16px;
  margin:20px 0;box-shadow:0 4px 20px #102b460c
}
input{
  width:100%;padding:13px;border:1px solid #ccd6e2;
  border-radius:9px;margin:7px 0 14px;font-size:15px;
  background:white;color:#172b40
}
label{font-size:14px;font-weight:bold}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
nav{display:flex;gap:8px;flex-wrap:wrap}
nav button{flex:1}
.booking{
  border:1px solid #e0e7f0;border-radius:12px;
  padding:15px;margin:12px 0;background:#fff
}
.booking h3{margin-top:0}
.status{font-size:14px;color:#1769e0;font-weight:bold}
#notice{padding:12px 0;overflow-wrap:anywhere}
footer{text-align:center;padding:25px;color:#6b7c90;font-size:13px}
[hidden]{display:none!important}
@media(max-width:850px){
  .car-grid{grid-template-columns:repeat(2,minmax(0,1fr))}
}
@media(max-width:540px){
  .container{padding:14px}
  header{padding:18px}
  .car-grid{grid-template-columns:1fr}
  .car-image{height:220px}
  .hero{padding:22px}
  .form-grid{grid-template-columns:1fr}
  .car-desc{min-height:0}
}
</style>
</head>

<body>
<header>
  <div>
    <div class="logo">🚘 Go<span>Drive</span></div>
    <p>Your journey, your way.</p>
  </div>
  <div id="headerActions">
    <button class="light" id="loginTop">Login / Register</button>
    <button class="light" id="logout" hidden>Sign Out</button>
  </div>
</header>

<main class="container">
  <section class="hero">
    <span class="pill">YOUR NEXT JOURNEY STARTS HERE</span>
    <h1>Find the perfect car for your trip.</h1>
    <p>Explore our collection, choose your favourite model,
       and send a rental booking request in a few simple steps.</p>
    <button id="exploreBtn" class="light">Explore Cars ↓</button>
  </section>

  <section id="authPanel" class="panel" hidden>
    <h2 id="authTitle">Login to GoDrive</h2>
    <p class="muted">Sign in to request a booking and view your bookings.</p>
    <form id="authForm">
      <label for="email">Email address</label>
      <input id="email" type="email" autocomplete="email"
        placeholder="Enter your email" required>
      <label for="password">Password</label>
      <input id="password" type="password" minlength="6"
        autocomplete="current-password" placeholder="At least 6 characters"
        required>
      <button id="authSubmit" type="submit">Sign In</button>
      <button id="toggleRegister" type="button" class="outline">
        Create New Account
      </button>
    </form>
  </section>

  <section id="carsSection">
    <h2>Explore Our Cars</h2>
    <p class="muted">Choose a vehicle that fits your style and journey.</p>
    <div id="carGrid" class="car-grid"></div>
  </section>

  <section id="bookingPanel" class="panel" hidden>
    <h2>Book Your Car</h2>
    <div id="chosenCar"></div>
    <form id="bookingForm">
      <div class="form-grid">
        <div>
          <label for="pickup">Pickup date</label>
          <input id="pickup" type="date" required>
        </div>
        <div>
          <label for="returnDate">Return date</label>
          <input id="returnDate" type="date" required>
        </div>
      </div>
      <p id="estimate" class="price"></p>
      <p class="muted">This is an estimated rental price.
        Your request is not a confirmed reservation.</p>
      <button id="confirmBtn" type="submit">Send Booking Request</button>
      <button id="cancelBtn" class="outline" type="button">
        Cancel
      </button>
    </form>
  </section>

  <section id="myBookings" class="panel" hidden>
    <h2>My Bookings</h2>
    <button id="refreshBookings" class="outline" type="button">
      Refresh Bookings
    </button>
    <div id="bookingList"></div>
  </section>

  <section id="accountPanel" class="panel" hidden>
    <p id="welcome"></p>
    <button id="showBookings" type="button">My Bookings</button>
  </section>

  <p id="notice" role="status" aria-live="polite"></p>
</main>

<footer>
  © 2026 GoDrive Car Rental · Drive safely, travel happily.
</footer>

<script type="module">
import {initializeApp}
  from "https://www.gstatic.com/firebasejs/12.4.0/firebase-app.js";

import {
  getAuth,createUserWithEmailAndPassword,
  signInWithEmailAndPassword,onAuthStateChanged,signOut
} from "https://www.gstatic.com/firebasejs/12.4.0/firebase-auth.js";

import {
  getFirestore,collection,addDoc,getDocs,query,
  where,serverTimestamp
} from "https://www.gstatic.com/firebasejs/12.4.0/firebase-firestore.js";

// STEP 1: Paste your real Firebase configuration here.
const firebaseConfig = {
  apiKey: "PASTE_YOUR_API_KEY",
  authDomain: "PASTE_YOUR_AUTH_DOMAIN",
  projectId: "PASTE_YOUR_PROJECT_ID",
  storageBucket: "PASTE_YOUR_STORAGE_BUCKET",
  messagingSenderId: "PASTE_YOUR_MESSAGING_SENDER_ID",
  appId: "PASTE_YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

// Real car photographs. These are representative photos;
// verify exact model, trim and colour before advertising a car.
const cars = [
  {
    id:"swift",
    name:"Maruti Suzuki Swift",
    model:"Swift VXi",
    year:"2025",
    color:"Pearl Arctic White",
    fuel:"Petrol",
    transmission:"Manual",
    seats:5,
    price:1500,
    image:"https://images.unsplash.com/photo-1503376780353-7e6692767b70?auto=format&fit=crop&w=1000&q=85",
    description:"A compact and sporty hatchback, ideal for city travel and short weekend trips."
  },
  {
    id:"city",
    name:"Honda City",
    model:"City V CVT",
    year:"2025",
    color:"Modern Steel Metallic",
    fuel:"Petrol",
    transmission:"Automatic",
    seats:5,
    price:2200,
    image:"https://images.unsplash.com/photo-1492144534655-ae79c964c9d7?auto=format&fit=crop&w=1000&q=85",
    description:"A comfortable sedan with a spacious cabin, perfect for business trips and family journeys."
  },
  {
    id:"creta",
    name:"Hyundai Creta",
    model:"Creta SX",
    year:"2025",
    color:"Atlas White",
    fuel:"Petrol",
    transmission:"Automatic",
    seats:5,
    price:2800,
    image:"https://images.unsplash.com/photo-1519641471654-76ce0107ad1b?auto=format&fit=crop&w=1000&q=85",
    description:"A versatile SUV with a commanding driving position and room for family luggage."
  },
  {
    id:"nexon",
    name:"Tata Nexon",
    model:"Nexon Creative",
    year:"2025",
    color:"Daytona Grey",
    fuel:"Petrol",
    transmission:"Automatic",
    seats:5,
    price:2300,
    image:"https://images.unsplash.com/photo-1549317661-bd32c8ce0db2?auto=format&fit=crop&w=1000&q=85",
    description:"A practical compact SUV for everyday drives, city errands and weekend adventures."
  },
  {
    id:"innova",
    name:"Toyota Innova Crysta",
    model:"Innova GX",
    year:"2025",
    color:"Super White",
    fuel:"Diesel",
    transmission:"Manual",
    seats:7,
    price:3500,
    image:"https://images.unsplash.com/photo-1511919884226-fd3cad34687c?auto=format&fit=crop&w=1000&q=85",
    description:"A spacious people-mover for group outings, long-distance trips and family holidays."
  },
  {
    id:"thar",
    name:"Mahindra Thar",
    model:"Thar LX",
    year:"2025",
    color:"Red Rage",
    fuel:"Petrol",
    transmission:"Automatic",
    seats:4,
    price:4000,
    image:"https://images.unsplash.com/photo-1533473359331-0135ef1b58bf?auto=format&fit=crop&w=1000&q=85",
    description:"A bold lifestyle SUV for memorable road trips and outdoor adventures."
  }
];

let user = null;
let selectedCar = null;
let isRegister = false;

const $ = id => document.getElementById(id);
const money = n => "₹" + Number(n).toLocaleString("en-IN");

function say(text){
  $("notice").textContent = text;
}

function dateString(d){
  return d.getFullYear()+"-"+
    String(d.getMonth()+1).padStart(2,"0")+"-"+
    String(d.getDate()).padStart(2,"0");
}

const today = dateString(new Date());
$("pickup").min = today;
$("returnDate").min = today;

// Build the six car cards.
function renderCars(){
  $("carGrid").innerHTML = cars.map(car => `
    <article class="car-card">
      <img class="car-image"
        src="${car.image}"
        alt="${car.name} car photograph"
        loading="lazy"
        onerror="this.onerror=null;this.style.display='none'">
      <div class="car-body">
        <span class="pill">${car.year} MODEL</span>
        <h3>${car.name}</h3>
        <div class="car-info">
          <b>Variant:</b> ${car.model}<br>
          <b>Colour:</b> ${car.color}<br>
          <b>Fuel:</b> ${car.fuel}<br>
          <b>Transmission:</b> ${car.transmission}<br>
          <b>Seats:</b> ${car.seats}
        </div>
        <p class="car-desc">${car.description}</p>
        <div class="car-bottom">
          <div class="price">${money(car.price)}
            <small>/day</small>
          </div>
          <button data-car="${car.id}">Book Now</button>
        </div>
      </div>
    </article>
  `).join("");

  $("carGrid").querySelectorAll("[data-car]").forEach(button => {
    button.addEventListener("click", () => {
      chooseCar(button.dataset.car);
    });
  });
}

function chooseCar(id){
  selectedCar = cars.find(car => car.id === id);
  if(!selectedCar) return;

  if(!user){
    $("authPanel").hidden = false;
    $("email").focus();
    say("Please sign in or create an account before booking.");
    $("authPanel").scrollIntoView({behavior:"smooth"});
    return;
  }

  $("chosenCar").innerHTML = `
    <h3>${selectedCar.name} — ${selectedCar.model}</h3>
    <p class="muted">Colour: ${selectedCar.color} · ${selectedCar.seats} seats</p>
    <p class="price">${money(selectedCar.price)} per day</p>
  `;

  $("pickup").value = "";
  $("returnDate").value = "";
  $("returnDate").min = today;
  updateEstimate();

  $("bookingPanel").hidden = false;
  $("bookingPanel").scrollIntoView({behavior:"smooth"});
}

function updateEstimate(){
  const start = $("pickup").value;
  const end = $("returnDate").value;

  if(!selectedCar || !start || !end || end <= start){
    $("estimate").textContent = "Select valid pickup and return dates.";
    return;
  }

  const days = Math.round(
    (Date.parse(end+"T00:00:00") -
     Date.parse(start+"T00:00:00")) / 86400000
  );

  $("estimate").textContent =
    days+" day(s) · Estimated total "+money(days*selectedCar.price);
}

$("pickup").addEventListener("change", () => {
  $("returnDate").min = $("pickup").value || today;
  if($("returnDate").value &&
     $("returnDate").value <= $("pickup").value){
    $("returnDate").value = "";
  }
  updateEstimate();
});

$("returnDate").addEventListener("change", updateEstimate);

$("exploreBtn").addEventListener("click", () => {
  $("carsSection").scrollIntoView({behavior:"smooth"});
});

$("loginTop").addEventListener("click", () => {
  $("authPanel").hidden = !$("authPanel").hidden;
  $("authPanel").scrollIntoView({behavior:"smooth"});
});

$("toggleRegister").addEventListener("click", () => {
  isRegister = !isRegister;
  $("authTitle").textContent = isRegister
    ? "Create Your Account" : "Login to GoDrive";
  $("authSubmit").textContent = isRegister
    ? "Register" : "Sign In";
  $("toggleRegister").textContent = isRegister
    ? "Already have an account? Sign In"
    : "Create New Account";
});

$("authForm").addEventListener("submit", async e => {
  e.preventDefault();
  $("authSubmit").disabled = true;

  try{
    const email = $("email").value.trim();
    const password = $("password").value;

    if(isRegister){
      await createUserWithEmailAndPassword(auth,email,password);
      say("Account created successfully!");
    }else{
      await signInWithEmailAndPassword(auth,email,password);
      say("Login successful!");
    }
    $("authPanel").hidden = true;
  }catch(error){
    say("Authentication error: "+error.message);
  }finally{
    $("authSubmit").disabled = false;
  }
});

$("logout").addEventListener("click", async () => {
  try{
    await signOut(auth);
    say("You have signed out.");
  }catch(error){
    say(error.message);
  }
});

$("cancelBtn").addEventListener("click", () => {
  $("bookingPanel").hidden = true;
  selectedCar = null;
});

$("bookingForm").addEventListener("submit", async e => {
  e.preventDefault();
  if(!user || !selectedCar){
    say("Please sign in and select a car.");
    return;
  }

  const pickup = $("pickup").value;
  const returnDate = $("returnDate").value;
  const days = Math.round(
    (Date.parse(returnDate+"T00:00:00") -
     Date.parse(pickup+"T00:00:00")) / 86400000
  );

  if(pickup < today || days < 1){
    say("Choose a valid pickup date and a return date after pickup.");
    return;
  }

  $("confirmBtn").disabled = true;
  try{
    await addDoc(collection(db,"bookings"),{
      uid:user.uid,
      carId:selectedCar.id,
      carName:selectedCar.name,
      dailyRate:selectedCar.price,
      pickupDate:pickup,
      returnDate:returnDate,
      days:days,
      totalPrice:days*selectedCar.price,
      status:"requested",
      createdAt:serverTimestamp()
    });

    say("Your booking request has been saved!");
    $("bookingPanel").hidden = true;
    selectedCar = null;
    await loadBookings();
  }catch(error){
    say("Booking failed: "+error.message);
  }finally{
    $("confirmBtn").disabled = false;
  }
});

async function loadBookings(){
  if(!user){
    say("Please sign in to view bookings.");
    return;
  }

  $("myBookings").hidden = false;
  $("bookingList").textContent = "Loading bookings...";

  try{
    const q = query(
      collection(db,"bookings"),
      where("uid","==",user.uid)
    );
    const result = await getDocs(q);
    $("bookingList").innerHTML = "";

    if(result.empty){
      $("bookingList").textContent = "No bookings yet. Choose a car to start!";
    }else{
      const records = [];
      result.forEach(doc => records.push(doc.data()));
      records.sort((a,b) =>
        (b.createdAt?.seconds || 0) - (a.createdAt?.seconds || 0)
      );

      records.forEach(b => {
        const card = document.createElement("article");
        card.className = "booking";

        const title = document.createElement("h3");
        title.textContent = "🚘 "+b.carName;

        const dates = document.createElement("p");
        dates.textContent =
          "Pickup: "+b.pickupDate+" · Return: "+b.returnDate;

        const total = document.createElement("p");
        total.textContent = "Estimated total: "+money(b.totalPrice);

        const status = document.createElement("p");
        status.className = "status";
        status.textContent = "Status: "+b.status;

        card.append(title,dates,total,status);
        $("bookingList").append(card);
      });
    }
    $("myBookings").scrollIntoView({behavior:"smooth"});
  }catch(error){
    $("bookingList").textContent =
      "Could not load bookings: "+error.message;
  }
}

$("showBookings").addEventListener("click",loadBookings);
$("refreshBookings").addEventListener("click",loadBookings);

onAuthStateChanged(auth, currentUser => {
  user = currentUser;
  $("logout").hidden = !user;
  $("loginTop").hidden = !!user;
  $("accountPanel").hidden = !user;

  if(user){
    $("welcome").textContent = "Signed in as "+user.email;
    $("authPanel").hidden = true;
  }else{
    $("myBookings").hidden = true;
    $("bookingPanel").hidden = true;
    $("bookingList").innerHTML = "";
  }
});

renderCars();
</script>
</body>
</html>
