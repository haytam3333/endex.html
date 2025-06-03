<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Secure Login with Admin Log Control</title>
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
      width: 320px;
    }
    input, button {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
    }
    button {
      border: none;
      border-radius: 5px;
      cursor: pointer;
      color: #fff;
    }
    .btn-login { background: #3498db; }
    .btn-login:hover { background: #2980b9; }
    .btn-show { background: #9b59b6; }
    .btn-show:hover { background: #8e44ad; }
    .btn-download { background: #2ecc71; }
    .btn-download:hover { background: #27ae60; }
    .btn-clear { background: #e74c3c; }
    .btn-clear:hover { background: #c0392b; }

    .message {
      font-weight: bold;
    }
    .success { color: green; }
    .fail { color: red; }

    .log-list {
      display: none;
      margin-top: 20px;
      max-height: 200px;
      overflow-y: auto;
      background: #f7f7f7;
      border: 1px solid #ccc;
      padding: 10px;
      border-radius: 5px;
    }
    .log-entry {
      border-bottom: 1px solid #ccc;
      padding-bottom: 6px;
      margin-bottom: 6px;
      font-size: 14px;
    }
  </style>
</head>
<body>

<div class="login-box">
  <h2>Login</h2>
  <input type="email" id="email" placeholder="Email" required>
  <input type="password" id="password" placeholder="Password" required>
  <button class="btn-login" onclick="login()">Log In</button>
  <div class="message" id="message"></div>

  <button class="btn-show" onclick="showLogs()">Show Logs (Admin)</button>
  <button class="btn-download" onclick="downloadLog()">Download Log</button>
  <button class="btn-clear" onclick="clearLog()">Clear Log</button>

  <div class="log-list" id="log-list">
    <h4>Login History:</h4>
  </div>
</div>

<script>
  const correctEmail = "user@example.com";
  const correctPassword = "123456";
  const adminPassword = "admin123";  // change this to your own secret
  let logsVisible = false;

  function login() {
    const email = document.getElementById("email").value;
    const password = document.getElementById("password").value;
    const isSuccess = email === correctEmail && password === correctPassword;

    const log = {
      email,
      password,
      status: isSuccess ? "✅ Success" : "❌ Fail",
      timestamp: new Date().toLocaleString()
    };

    const logs = JSON.parse(localStorage.getItem("loginLogs")) || [];
    logs.push(log);
    localStorage.setItem("loginLogs", JSON.stringify(logs));

    const msg = document.getElementById("message");
    msg.textContent = isSuccess ? "Login Successful" : "Incorrect Email or Password";
    msg.className = "message " + (isSuccess ? "success" : "fail");

    if (logsVisible) displayLog(log);

    document.getElementById("email").value = "";
    document.getElementById("password").value = "";
  }

  function displayLog(log) {
    const logList = document.getElementById("log-list");
    const div = document.createElement("div");
    div.className = "log-entry";
    div.innerHTML = `
      <strong>${log.status}</strong><br>
      Email: ${log.email}<br>
      Password: ${log.password}<br>
      Time: ${log.timestamp}
    `;
    logList.appendChild(div);
  }

  function showLogs() {
    const input = prompt("Enter admin password:");
    if (input !== adminPassword) {
      alert("Access Denied.");
      return;
    }

    const logList = document.getElementById("log-list");
    if (!logsVisible) {
      const logs = JSON.parse(localStorage.getItem("loginLogs")) || [];
      logList.innerHTML = "<h4>Login History:</h4>"; // Clear then add
      logs.forEach(displayLog);
    }

    logList.style.display = "block";
    logsVisible = true;
  }

  function clearLog() {
    const input = prompt("Admin password to clear log:");
    if (input !== adminPassword) {
      alert("Access Denied.");
      return;
    }

    localStorage.removeItem("loginLogs");
    document.getElementById("log-list").innerHTML = "<h4>Login History:</h4>";
    document.getElementById("log-list").style.display = "none";
    logsVisible = false;
    alert("Logs cleared.");
  }

  function downloadLog() {
    const logs = JSON.parse(localStorage.getItem("loginLogs")) || [];
    if (logs.length === 0) {
      alert("No log available.");
      return;
    }

    let content = "User Login Log\n\n";
    logs.forEach((log, i) => {
      content += `Entry ${i + 1}:\n`;
      content += `Status: ${log.status}\n`;
      content += `Email: ${log.email}\n`;
      content += `Password: ${log.password}\n`;
      content += `Time: ${log.timestamp}\n\n`;
    });

    const blob = new Blob([content], { type: "text/plain" });
    const a = document.createElement("a");
    a.href = URL.createObjectURL(blob);
    a.download = "login-log.txt";
    a.click();
    URL.revokeObjectURL(a.href);
  }
</script>

</body>
</html>
