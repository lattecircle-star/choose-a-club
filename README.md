<!DOCTYPE html>
<html lang="zh-TW">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>學校社團選填系統 </title>
  <style>
    body {
      font-family: "Microsoft JhengHei", "微軟正黑體", sans-serif;
      background: #f4f7fb;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    .container {
      max-width: 980px;
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
      margin-right: 8px;
      margin-top: 6px;
    }
    button:hover {
      background: #1f68d6;
    }
    button:disabled {
      background: #999;
      cursor: not-allowed;
    }
    .danger-btn {
      background: #d93025;
    }
    .danger-btn:hover {
      background: #b3261e;
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
    .note {
      font-size: 14px;
      color: #666;
      margin-top: 8px;
    }
    .toolbar {
      margin-bottom: 10px;
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>學校社團選填系統（最終折衷版）</h1>

    <div id="studentFormPanel" class="panel">
      <h2>學生資料輸入</h2>
      <div class="form-group">
        <label for="classInput">班級</label>
        <input type="text" id="classInput" placeholder="請輸入班級，例如 801" />
      </div>
      <div class="form-group">
        <label for="studentIdInput">座號</label>
        <input type="text" id="studentIdInput" placeholder="請輸入座號，例如 15" />
      </div>
      <div class="form-group">
        <label for="nameInput">姓名</label>
        <input type="text" id="nameInput" placeholder="請輸入姓名" />
      </div>
      <button id="checkBtn">下一步 選社團</button>
      <div class="note">同一位學生只能選一次，所有裝置會即時同步。</div>
      <div id="studentMsg" class="alert hidden"></div>
    </div>

    <div id="clubPanel" class="panel hidden">
      <h2>請選擇社團</h2>
      <p>各裝置會即時同步更新社團人數與額滿狀態。</p>
      <div id="clubList" class="club-list"></div>
      <div id="clubMsg" class="alert hidden"></div>
    </div>

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
      <button id="logoutBtn" class="hidden">登出</button>
      <div id="adminMsg" class="alert hidden"></div>
    </div>

    <div id="adminPanel" class="panel hidden">
      <h2>管理者介面</h2>
      <div class="toolbar">
        <button id="deleteAllBtn" class="danger-btn">刪除全部選社資料</button>
      </div>
      <div id="tableArea"></div>
    </div>
  </div>

  <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-database-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth-compat.js"></script>

  <script>
    // ===== 這裡改成你的 Firebase 設定 =====
   const firebaseConfig = {
  apiKey: "AIzaSyD0YuLv3GJIhr9rXUOFjjoyX58ulQ9V-kM",
  authDomain: "club-selection-system.firebaseapp.com",
  projectId: "club-selection-system",
  storageBucket: "club-selection-system.firebasestorage.app",
  messagingSenderId: "64472694372",
  appId: "1:64472694372:web:9f31c284a2a383af522971",
  measurementId: "G-QW834C9LBZ"
};

    // ===== 這裡保留你原本想要的入口帳密 =====
    const DISPLAY_ADMIN_USER = "admin";
    const DISPLAY_ADMIN_PASS = "sjjh313";

    // ===== 這裡改成真正 Firebase 管理者帳號 =====
    const FIREBASE_ADMIN_EMAIL = "lattecircle@gmail.com";
    const FIREBASE_ADMIN_PASSWORD = "lily791217";

    firebase.initializeApp(firebaseConfig);

    const db = firebase.database();
    const auth = firebase.auth();
    const studentsRef = db.ref("students");

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

    let currentStudent = null;
    let students = [];
    let clubCounts = [];

    function showMsg(id, text, type) {
      const el = document.getElementById(id);
      el.textContent = text;
      el.className = `alert alert-${type}`;
      el.classList.remove("hidden");
    }

    function hideMsg(id) {
      document.getElementById(id).classList.add("hidden");
    }

    function sanitizeKey(text) {
      return String(text).trim().replace(/[.#$\[\]/]/g, "_");
    }

    function makeStudentKey(className, studentId) {
      return `${sanitizeKey(className)}_${sanitizeKey(studentId)}`;
    }

    function studentAlreadySelected(className, studentId) {
      return students.some(s => s.class === className && s.studentId === studentId);
    }

    function rebuildCounts() {
      clubCounts = {};
      CLUBS.forEach(c => clubCounts[c.id] = 0);
      students.forEach(s => {
        if (clubCounts[s.club] !== undefined) {
          clubCounts[s.club]++;
        }
      });
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
        info.innerHTML = `
          <div class="club-name">${club.name}</div>
          <div class="club-count ${full ? "club-full" : ""}">
            目前人數：${count} / ${club.max} ${full ? "（已額滿）" : ""}
          </div>
        `;

        const btn = document.createElement("button");
        btn.textContent = full ? "已額滿" : "選擇";
        btn.disabled = full;

        btn.addEventListener("click", async () => {
          if (!currentStudent) return;

          if (studentAlreadySelected(currentStudent.class, currentStudent.studentId)) {
            showMsg("clubMsg", "你已經選過社團，不能再重複選填。", "danger");
            return;
          }

          if (full) {
            showMsg("clubMsg", `「${club.name}」已額滿，請選其他社團。`, "danger");
            return;
          }

          const studentKey = makeStudentKey(currentStudent.class, currentStudent.studentId);
          const newStudent = {
            class: currentStudent.class,
            studentId: currentStudent.studentId,
            name: currentStudent.name,
            club: club.id,
            timestamp: Date.now()
          };

          try {
            await studentsRef.child(studentKey).set(newStudent);
            alert(`你已成功選擇「${club.name}」社團`);
            showMsg("clubMsg", `成功選擇「${club.name}」`, "success");
          } catch (error) {
            console.error(error);
            showMsg("clubMsg", "寫入失敗，請確認 Firebase 規則是否正確。", "danger");
          }
        });

        item.appendChild(info);
        item.appendChild(btn);
        list.appendChild(item);
      });
    }

    function renderAdminTable() {
      const area = document.getElementById("tableArea");

      if (students.length === 0) {
        area.innerHTML = "<p>目前沒有學生選社資料。</p>";
        return;
      }

      const sortedStudents = [...students].sort((a, b) => {
        if (a.class !== b.class) return String(a.class).localeCompare(String(b.class), "zh-Hant");
        return String(a.studentId).localeCompare(String(b.studentId), "zh-Hant");
      });

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
          ${sortedStudents.map(s => {
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

    async function verifyFirebaseAdmin() {
      const user = auth.currentUser;
      if (!user) return false;
      const tokenResult = await user.getIdTokenResult(true);
      return tokenResult.claims.admin === true;
    }

    studentsRef.on("value", snapshot => {
      students = [];
      snapshot.forEach(child => {
        const data = child.val();
        if (data) students.push(data);
      });

      rebuildCounts();
      renderClubs();

      if (!document.getElementById("adminPanel").classList.contains("hidden")) {
        renderAdminTable();
      }
    });

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
    });

    document.getElementById("loginBtn").addEventListener("click", async () => {
      const inputUser = document.getElementById("adminUser").value.trim();
      const inputPass = document.getElementById("adminPass").value.trim();

      if (inputUser !== DISPLAY_ADMIN_USER || inputPass !== DISPLAY_ADMIN_PASS) {
        showMsg("adminMsg", "帳號或密碼錯誤。", "danger");
        return;
      }

      try {
        await auth.signInWithEmailAndPassword(FIREBASE_ADMIN_EMAIL, FIREBASE_ADMIN_PASSWORD);
        const isAdmin = await verifyFirebaseAdmin();

        if (!isAdmin) {
          showMsg("adminMsg", "Firebase 管理者帳號沒有 admin 權限。", "danger");
          await auth.signOut();
          return;
        }

        hideMsg("adminMsg");
        document.getElementById("adminPanel").classList.remove("hidden");
        document.getElementById("logoutBtn").classList.remove("hidden");
        renderAdminTable();
      } catch (error) {
        console.error(error);
        showMsg("adminMsg", "Firebase 管理者登入失敗，請檢查 Email、密碼或 admin claim。", "danger");
      }
    });

    document.getElementById("logoutBtn").addEventListener("click", async () => {
      try {
        await auth.signOut();
      } catch (e) {
        console.error(e);
      }
      document.getElementById("adminPanel").classList.add("hidden");
      document.getElementById("logoutBtn").classList.add("hidden");
      showMsg("adminMsg", "已登出。", "success");
    });

    document.getElementById("deleteAllBtn").addEventListener("click", async () => {
      const ok = confirm("確定要刪除全部選社資料嗎？此動作無法復原。");
      if (!ok) return;

      try {
        const isAdmin = await verifyFirebaseAdmin();
        if (!isAdmin) {
          showMsg("adminMsg", "你沒有 admin 權限，無法刪除資料。", "danger");
          return;
        }

        await studentsRef.remove();
        alert("已刪除全部選社資料");
        showMsg("adminMsg", "全部資料已刪除。", "success");
      } catch (error) {
        console.error(error);
        showMsg("adminMsg", "刪除失敗，請確認 Firebase 規則與 admin claim。", "danger");
      }
    });

    auth.onAuthStateChanged(async (user) => {
      if (!user) {
        document.getElementById("adminPanel").classList.add("hidden");
        document.getElementById("logoutBtn").classList.add("hidden");
        return;
      }

      try {
        const isAdmin = await verifyFirebaseAdmin();
        if (isAdmin) {
          document.getElementById("adminPanel").classList.remove("hidden");
          document.getElementById("logoutBtn").classList.remove("hidden");
          renderAdminTable();
        }
      } catch (error) {
        console.error(error);
      }
    });

    rebuildCounts();
    renderClubs();
  </script>
</body>
</html>
