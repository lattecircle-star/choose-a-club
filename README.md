<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>學校社團選填系統</title>
  <style>
    body {
      font-family: "Microsoft JhengHei", sans-serif;
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
    .success {
      background: #e7f7ec;
      color: #1c7c3a;
      border: 1px solid #bce5c8;
    }
    .danger {
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
    .full {
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

    <div id="studentFormPanel" class="panel">
      <h2>學生資料輸入</h2>
      <div class="form-group">
        <label for="classInput">班級</label>
        <input type="text" id="classInput" placeholder="例如：801" />
      </div>
      <div class="form-group">
        <label for="studentIdInput">學號</label>
        <input type="text" id="studentIdInput" placeholder="例如：15" />
      </div>
      <div class="form-group">
        <label for="nameInput">姓名</label>
        <input type="text" id="nameInput" placeholder="例如：王小明" />
      </div>
      <button id="checkBtn">下一步</button>
      <div id="studentMsg" class="alert hidden"></div>
    </div>

    <div id="clubPanel" class="panel hidden">
      <h2>請選擇社團</h2>
      <p>每位學生只能選一個社團，選過後不能再選。</p>
      <div id="clubList" class="club-list"></div>
      <div id="clubMsg" class="alert hidden"></div>
    </div>

    <div id="adminLoginPanel" class="panel">
      <h2>管理者登入</h2>
      <div class="form-group">
        <label for="adminUser">帳號</label>
        <input type="text" id="adminUser" placeholder="admin" />
      </div>
      <div class="form-group">
        <label for="adminPass">密碼</label>
        <input type="password" id="adminPass" placeholder="sjjh313" />
      </div>
      <button id="loginBtn">登入</button>
      <div id="adminMsg" class="alert hidden"></div>
    </div>

    <div id="adminPanel" class="panel hidden">
      <h2>管理者介面</h2>
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
      { id: "fashion", name: "時尚造型社", max: 25 }
    ];

    const ADMIN_USER = "admin";
    const ADMIN_PASS = "sjjh313";

    let currentStudent = null;

    function loadStudents() {
      return JSON.parse(localStorage.getItem("students")) || [];
    }

    function saveStudents(data) {
      localStorage.setItem("students", JSON.stringify(data));
    }

    function loadCounts() {
      return JSON.parse(localStorage.getItem("clubCounts")) || {};
    }

    function saveCounts(data) {
      localStorage.setItem("clubCounts", JSON.stringify(data));
    }

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

    function showMsg(id, text, type) {
      const el = document.getElementById(id);
      el.textContent = text;
      el.className = `alert ${type}`;
      el.classList.remove("hidden");
    }

    function hideMsg(id) {
      document.getElementById(id).classList.add("hidden");
    }

    function studentAlreadySelected(className, studentId) {
      return students.some(s => s.class === className && s.studentId === studentId);
    }

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
          <div class="club-count ${full ? "full" : ""}">
            目前人數：${count} / ${club.max} ${full ? "（已額滿）" : ""}
          </div>
        `;

        const btn = document.createElement("button");
        btn.textContent = full ? "已額滿" : "選擇";
        btn.disabled = full;

        btn.addEventListener("click", () => {
          if (!currentStudent) return;

          if (studentAlreadySelected(currentStudent.class, currentStudent.studentId)) {
            showMsg("clubMsg", "您已選過社團，不能再重複選填。", "danger");
            return;
          }

          students.push({
            class: currentStudent.class,
            studentId: currentStudent.studentId,
            name: currentStudent.name,
            club: club.id
          });

          clubCounts[club.id] = count + 1;
          saveStudents(students);
          saveCounts(clubCounts);

          showMsg("clubMsg", `成功選擇「${club.name}」`, "success");
          renderClubs();
        });

        item.appendChild(info);
        item.appendChild(btn);
        list.appendChild(item);
      });
    }

    document.getElementById("checkBtn").addEventListener("click", () => {
      const className = document.getElementById("classInput").value.trim();
      const studentId = document.getElementById("studentIdInput").value.trim();
      const name = document.getElementById("nameInput").value.trim();

      if (!className || !studentId || !name) {
        showMsg("studentMsg", "請輸入完整的班級、學號與姓名。", "danger");
        return;
      }

      if (studentAlreadySelected(className, studentId)) {
        showMsg("studentMsg", "你已經選過社團，不能再次選填。", "danger");
        return;
      }

      currentStudent = { class: className, studentId, name };
      hideMsg("studentMsg");
      document.getElementById("clubPanel").classList.remove("hidden");
      renderClubs();
    });

    document.getElementById("loginBtn").addEventListener("click", () => {
      const user = document.getElementById("adminUser").value.trim();
      const pass = document.getElementById("adminPass").value.trim();

      if (user === ADMIN_USER && pass === ADMIN_PASS) {
        hideMsg("adminMsg");
        document.getElementById("adminPanel").classList.remove("hidden");
        renderAdminTable();
      } else {
        showMsg("adminMsg", "帳號或密碼錯誤。", "danger");
      }
    });

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
      saveCounts(clubCounts);
      saveStudents(students);
    });
  </script>
</body>
</html>
