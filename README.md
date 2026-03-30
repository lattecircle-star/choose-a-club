<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <title>社團選填系統</title>
  <style>
    body {
      font-family: '微軟正黑體', sans-serif;
      margin: 20px;
      background: #f5f7fa;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    h2 {
      color: #34495e;
      border-bottom: 2px solid #3498db;
      padding-bottom: 5px;
    }
    .container {
      max-width: 800px;
      margin: 0 auto;
    }
    .panel {
      background: white;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.1);
      margin-bottom: 20px;
    }
    .form-group {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }
    input, select {
      width: 100%;
      padding: 8px;
      border: 1px solid #ddd;
      border-radius: 4px;
      box-sizing: border-box;
    }
    button {
      background: #3498db;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background: #2980b9;
    }
    button:disabled {
      background: #95a5a6;
      cursor: not-allowed;
    }
    .club-list {
      display: grid;
      gap: 10px;
    }
    .club-item {
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 4px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }
    .club-name {
      font-weight: bold;
    }
    .club-count {
      color: #e74c3c;
    }
    .club-full {
      color: #c0392b;
      font-weight: bold;
    }
    .club-status {
      font-size: 0.9em;
      margin-top: 5px;
    }
    .admin-panel {
      background: #f9f9f9;
      padding: 15px;
      border: 1px dashed #3498db;
    }
    .admin-table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
    .admin-table th,
    .admin-table td {
      border: 1px solid #ddd;
      padding: 8px;
    }
    .admin-table th {
      background: #34495e;
      color: white;
    }
    .admin-table td {
      background: #fff;
    }
    .alert {
      padding: 10px;
      margin: 10px 0;
      border-radius: 4px;
      text-align: center;
      font-weight: bold;
    }
    .alert-success {
      background: #d4edda;
      color: #155724;
      border: 1px solid #c3e6cb;
    }
    .alert-danger {
      background: #f8d7da;
      color: #721c24;
      border: 1px solid #f5c6cb;
    }
    .hidden {
      display: none;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>學校社團選填系統</h1>

    <!-- 學生輸入區 -->
    <div id="studentInput" class="panel">
      <h2>學生資料</h2>
      <div class="form-group">
        <label>班級</label>
        <input type="text" id="classInput" placeholder="例如：101" />
      </div>
      <div class="form-group">
        <label>學號</label>
        <input type="text" id="studentIdInput" placeholder="例如：12345" />
      </div>
      <div class="form-group">
        <label>姓名</label>
        <input type="text" id="nameInput" placeholder="你的姓名" />
      </div>
      <button id="nextBtn">下一步 選社團</button>
      <div id="alertStudent" class="alert hidden"></div>
    </div>

    <!-- 社團選填區 -->
    <div id="clubSelect" class="panel hidden">
      <h2>請選擇社團</h2>
      <p>目前每人只能選一個社團，已滿人之社團無法再選。</p>
      <div id="clubList" class="club-list"></div>
      <div id="alertSelect" class="alert hidden"></div>
    </div>

    <!-- 管理者登入區 -->
    <div id="adminLogin" class="panel">
      <h2>管理者登入</h2>
      <div class="form-group">
        <label>帳號</label>
        <input type="text" id="adminUser" placeholder="admin" />
      </div>
      <div class="form-group">
        <label>密碼</label>
        <input type="password" id="adminPass" placeholder="sjjh313" />
      </div>
      <button id="loginBtn">登入</button>
      <div id="alertAdmin" class="alert hidden"></div>
    </div>

    <!-- 管理介面 -->
    <div id="adminPanel" class="panel admin-panel hidden">
      <h2>管理介面</h2>
      <p>以下為所有學生選社情況：</p>
      <div id="adminTableContainer"></div>
    </div>
  </div>

  <script>
    // =========== 定義資料 ===========
    const CLUBS = [
      { id: "badminton",  name: "羽球社",    max: 25 },
      { id: "handball",   name: "手球社",    max: 25 },
      { id: "clay",       name: "黏土社",    max: 25 },
      { id: "movie",      name: "電影欣賞社", max: 25 },
      { id: "go",         name: "圍棋社",    max: 25 },
      { id: "chess",      name: "象棋社",    max: 25 },
      { id: "shadow",     name: "山中剪影社", max: 12 },
      { id: "tableTennis",name: "桌球社",    max: 12 },
      { id: "fashion",    name: "時尚造型社", max: 25 },
    ];

    const ADMIN_USER = "admin";
    const ADMIN_PASS = "sjjh313";

    // 用 localStorage 保存資料
    function loadStudents() {
      const data = localStorage.getItem("students");
      return data ? JSON.parse(data) : [];
    }

    function saveStudents(students) {
      localStorage.setItem("students", JSON.stringify(students));
    }

    function loadCounts() {
      const data = localStorage.getItem("clubCounts");
      if (!data) {
        // 初始化社團人數
        const counts = {};
        CLUBS.forEach(c => (counts[c.id] = 0));
        localStorage.setItem("clubCounts", JSON.stringify(counts));
        return counts;
      }
      return JSON.parse(data);
    }

    function saveCounts(counts) {
      localStorage.setItem("clubCounts", JSON.stringify(counts));
    }

    let students = loadStudents();
    let clubCounts = loadCounts();

    // =========== 工具函式 ===========
    function showAlert(elId, msg, type) {
      const el = document.getElementById(elId);
      el.textContent = msg;
      el.className = "alert " + (type === "success" ? "alert-success" : "alert-danger");
      el.classList.remove("hidden");
    }

    function hideAlert(elId) {
      document.getElementById(elId).classList.add("hidden");
    }

    function isStudentExist(class_, id_, name_) {
      return students.some(s => s.class === class_ && s.studentId === id_);
    }

    // =========== 學生選社流程 ===========
    document.getElementById("nextBtn").addEventListener("click", () => {
      const class_ = document.getElementById("classInput").value.trim();
      const id_ = document.getElementById("studentIdInput").value.trim();
      const name = document.getElementById("nameInput").value.trim();

      if (!class_ || !id_ || !name) {
        showAlert("alertStudent", "請填寫完整班級、學號與姓名。", "danger");
        return;
      }

      if (isStudentExist(class_, id_, name)) {
        showAlert("alertStudent", "您已報名過一個社團，無法再更改。",
