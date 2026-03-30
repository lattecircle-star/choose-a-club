<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Club Sign-Up — SJJH</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Epilogue:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root {
  --bg: #f5f2ec;
  --surface: #ffffff;
  --card: #faf8f4;
  --border: #e0dbd2;
  --ink: #1a1714;
  --muted: #8a8278;
  --accent: #c8402a;
  --accent2: #2a6cc8;
  --green: #2a8a4a;
  --yellow: #c9900a;
  --tag-bg: #ede9e1;
}
* { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: 'Epilogue', sans-serif;
  background: var(--bg);
  color: var(--ink);
  min-height: 100vh;
}

/* ── NAV ── */
nav {
  display: flex; align-items: center;
  background: var(--ink);
  padding: 0 28px;
  position: sticky; top: 0; z-index: 50;
}
.nav-brand {
  font-family: 'Syne', sans-serif; font-weight: 800;
  font-size: 1rem; color: #fff;
  padding: 18px 0; margin-right: auto; letter-spacing: 0.05em;
}
.nav-admin-btn {
  font-family: 'Syne', sans-serif; font-size: 0.75rem;
  font-weight: 700; letter-spacing: 0.08em; text-transform: uppercase;
  color: rgba(255,255,255,0.45); cursor: pointer;
  padding: 8px 14px; border: 1px solid rgba(255,255,255,0.15);
  border-radius: 6px; transition: all 0.2s; background: none;
}
.nav-admin-btn:hover { color: #fff; border-color: rgba(255,255,255,0.4); }
.nav-admin-btn.active { color: #fff; border-color: rgba(255,255,255,0.6); background: rgba(255,255,255,0.08); }

/* ── PAGES ── */
.page { display: none; }
.page.active { display: block; }

/* ══ STUDENT PAGE ══ */
.student-wrap { max-width: 760px; margin: 0 auto; padding: 48px 24px 80px; }
.page-header { margin-bottom: 36px; }
.page-header h1 {
  font-family: 'Syne', sans-serif; font-weight: 800;
  font-size: clamp(2rem, 5vw, 3rem); line-height: 1.05;
}
.page-header p { color: var(--muted); font-size: 0.93rem; font-weight: 300; margin-top: 8px; }

.name-row { display: flex; gap: 12px; margin-bottom: 32px; }
.field { display: flex; flex-direction: column; gap: 6px; flex: 1; }
.field label {
  font-size: 0.72rem; font-weight: 700; letter-spacing: 0.09em;
  text-transform: uppercase; color: var(--muted);
}
.field input {
  background: var(--surface); border: 1.5px solid var(--border);
  font-family: 'Epilogue', sans-serif; font-size: 0.95rem; color: var(--ink);
  padding: 11px 14px; border-radius: 8px; outline: none; transition: border-color 0.2s;
}
.field input:focus { border-color: var(--accent2); }
.field input::placeholder { color: var(--muted); }
.field input.error { border-color: var(--accent); animation: shake 0.3s; }
@keyframes shake { 0%,100%{transform:translateX(0)} 25%{transform:translateX(-5px)} 75%{transform:translateX(5px)} }

.section-label {
  font-family: 'Syne', sans-serif; font-size: 0.72rem; font-weight: 700;
  letter-spacing: 0.09em; text-transform: uppercase; color: var(--muted); margin-bottom: 14px;
}

.clubs-grid {
  display: grid; grid-template-columns: repeat(auto-fill, minmax(175px, 1fr));
  gap: 12px; margin-bottom: 28px;
}
.club-card {
  background: var(--surface); border: 2px solid var(--border);
  border-radius: 12px; padding: 16px; cursor: pointer;
  transition: border-color 0.18s, transform 0.15s, box-shadow 0.15s;
  position: relative; user-select: none;
}
.club-card:hover:not(.full):not(.selected) {
  border-color: var(--accent2); transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(42,108,200,0.1);
}
.club-card.selected { border-color: var(--accent2); background: #eef3fc; }
.club-card.full { opacity: 0.45; cursor: not-allowed; background: var(--card); }
.club-icon { font-size: 1.8rem; margin-bottom: 8px; display: block; }
.club-name { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 0.93rem; margin-bottom: 8px; }
.club-bar-wrap { height: 4px; background: var(--border); border-radius: 99px; overflow: hidden; margin-bottom: 5px; }
.club-bar { height: 100%; border-radius: 99px; background: var(--green); transition: width 0.4s ease; }
.club-bar.warn { background: var(--yellow); }
.club-bar.full-bar { background: var(--accent); }
.club-count { font-size: 0.75rem; color: var(--muted); }
.badge {
  position: absolute; top: 9px; right: 9px;
  font-size: 0.62rem; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase;
  padding: 2px 7px; border-radius: 99px;
}
.badge-full { background: var(--accent); color: #fff; }
.badge-sel { background: var(--accent2); color: #fff; }

.btn-submit {
  font-family: 'Syne', sans-serif; font-weight: 700;
  font-size: 0.88rem; letter-spacing: 0.07em; text-transform: uppercase;
  background: var(--ink); color: #fff; border: none;
  padding: 14px 44px; border-radius: 8px; cursor: pointer;
  transition: background 0.15s, transform 0.12s;
}
.btn-submit:hover:not(:disabled) { background: #333; transform: translateY(-1px); }
.btn-submit:disabled { background: var(--border); color: var(--muted); cursor: not-allowed; }

.done-box {
  background: #eef6f0; border: 1.5px solid #b3dfc0;
  border-radius: 12px; padding: 20px 24px; margin-bottom: 28px;
}
.done-box h3 { font-family: 'Syne', sans-serif; font-weight: 700; color: var(--green); margin-bottom: 4px; }
.done-box p { font-size: 0.88rem; color: var(--muted); font-weight: 300; }

/* ══ LOGIN MODAL ══ */
.modal-overlay {
  display: none; position: fixed; inset: 0;
  background: rgba(20,18,15,0.72); backdrop-filter: blur(6px);
  z-index: 200; align-items: center; justify-content: center;
}
.modal-overlay.open { display: flex; }
.modal {
  background: var(--surface); border: 1.5px solid var(--border);
  border-radius: 16px; padding: 36px 40px;
  width: 90%; max-width: 360px;
  animation: popIn 0.28s cubic-bezier(0.34,1.56,0.64,1);
}
@keyframes popIn { from{transform:scale(0.88);opacity:0} to{transform:scale(1);opacity:1} }
.modal h2 { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 1.5rem; margin-bottom: 4px; }
.modal .sub { color: var(--muted); font-size: 0.85rem; font-weight: 300; margin-bottom: 24px; }
.modal .field { margin-bottom: 14px; }
.login-err {
  color: var(--accent); font-size: 0.82rem; font-weight: 500;
  margin-bottom: 12px; display: none;
}
.login-err.show { display: block; }
.modal-btns { display: flex; gap: 10px; margin-top: 8px; }
.btn-primary {
  flex: 1; font-family: 'Syne', sans-serif; font-weight: 700;
  font-size: 0.85rem; letter-spacing: 0.06em; text-transform: uppercase;
  background: var(--ink); color: #fff; border: none;
  padding: 12px 20px; border-radius: 8px; cursor: pointer; transition: background 0.15s;
}
.btn-primary:hover { background: #333; }
.btn-cancel {
  font-family: 'Syne', sans-serif; font-weight: 600; font-size: 0.85rem;
  background: transparent; color: var(--muted);
  border: 1.5px solid var(--border); padding: 12px 20px;
  border-radius: 8px; cursor: pointer; transition: border-color 0.15s;
}
.btn-cancel:hover { border-color: var(--muted); }

/* ══ ADMIN PAGE ══ */
.admin-wrap { max-width: 1020px; margin: 0 auto; padding: 48px 24px 80px; }
.admin-top {
  display: flex; align-items: baseline; gap: 14px;
  margin-bottom: 32px; flex-wrap: wrap;
}
.admin-top h1 { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 2rem; }
.stat-pill {
  background: var(--tag-bg); border: 1px solid var(--border); border-radius: 99px;
  padding: 4px 14px; font-size: 0.8rem; font-weight: 600; color: var(--muted);
}
.logout-btn {
  margin-left: auto; font-family: 'Syne', sans-serif; font-weight: 700;
  font-size: 0.75rem; letter-spacing: 0.07em; text-transform: uppercase;
  background: transparent; color: var(--muted); border: 1.5px solid var(--border);
  padding: 7px 16px; border-radius: 6px; cursor: pointer; transition: all 0.15s;
}
.logout-btn:hover { border-color: var(--accent); color: var(--accent); }

.summary-grid {
  display: grid; grid-template-columns: repeat(auto-fill, minmax(155px, 1fr));
  gap: 10px; margin-bottom: 40px;
}
.sum-card {
  background: var(--surface); border: 1.5px solid var(--border);
  border-radius: 10px; padding: 14px;
}
.sum-card .s-icon { font-size: 1.4rem; }
.sum-card .s-name { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 0.82rem; margin: 6px 0 8px; }
.sum-card .s-bar-wrap { height: 6px; background: var(--border); border-radius: 99px; overflow: hidden; margin-bottom: 5px; }
.sum-card .s-bar { height: 100%; border-radius: 99px; background: var(--green); transition: width 0.5s; }
.sum-card .s-bar.warn { background: var(--yellow); }
.sum-card .s-bar.full-bar { background: var(--accent); }
.sum-card .s-count { font-size: 0.75rem; color: var(--muted); }

.controls { display: flex; gap: 10px; margin-bottom: 18px; flex-wrap: wrap; align-items: center; }
.search-box {
  flex: 1; min-width: 180px; background: var(--surface); border: 1.5px solid var(--border);
  font-family: 'Epilogue', sans-serif; font-size: 0.9rem; color: var(--ink);
  padding: 9px 14px; border-radius: 8px; outline: none; transition: border-color 0.2s;
}
.search-box:focus { border-color: var(--accent2); }
.filter-sel {
  background: var(--surface); border: 1.5px solid var(--border);
  font-family: 'Epilogue', sans-serif; font-size: 0.88rem; color: var(--ink);
  padding: 9px 14px; border-radius: 8px; outline: none; cursor: pointer;
}
.ctrl-btn {
  font-family: 'Syne', sans-serif; font-weight: 700; font-size: 0.75rem;
  letter-spacing: 0.06em; text-transform: uppercase;
  padding: 9px 18px; border-radius: 8px; cursor: pointer;
  transition: all 0.15s; white-space: nowrap; border: none;
}
.ctrl-btn.dark { background: var(--ink); color: #fff; }
.ctrl-btn.dark:hover { background: #333; }
.ctrl-btn.danger { background: transparent; color: var(--accent); border: 1.5px solid var(--accent); }
.ctrl-btn.danger:hover { background: var(--accent); color: #fff; }

.table-wrap { background: var(--surface); border: 1.5px solid var(--border); border-radius: 12px; overflow: hidden; }
table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
thead th {
  background: var(--tag-bg); font-family: 'Syne', sans-serif; font-weight: 700;
  font-size: 0.7rem; letter-spacing: 0.08em; text-transform: uppercase;
  color: var(--muted); padding: 12px 16px; text-align: left; border-bottom: 1.5px solid var(--border);
}
tbody tr { border-bottom: 1px solid var(--border); transition: background 0.12s; }
tbody tr:last-child { border-bottom: none; }
tbody tr:hover { background: var(--card); }
tbody td { padding: 11px 16px; }
.club-tag {
  display: inline-block; background: var(--tag-bg); border: 1px solid var(--border);
  border-radius: 5px; padding: 2px 8px; font-size: 0.8rem; font-weight: 500;
}
.no-results { text-align: center; color: var(--muted); font-size: 0.9rem; padding: 36px; font-style: italic; font-weight: 300; }

/* ── LOADING SCREEN ── */
#loading-screen {
  position: fixed; inset: 0; background: var(--bg);
  display: flex; align-items: center; justify-content: center;
  z-index: 999; font-family: 'Syne', sans-serif; color: var(--muted);
  font-size: 0.9rem; letter-spacing: 0.05em;
}

/* ── TOAST ── */
#toast {
  position: fixed; bottom: 28px; left: 50%;
  transform: translateX(-50%) translateY(70px);
  background: var(--ink); color: #fff;
  padding: 12px 24px; border-radius: 8px;
  font-size: 0.9rem; font-weight: 500;
  transition: transform 0.3s cubic-bezier(0.34,1.56,0.64,1);
  z-index: 999; white-space: nowrap; pointer-events: none;
}
#toast.show { transform: translateX(-50%) translateY(0); }

@media(max-width:520px){
  .name-row{flex-direction:column;}
  .clubs-grid{grid-template-columns:1fr 1fr;}
  .modal{padding:28px 20px;}
}
</style>
</head>
<body>

<div id="loading-screen">Loading…</div>

<nav>
  <div class="nav-brand">SJJH · Club Sign-Up</div>
  <button class="nav-admin-btn" id="admin-nav-btn" onclick="openAdminGate()">Admin</button>
</nav>

<!-- ══ STUDENT PAGE ══ -->
<div class="page active" id="page-student">
  <div class="student-wrap">
    <div class="page-header">
      <h1>Choose Your Club</h1>
      <p>Enter your student number and name, pick one club, and submit.<br>Each club has a maximum of 25 spots.</p>
    </div>

    <div id="done-box" class="done-box" style="display:none">
      <h3>✓ Submission received</h3>
      <p id="done-text"></p>
    </div>

    <div class="name-row">
      <div class="field">
        <label>Student Number</label>
        <input id="inp-num" type="text" placeholder="e.g. S042" maxlength="20" />
      </div>
      <div class="field">
        <label>Full Name</label>
        <input id="inp-name" type="text" placeholder="Your full name" maxlength="60" />
      </div>
    </div>

    <div class="section-label">Select a Club</div>
    <div class="clubs-grid" id="clubs-grid"></div>
    <button class="btn-submit" id="submit-btn" disabled>Submit Selection</button>
  </div>
</div>

<!-- ══ ADMIN PAGE ══ -->
<div class="page" id="page-admin">
  <div class="admin-wrap">
    <div class="admin-top">
      <h1>Admin Dashboard</h1>
      <span class="stat-pill" id="total-count">0 / 130 students</span>
      <button class="logout-btn" onclick="logout()">Log out</button>
    </div>
    <div class="summary-grid" id="summary-grid"></div>
    <div class="controls">
      <input class="search-box" type="text" id="search-input" placeholder="Search by name or number…" oninput="renderTable()" />
      <select class="filter-sel" id="filter-club" onchange="renderTable()">
        <option value="">All Clubs</option>
      </select>
      <button class="ctrl-btn dark" onclick="exportCSV()">Export CSV</button>
      <button class="ctrl-btn danger" onclick="resetAll()">Reset All</button>
    </div>
    <div class="table-wrap">
      <table>
        <thead>
          <tr>
            <th>#</th><th>Student No.</th><th>Name</th><th>Club</th><th>Submitted At</th>
          </tr>
        </thead>
        <tbody id="student-table"></tbody>
      </table>
      <div id="no-results" class="no-results" style="display:none">No matching students found.</div>
    </div>
  </div>
</div>

<!-- ══ LOGIN MODAL ══ -->
<div class="modal-overlay" id="login-modal">
  <div class="modal">
    <h2>Admin Login</h2>
    <p class="sub">Enter your administrator credentials to view submissions.</p>
    <div class="field">
      <label>Account</label>
      <input id="login-user" type="text" placeholder="Username" autocomplete="off" />
    </div>
    <div class="field">
      <label>Password</label>
      <input id="login-pass" type="password" placeholder="Password" />
    </div>
    <div class="login-err" id="login-err">Incorrect account or password.</div>
    <div class="modal-btns">
      <button class="btn-cancel" onclick="closeModal()">Cancel</button>
      <button class="btn-primary" onclick="attemptLogin()">Log In</button>
    </div>
  </div>
</div>

<div id="toast"></div>

<script>
// ══ CONFIG ══
const CLUBS = [
  { id:'dance',     name:'Dance',     icon:'💃', max:25 },
  { id:'movie',     name:'Movie',     icon:'🎬', max:25 },
  { id:'badminton', name:'Badminton', icon:'🏸', max:25 },
  { id:'makeup',    name:'Makeup',    icon:'💄', max:25 },
  { id:'tennis',    name:'Tennis',    icon:'🎾', max:25 },
  { id:'go',        name:'Go',        icon:'⬛', max:25 },
  { id:'chess',     name:'Chess',     icon:'♟️', max:25 },
  { id:'handball',  name:'Handball',  icon:'🤾', max:25 },
];
const ADMIN_USER = 'sjjh313';
const ADMIN_PASS = 'sjjh313';
const KEY = 'sjjh_club_v1';

// ══ STORAGE — shared across all users ══
async function loadSubs() {
  try {
    const r = await window.storage.get(KEY, true);
    return r ? JSON.parse(r.value) : [];
  } catch {
    try { return JSON.parse(localStorage.getItem(KEY) || '[]'); } catch { return []; }
  }
}
async function saveSubs(arr) {
  try {
    await window.storage.set(KEY, JSON.stringify(arr), true);
  } catch {
    localStorage.setItem(KEY, JSON.stringify(arr));
  }
}
function counts(subs) {
  const c = {};
  CLUBS.forEach(cl => c[cl.id] = 0);
  subs.forEach(s => { if (c[s.clubId] !== undefined) c[s.clubId]++; });
  return c;
}

// ══ TOAST ══
function toast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 3000);
}

// ══ AUTH ══
let adminLoggedIn = false;

function openAdminGate() {
  if (adminLoggedIn) { showAdmin(); return; }
  document.getElementById('login-user').value = '';
  document.getElementById('login-pass').value = '';
  document.getElementById('login-err').classList.remove('show');
  document.getElementById('login-modal').classList.add('open');
  setTimeout(() => document.getElementById('login-user').focus(), 120);
}
function closeModal() {
  document.getElementById('login-modal').classList.remove('open');
}
function attemptLogin() {
  const u = document.getElementById('login-user').value.trim();
  const p = document.getElementById('login-pass').value;
  if (u === ADMIN_USER && p === ADMIN_PASS) {
    adminLoggedIn = true;
    closeModal();
    showAdmin();
  } else {
    document.getElementById('login-err').classList.add('show');
    document.getElementById('login-pass').value = '';
    document.getElementById('login-pass').focus();
  }
}
document.getElementById('login-pass').addEventListener('keydown', e => { if (e.key === 'Enter') attemptLogin(); });
document.getElementById('login-user').addEventListener('keydown', e => { if (e.key === 'Enter') document.getElementById('login-pass').focus(); });

function logout() {
  adminLoggedIn = false;
  document.getElementById('admin-nav-btn').classList.remove('active');
  showPage('student');
  renderStudentPage();
}

// ══ NAVIGATION ══
function showPage(id) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.getElementById('page-' + id).classList.add('active');
}
async function showAdmin() {
  document.getElementById('admin-nav-btn').classList.add('active');
  showPage('admin');
  await renderAdmin();
}

// ══ STUDENT PAGE ══
let selectedClub = null;

async function renderStudentPage() {
  selectedClub = null;
  const subs = await loadSubs();
  const c = counts(subs);
  const grid = document.getElementById('clubs-grid');
  grid.innerHTML = '';

  CLUBS.forEach(club => {
    const cnt = c[club.id];
    const full = cnt >= club.max;
    const pct = Math.round((cnt / club.max) * 100);
    const barCls = pct >= 100 ? 'full-bar' : pct >= 80 ? 'warn' : '';

    const card = document.createElement('div');
    card.className = 'club-card' + (full ? ' full' : '');
    card.dataset.id = club.id;
    card.innerHTML = `
      <span class="club-icon">${club.icon}</span>
      <div class="club-name">${club.name}</div>
      <div class="club-bar-wrap"><div class="club-bar ${barCls}" style="width:${pct}%"></div></div>
      <div class="club-count">${cnt} / ${club.max}</div>
      ${full ? '<span class="badge badge-full">Full</span>' : ''}
    `;
    if (!full) card.addEventListener('click', () => pickClub(club.id));
    grid.appendChild(card);
  });
  updateBtn();
}

function pickClub(id) {
  selectedClub = id;
  document.querySelectorAll('.club-card:not(.full)').forEach(card => {
    const on = card.dataset.id === id;
    card.classList.toggle('selected', on);
    const old = card.querySelector('.badge-sel');
    if (old) old.remove();
    if (on) {
      const b = document.createElement('span');
      b.className = 'badge badge-sel'; b.textContent = '✓';
      card.appendChild(b);
    }
  });
  updateBtn();
}

function updateBtn() {
  const name = document.getElementById('inp-name').value.trim();
  const num = document.getElementById('inp-num').value.trim();
  document.getElementById('submit-btn').disabled = !(name && num && selectedClub);
}
document.getElementById('inp-name').addEventListener('input', updateBtn);
document.getElementById('inp-num').addEventListener('input', updateBtn);

document.getElementById('submit-btn').addEventListener('click', async () => {
  const name = document.getElementById('inp-name').value.trim();
  const num = document.getElementById('inp-num').value.trim();
  if (!name || !num || !selectedClub) return;

  const subs = await loadSubs();
  if (subs.find(s => s.studentNum === num)) {
    const el = document.getElementById('inp-num');
    el.classList.add('error');
    toast('⚠ This student number has already submitted.');
    setTimeout(() => el.classList.remove('error'), 800);
    return;
  }

  const c = counts(subs);
  const club = CLUBS.find(cl => cl.id === selectedClub);
  if (c[selectedClub] >= club.max) {
    toast('⚠ That club just filled up. Please choose another.');
    await renderStudentPage();
    return;
  }

  subs.push({
    studentNum: num, name,
    clubId: selectedClub,
    time: new Date().toLocaleString('zh-TW', { hour12: false })
  });
  await saveSubs(subs);

  document.getElementById('done-text').textContent =
    `${name} (${num}) has been registered for ${club.icon} ${club.name}.`;
  document.getElementById('done-box').style.display = 'block';
  toast(`✓ Joined ${club.icon} ${club.name}!`);

  document.getElementById('inp-name').value = '';
  document.getElementById('inp-num').value = '';
  selectedClub = null;
  await renderStudentPage();
});

// ══ ADMIN PAGE ══
async function renderAdmin() {
  const subs = await loadSubs();
  const c = counts(subs);
  document.getElementById('total-count').textContent = `${subs.length} / 130 students`;

  // Summary
  const sg = document.getElementById('summary-grid');
  sg.innerHTML = '';
  CLUBS.forEach(club => {
    const cnt = c[club.id];
    const pct = Math.round((cnt / club.max) * 100);
    const barCls = pct >= 100 ? 'full-bar' : pct >= 80 ? 'warn' : '';
    const card = document.createElement('div');
    card.className = 'sum-card';
    card.innerHTML = `
      <div class="s-icon">${club.icon}</div>
      <div class="s-name">${club.name}</div>
      <div class="s-bar-wrap"><div class="s-bar ${barCls}" style="width:${pct}%"></div></div>
      <div class="s-count">${cnt} / ${club.max}</div>
    `;
    sg.appendChild(card);
  });

  // Filter dropdown
  const sel = document.getElementById('filter-club');
  sel.innerHTML = '<option value="">All Clubs</option>';
  CLUBS.forEach(cl => {
    const o = document.createElement('option');
    o.value = cl.id; o.textContent = `${cl.icon} ${cl.name}`;
    sel.appendChild(o);
  });

  renderTable(subs);
}

async function renderTable(subsArg) {
  const subs = subsArg !== undefined ? subsArg : await loadSubs();
  const search = document.getElementById('search-input').value.trim().toLowerCase();
  const fc = document.getElementById('filter-club').value;

  const filtered = subs.filter(s =>
    (!search || s.name.toLowerCase().includes(search) || s.studentNum.toLowerCase().includes(search)) &&
    (!fc || s.clubId === fc)
  );

  const tbody = document.getElementById('student-table');
  tbody.innerHTML = '';
  document.getElementById('no-results').style.display = filtered.length === 0 ? 'block' : 'none';

  filtered.forEach((s, i) => {
    const club = CLUBS.find(cl => cl.id === s.clubId);
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td style="color:var(--muted);font-size:0.78rem">${i + 1}</td>
      <td style="font-family:'Syne',sans-serif;font-weight:700;font-size:0.85rem">${s.studentNum}</td>
      <td>${s.name}</td>
      <td><span class="club-tag">${club?.icon} ${club?.name || s.clubId}</span></td>
      <td style="color:var(--muted);font-size:0.8rem">${s.time}</td>
    `;
    tbody.appendChild(tr);
  });
}

async function exportCSV() {
  const subs = await loadSubs();
  if (!subs.length) { toast('No data to export.'); return; }
  const rows = [['Student Number','Name','Club','Submitted At']];
  subs.forEach(s => {
    const club = CLUBS.find(cl => cl.id === s.clubId);
    rows.push([s.studentNum, s.name, club?.name || s.clubId, s.time]);
  });
  const csv = rows.map(r => r.map(v => `"${v}"`).join(',')).join('\n');
  const a = document.createElement('a');
  a.href = URL.createObjectURL(new Blob(['\uFEFF' + csv], {type:'text/csv;charset=utf-8'}));
  a.download = 'sjjh_club_selections.csv';
  a.click();
  toast('CSV exported!');
}

async function resetAll() {
  if (!confirm('Delete ALL submissions? This cannot be undone.')) return;
  await saveSubs([]);
  toast('All submissions cleared.');
  await renderAdmin();
}

// ══ INIT ══
(async () => {
  await renderStudentPage();
  document.getElementById('loading-screen').style.display = 'none';
})();
</script>
</body>
</html>
