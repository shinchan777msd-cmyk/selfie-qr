<!DOCTYPE html>
<html>
<head>
  <title>Private Selfie Access</title>
</head>
<body style="text-align:center; font-family:Arial">

<div id="login">
  <h2>Private Access</h2>
  <input type="password" id="pass" placeholder="Enter Password">
  <br><br>
  <button onclick="checkPass()">Enter</button>
</div>

<div id="camera" style="display:none">
  <h2>Take Selfie</h2>
  <video id="video" width="300" autoplay></video>
  <br><br>
  <button onclick="takePhoto()">Take Selfie</button>
  <br><br>
  <canvas id="canvas" width="300" height="300"></canvas>
</div>

<script>
  function checkPass() {
    if (document.getElementById("pass").value === "admin123") {
      document.getElementById("login").style.display = "none";
      document.getElementById("camera").style.display = "block";
      startCamera();
    } else {
      alert("Wrong Password!");
    }
  }

  function startCamera() {
    navigator.mediaDevices.getUserMedia({ video: { facingMode: "user" } })
      .then(stream => {
        document.getElementById("video").srcObject = stream;
      });
  }

  function takePhoto() {
    const canvas = document.getElementById("canvas");
    const video = document.getElementById("video");
    canvas.getContext("2d").drawImage(video, 0, 0, canvas.width, canvas.height);
  }
</script>

</body>
</html>
