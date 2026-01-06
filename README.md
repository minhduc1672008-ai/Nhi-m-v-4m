<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<title>Web Nhiệm Vụ</title>
<style>
body {
  background:#111;
  color:#fff;
  font-family:Arial;
  text-align:center;
  padding:30px;
}
input, button {
  padding:10px;
  margin:5px;
  border-radius:5px;
  border:none;
}
button {
  background:gold;
  color:black;
}
.box {
  background:#222;
  padding:20px;
  margin:15px auto;
  max-width:400px;
  border-radius:8px;
}
.hidden { display:none; }
</style>
</head>

<body>

<h1>Web Nhiệm Vụ</h1>

<!-- ĐĂNG KÝ / ĐĂNG NHẬP -->
<div class="box" id="authBox">
  <h3>Đăng ký / Đăng nhập</h3>
  <input id="username" placeholder="Tên đăng nhập"><br>
  <input id="password" type="password" placeholder="Mật khẩu"><br>
  <button onclick="register()">Đăng ký</button>
  <button onclick="login()">Đăng nhập</button>
</div>

<!-- USER -->
<div class="box hidden" id="userBox">
  <h3>Xin chào <span id="userName"></span></h3>
  <p>Vàng: <span id="gold">0</span></p>
  <button onclick="doTask()">Làm nhiệm vụ</button>
  <button onclick="logout()">Đăng xuất</button>
</div>

<!-- ADMIN -->
<div class="box hidden" id="adminBox">
  <h3>Trang Admin (Chủ Web)</h3>
  <input id="taskReward" placeholder="Số vàng thưởng">
  <button onclick="createTask()">Tạo nhiệm vụ</button>
  <p>Nhiệm vụ hiện tại: <span id="taskInfo"></span></p>
</div>

<script>
let users = JSON.parse(localStorage.getItem("users")) || {};
let admin = localStorage.getItem("admin");
let currentUser = localStorage.getItem("currentUser");
let task = JSON.parse(localStorage.getItem("task")) || { reward: 10 };

function save() {
  localStorage.setItem("users", JSON.stringify(users));
  localStorage.setItem("task", JSON.stringify(task));
}

function register() {
  let u = username.value;
  let p = password.value;
  if (!u || !p) return alert("Nhập đủ đi");

  if (users[u]) return alert("Tài khoản tồn tại");

  users[u] = { password: p, gold: 0 };

  if (!admin) {
    admin = u;
    localStorage.setItem("admin", admin);
    alert("Bạn là CHỦ WEB");
  }

  save();
  alert("Đăng ký xong");
}

function login() {
  let u = username.value;
  let p = password.value;

  if (!users[u] || users[u].password !== p)
    return alert("Sai tài khoản");

  currentUser = u;
  localStorage.setItem("currentUser", u);
  loadUI();
}

function loadUI() {
  authBox.classList.add("hidden");
  userBox.classList.remove("hidden");
  userName.innerText = currentUser;
  gold.innerText = users[currentUser].gold;

  if (currentUser === admin) {
    adminBox.classList.remove("hidden");
    taskInfo.innerText = task.reward + " vàng";
  }
}

function doTask() {
  users[currentUser].gold += task.reward;
  gold.innerText = users[currentUser].gold;
  save();
}

function createTask() {
  let r = parseInt(taskReward.value);
  if (isNaN(r)) return alert("Nhập số");
  task.reward = r;
  taskInfo.innerText = r + " vàng";
  save();
}

function logout() {
  localStorage.removeItem("currentUser");
  location.reload();
}

if (currentUser) loadUI();
</script>

</body>
</html>
