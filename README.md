<!DOCTYPE html>
<html>
<head>
  <title>Take Selfie</title>
</head>
<body style="text-align:center; font-family:Arial">

  <h2>Scan QR & Take Selfie</h2>

  <video id="video" width="300" autoplay></video>
  <br><br>

  <button onclick="takePhoto()">Take Selfie</button>
  <br><br>

  <canvas id="canvas" width="300" height="300"></canvas>

  <script>
    navigator.mediaDevices.getUserMedia({
      video: { facingMode: "user" }
    })
    .then(stream => {
      document.getElementById("video").srcObject = stream;
    })
    .catch(err => {
      alert("Camera permission required!");
    });

    function takePhoto() {
      const canvas = document.getElementById("canvas");
      const video = document.getElementById("video");
      const ctx = canvas.getContext("2d");
      ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
    }
  </script>

</body>
</html>
