<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>學校社團選填系統</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", "微軟正黑體", sans-serif;
      background: #f4f7fb;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    .container {
      max-width: 900px;
      margin: 0 auto;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
      color: #1f3b68;
    }
    .panel {
      background: #fff;
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 20px;
      box-shadow: 0 2px 10px rgba(0,0,0,0.08);
    }
    h2 {
      margin-top: 0;
      color: #224b7a;
    }
    .form-group {
      margin-bottom: 15px;
    }
    label {
      display: block;
      margin-bottom: 6px;
      font-weight: bold;
    }
    input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 8px;
      box-sizing: border-box;
      font-size: 16px;
    }
    button {
      border: none;
      border-radius: 8px;
      padding: 10px 16px;
      font-size: 16px;
      cursor: pointer;
      background: #2d7ff9;
      color: white;
    }
    button:hover {
      background: #1f68d6;
    }
    button:disabled {
      background: #999;
      cursor: not-allowed;
    }
    .hidden {
      display: none;
    }
    .alert {
      margin-top: 12px;
      padding: 10px 12px;
      border-radius: 8px;
      font-weight: bold;
    }
    .alert-success {
      background: #e7f7ec;
      color: #1c7c3a;
      border: 1px solid #bce5c8;
    }
    .alert-danger {
      background: #fdecec;
      color: #b42318;
      border: 1px solid #f5b5b5;
    }
    .club-list {
      display: grid;
      grid-template-columns: 1fr;
      gap: 12px;
    }
    .club-item {
      border: 1px solid #dbe4f0;
      border-radius: 10px;
      padding: 14px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: #fafcff;
    }
    .club-info {
      line-height: 1.5;
    }
    .club-name {
      font-size: 18px;
      font-weight: bold;
      color: #16304f;
    }
    .club-count {
      color: #555;
      margin-top: 4px;
    }
    .club-full {
      color: #d93025;
      font-weight: bold;
    }
    .admin-table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }
    .admin-table th,
    .admin-table td {
      border: 1px solid #cfd8e3;
      padding: 10px;
      text-align: center;
    }
    .admin-table th {
      background: #224b7a;
      color: white;
    }
    .admin-table tr:nth-child(even) {
      background: #f8fbff;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>學校社團選填系統</h1>

    <!-- 學生資料輸入 -->
    <div id="studentFormPanel" class="panel">
      <h2>學生資料輸入</h2>
      <div class="form-group">
        <label for="classInput">班級</label>
        <input type="text" id="classInput" placeholder="請輸入班級（例如：801）" />
      </div>
      <div class="form-group">
        <label for="studentIdInput">學號</label>
        <input type="text" id="studentIdInput" placeholder="請輸入學號" />
      </div>
      <div class="form-group">
        <label for="nameInput">姓名</label>
        <input type="text" id="nameInput" placeholder="請輸入姓名" />
      </div>
      <button id="checkBtn">下一步 選社團</button>
      <div id="studentMsg" class="alert hidden"></div>
    </div>

    <!-- 社團選填區 -->
    <div id="clubPanel" class="panel hidden">
      <h2>請選擇社團</h2>
      <p>每位學生只能選一個社團，選過後不能再選。</p>
      <div id="clubList" class="club-list"></div>
      <div id="clubMsg" class="alert hidden"></div>
    </div>

    <!-- 管理者登入 -->
    <div id="adminLoginPanel" class="panel">
      <h2>管理者登入</h2>
      <div class="form-group">
        <label for="adminUser">帳號</label>
        <input type="text" id="adminUser" placeholder="請輸入帳號" />
      </div>
      <div class="form-group">
        <label for="adminPass">密碼</label>
        <input type="password" id="adminPass" placeholder="請輸入密碼" />
      </div>
      <button id="loginBtn">登入</button>
      <div id="adminMsg" class="alert hidden"></div>
    </div>

    <!-- 管理介面 -->
    <div id="adminPanel" class="panel hidden">
      <h2>管理者介面</h2>
      <button id="deleteAllBtn" style="background:#d93025; margin-bottom:10px;">刪除全部選社資料</button>
      <div id="tableArea"></div>
    </div>
  </div>

  <script>
    const CLUBS = [
      { id: "badminton", name: "羽球社", max: 25 },
      { id: "handball", name: "手球社", max: 25 },
      { id: "clay", name: "黏土社", max: 25 },
      { id: "movie", name: "電影欣賞社", max: 25 },
      { id: "go", name: "圍棋社", max: 25 },
      { id: "chess", name: "象棋社", max: 25 },
      { id: "shadow", name: "山中剪影社", max: 12 },
      { id: "tableTennis", name: "桌球社", max: 12 },
      { id: "fashion", name: "時尚造型社", max: 25 },
    ];

    const ADMIN_USER = "admin";
    const ADMIN_PASS = "sjjh313";

    let currentStudent = null;

    // 讀取資料
    function loadStudents() {
      const data = localStorage.getItem("students");
      return data ? JSON.parse(data) : [];
    }

    function saveStudents(data) {
      localStorage.setItem("students", JSON.stringify(data));
    }

    function loadCounts() {
      const data = localStorage.getItem("clubCounts");
      return data ? JSON.parse(data) : {};
    }

    function saveCounts(data) {
      localStorage.setItem("clubCounts", JSON.stringify(data));
    }

    // 初始化社團人數
    function initCounts() {
      let counts = loadCounts();
      if (Object.keys(counts).length === 0) {
        counts = {};
        CLUBS.forEach(c => counts[c.id] = 0);
      }
      return counts;
    }

    let students = loadStudents();
    let clubCounts = initCounts();

    // 顯示訊息
    function showMsg(id, text, type) {
      const el = document.getElementById(id);
      el.textContent = text;
      el.className = `alert alert-${type}`;
      el.classList.remove("hidden");
    }

    function hideMsg(id) {
      document.getElementById(id).classList.add("hidden");
    }

    // 檢查學生是否已選社
    function studentAlreadySelected(className, studentId) {
      return students.some(s => s.class === className && s.studentId === studentId);
    }

    // 渲染社團選項
    function renderClubs() {
      const list = document.getElementById("clubList");
      list.innerHTML = "";

      CLUBS.forEach(club => {
        const count = clubCounts[club.id] || 0;
        const full = count >= club.max;

        const item = document.createElement("div");
        item.className = "club-item";

        const info = document.createElement("div");
        info.className = "club-info";
        info.innerHTML = `
          <div class="club-name">${club.name}</div>
          <div class="club-count ${full ? "club-full" : ""}">
            目前人數：${count} / ${club.max} ${full ? "（已額滿）" : ""}
          </div>
        `;

        const btn = document.createElement("button");
        btn.textContent = full ? "已額滿" : "選擇";
        btn.disabled = full;

        btn.addEventListener("click", () => {
          if (!currentStudent) return;

          if (studentAlreadySelected(currentStudent.class, currentStudent.studentId)) {
            showMsg("clubMsg", "你已經選過社團，不能再重複選填。", "danger");
            return;
          }

          students.push({
            class: currentStudent.class,
            studentId: currentStudent.studentId,
            name: currentStudent.name,
            club: club.id,
          });

          clubCounts[club.id] = count + 1;
          saveStudents(students);
          saveCounts(clubCounts);

          alert(`你已成功選擇「${club.name}」社團`);  // 確認已選社團的提示
          showMsg("clubMsg", `成功選擇「${club.name}」`, "success");
          renderClubs();
        });

        item.appendChild(info);
        item.appendChild(btn);
        list.appendChild(item);
      });
    }

    // 學生資料送出
    document.getElementById("checkBtn").addEventListener("click", () => {
      const className = document.getElementById("classInput").value.trim();
      const studentId = document.getElementById("studentIdInput").value.trim();
      const name = document.getElementById("nameInput").value.trim();

      if (!className || !studentId || !name) {
        showMsg("studentMsg", "請輸入完整的班級、學號與姓名。", "danger");
        return;
      }

      if (studentAlreadySelected(className, studentId)) {
        showMsg("studentMsg", "你已經選過社團，不能再重複選填。", "danger");
        return;
      }

      currentStudent = { class: className, studentId, name };
      hideMsg("studentMsg");
      document.getElementById("studentFormPanel").classList.add("hidden");
      document.getElementById("clubPanel").classList.remove("hidden");
      renderClubs();
    });

    // 管理者登入
    document.getElementById("loginBtn").addEventListener("click", () => {
      const user = document.getElementById("adminUser").value.trim();
      const pass = document.getElementById("adminPass").value.trim();

      if (user === ADMIN_USER && pass === ADMIN_PASS) {
        hideMsg("adminMsg");
        document.getElementById("adminLoginPanel").classList.add("hidden");
        document.getElementById("adminPanel").classList.remove("hidden");
        renderAdminTable();
      } else {
        showMsg("adminMsg", "帳號或密碼錯誤，請再試一次。", "danger");
      }
    });

    // 顯示管理者表格
    function renderAdminTable() {
      const area = document.getElementById("tableArea");
      if (students.length === 0) {
        area.innerHTML = "<p>目前沒有學生選社資料。</p>";
        return;
      }

      const table = document.createElement("table");
      table.className = "admin-table";
      table.innerHTML = `
        <thead>
          <tr>
            <th>班級</th>
            <th>學號</th>
            <th>姓名</th>
            <th>社團</th>
          </tr>
        </thead>
        <tbody>
          ${students.map(s => {
            const clubName = CLUBS.find(c => c.id === s.club)?.name || "未知";
            return `
              <tr>
                <td>${s.class}</td>
                <td>${s.studentId}</td>
                <td>${s.name}</td>
                <td>${clubName}</td>
              </tr>
            `;
          }).join("")}
        </tbody>
      `;
      area.innerHTML = "";
      area.appendChild(table);
    }

    // 刪除全部資料
    document.getElementById("deleteAllBtn").addEventListener("click", () => {
      const ok = confirm("確定要刪除全部選社資料嗎？此動作無法復原。");
      if (!ok) return;

      students = [];
      clubCounts = {};
      CLUBS.forEach(c => clubCounts[c.id] = 0);

      localStorage.removeItem("students");
      localStorage.removeItem("clubCounts");

      document.getElementById("tableArea").innerHTML = "<p>目前沒有學生選社資料。</p>";
      alert("已刪除全部選社資料");
    });

    // 初始檢查與修復人數
    window.addEventListener("load", () => {
      clubCounts = initCounts();
      students.forEach(s => {
        if (clubCounts[s.club] === undefined) clubCounts[s.club] = 0;
      });

      const rebuilt = {};
      CLUBS.forEach(c => rebuilt[c.id] = 0);
      students.forEach(s => {
        if (rebuilt[s.club] !== undefined) rebuilt[s.club]++;
      });
      clubCounts = rebuilt;
      saveStudents(students);
      saveCounts(clubCounts);
    });
  </script>
</body>
</html>
