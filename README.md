<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Send High-Accuracy Location</title>
  <script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</head>
<body>

<h2>📍 Click to Send Your Location</h2>
<button onclick="getLocation()" style="padding: 10px 20px; font-size: 18px;">Send Location</button>

<script>
const botToken = "YOUR_NEW_BOT_TOKEN";   // ضع التوكن الجديد هنا
const chatId   = "1538488453";           // نفس ID الخاص بك

function sendLocationToTelegram(lat, lon, acc) {
  const url = `https://api.telegram.org/bot${botToken}/sendMessage`;
  const text =
    `📍 User Location:\n` +
    `🌐 Latitude: ${lat}\n` +
    `🌐 Longitude: ${lon}\n` +
    `🎯 Accuracy: ${acc} meters`;

  axios.post(url, {
    chat_id: chatId,
    text: text
  })
  .then(() => {
    alert("✔ Location sent successfully!");
  })
  .catch((err) => {
    console.error("Telegram error:", err);
    alert("❌ Failed to send location");
  });
}

function getLocation() {
  if (!navigator.geolocation) {
    alert("❌ Geolocation is not supported on this device.");
    return;
  }

  navigator.geolocation.getCurrentPosition(
    (pos) => {
      const lat = pos.coords.latitude;
      const lon = pos.coords.longitude;
      const acc = pos.coords.accuracy;

      sendLocationToTelegram(lat, lon, acc);
    },
    (err) => {
      console.error("Location error:", err);
      alert("❌ Failed to get location. You must click Allow.");
    },
    {
      enableHighAccuracy: true,
      timeout: 15000,
      maximumAge: 0
    }
  );
}
</script>

</body>
</html>
