<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Digital Health Assistant</title>

<style>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  background: #f4f8fc;
}

.navbar {
  background: #0b3c5d;
  color: white;
  padding: 15px 40px;
  display: flex;
  justify-content: space-between;
}

.navbar a {
  color: white;
  margin-left: 20px;
  text-decoration: none;
}

.hero {
  height: 70vh;
  background: linear-gradient(rgba(0,0,0,.4), rgba(0,0,0,.4)),
  url("https://images.unsplash.com/photo-1580281657527-47f249e8f39c")
  center/cover no-repeat;
  display: flex;
  align-items: center;
  padding-left: 80px;
}

.hero-text {
  color: white;
  max-width: 520px;
}

.hero-text h1 {
  font-size: 42px;
}

.hero-text button {
  padding: 12px 25px;
  background: #f1c40f;
  border: none;
  cursor: pointer;
  border-radius: 5px;
}

.reminder {
  padding: 40px;
  text-align: center;
  background: #eaf2f8;
}

.reminder input, .reminder button {
  display: block;
  margin: 10px auto;
  padding: 10px;
  width: 260px;
}

.reminder button {
  background: #27ae60;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.medicine-list {
  margin-top: 30px;
}

.card {
  background: white;
  max-width: 500px;
  margin: 15px auto;
  padding: 15px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  gap: 15px;
}

.card img {
  width: 80px;
  height: 80px;
  border-radius: 10px;
  object-fit: cover;
  border: 2px solid #555;
}

footer {
  text-align: center;
  background: #0b3c5d;
  color: white;
  padding: 15px;
}
</style>
</head>

<body>

<!-- NAVBAR -->
<div class="navbar">
  <div>➕ Digital Health Assistant</div>
  <div>
    <a href="#">Home</a>
    <a href="#reminder">Reminder</a>
    <a href="#contact">Contact</a>
  </div>
</div>

<!-- HERO -->
<div class="hero">
  <div class="hero-text">
    <h1>Your Smart Medicine Reminder System</h1>
    <p>Add multiple medicines and never miss a dose.</p>
    <button onclick="scrollToReminder()">Get Started</button>
  </div>
</div>

<!-- REMINDER -->
<div class="reminder" id="reminder">
  <h2>Add Medicine</h2>

  <input type="text" id="medName" placeholder="Medicine Name">
  <input type="time" id="medTime">

  <!-- CAMERA + GALLERY -->
  <input type="file" id="medImage" accept="image/*" capture="environment">

  <button onclick="addMedicine()">Add Medicine</button>

  <!-- MEDICINE LIST -->
  <div class="medicine-list" id="medicineList"></div>
</div>

<footer id="contact">
  © 2025 Digital Health Assistant | Student Project
</footer>

<script>
let medicines = [];

function scrollToReminder() {
  document.getElementById("reminder")
  .scrollIntoView({ behavior: "smooth" });
}

function addMedicine() {
  const name = document.getElementById("medName").value;
  const time = document.getElementById("medTime").value;
  const imgFile = document.getElementById("medImage").files[0];

  if (!name || !time || !imgFile) {
    alert("Please fill all details");
    return;
  }

  const imgURL = URL.createObjectURL(imgFile);

  const medicine = {
    name: name,
    time: time,
    image: imgURL,
    notified: false
  };

  medicines.push(medicine);
  displayMedicines();

  // Clear inputs
  document.getElementById("medName").value = "";
  document.getElementById("medTime").value = "";
  document.getElementById("medImage").value = "";

  Notification.requestPermission();
}

function displayMedicines() {
  const list = document.getElementById("medicineList");
  list.innerHTML = "";

  medicines.forEach((med, index) => {
    list.innerHTML += `
      <div class="card">
        <img src="${med.image}">
        <div>
          <h3>${med.name}</h3>
          <p>⏰ ${med.time}</p>
        </div>
      </div>
    `;
  });
}

// CHECK REMINDERS
setInterval(() => {
  const now = new Date();
  const currentTime =
    now.getHours().toString().padStart(2,"0") + ":" +
    now.getMinutes().toString().padStart(2,"0");

  medicines.forEach(med => {
    if (med.time === currentTime && !med.notified) {
      new Notification("💊 Medicine Time!", {
        body: "Time to take " + med.name,
        icon: med.image
      });
      med.notified = true;
    }
  });
}, 1000);
</script>

</body>
</html>
