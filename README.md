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
