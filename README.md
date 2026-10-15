
<!DOCTYPE html>
<html lang="en">
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Car Rental App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f5fa;
      margin: 0;
      padding: 20px;
    }
    .app {
      max-width: 420px;
      margin: 20px auto;
      background: white;
      padding: 22px;
      border-radius: 18px;
    }
    h1 { color: #153e75; }
    .car {
      background: #eaf1ff;
      padding: 16px;
      margin: 12px 0;
      border-radius: 12px;
    }
    button {
      background: #155eef;
      color: white;
      border: 0;
      padding: 12px;
      border-radius: 8px;
      width: 100%;
    }
  </style>
</head>
<body>
  <div class="app">
    <h1>🚘 GoDrive</h1>
    <p>Find your perfect rental car.</p>

    <div class="car">
      <h3>Swift Dzire</h3>
      <p>₹1,500 per day</p>
      <button onclick="bookCar('Swift Dzire')">
        Book Now
      </button>
    </div>

    <div class="car">
      <h3>Hyundai Creta</h3>
      <p>₹2,500 per day</p>
      <button onclick="bookCar('Hyundai Creta')">
        Book Now
      </button>
    </div>

    <p id="message"></p>
  </div>

  <script>
    function bookCar(car) {
      document.getElementById("message").textContent =
        "You selected " + car + "!";
    }
  </script>
</body>
</html>
