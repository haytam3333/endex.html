<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Login Page</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }
    .login-box {
      background: #fff;
      padding: 30px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      width: 300px;
    }
    input {
      display: block;
      width: 100%;
      padding: 10px;
      margin: 10px 0;
    }
    button {
      padding: 10px;
      width: 100%;
      background: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background: #2980b9;
    }
    .message {
      margin-top: 10px;
      font-weight: bold;
    }
    .log-box {
      margin-top: 20px;
      background: #eee;
      padding: 10px;
      border-radius: 5px;
      font-size: 14px;
    }
  </style>
</head>
<body>

<div class="login-box">
  <h2>Login</h2>
  <input type="email" id="email" placeholder="Enter email" required>
  <input type="password" id="password" placeholder="Enter password" required>
  <button onclick="login()">Log In</button>
  <div class="message" id="message"></div>

  <div class="log-box" id="log-box" style="display:none;">
    <p><strong>Email:</strong> <span id="log-email"></span></p>
    <p><strong>Password:</strong> <span id="log-password"></span></p>
  </div>
</div>

<script>
  function login() {
    const email = document.getElementById('email').value;
    const password = document.getElementById('password').value;

    const correctEmail = "user@example.com";
    const correctPassword = "123456";

    const message = document.getElementById("message");
    const logBox = document.getElementById("log-box");

    // Show log regardless of success
    document.getElementById("log-email").textContent = email;
    document.getElementById("log-password").textContent = password;
    logBox.style.display = "block";

    if (email === correctEmail && password === correctPassword) {
      console.log("✅ Login successful");
      message.style.color = "green";
      message.textContent = "Login successful!";
    } else {
      console.log("❌ Invalid email or password");
      message.style.color = "red";
      message.textContent = "Invalid email or password.";
    }
  }
</script>

</body>
</html>
