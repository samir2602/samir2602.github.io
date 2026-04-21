<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WorkLog — Time Tracker</title>
<meta name="theme-color" content="#0e0f11">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:ital,wght@0,300;0,400;0,500;1,300&family=DM+Sans:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}

:root {
  --bg: #0e0f11;
  --surface: #16181c;
  --surface2: #1e2026;
  --border: #2a2d35;
  --border2: #353840;
  --text: #e8eaf0;
  --muted: #6b7280;
  --accent: #6ee7b7;
  --accent2: #34d399;
  --over: #6ee7b7;
  --under: #f87171;
  --warn: #fbbf24;
  --mono: 'DM Mono', monospace;
  --sans: 'DM Sans', sans-serif;
  --radius: 12px;
  --std-day: 480; /* 8 hours standard */
}

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--sans);
  min-height: 100vh;
  font-size: 14px;
}

/* ── Layout ── */
.app { max-width: 1100px; margin: 0 auto; padding: 2rem 1.5rem 4rem; }

/* ── Header ── */
header {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
  margin-bottom: 2.5rem;
  padding-bottom: 1.5rem;
  border-bottom: 1px solid var(--border);
  flex-wrap: wrap;
  gap: 1rem;
}
.logo { font-family: var(--mono); font-size: 1.4rem; font-weight: 500; letter-spacing: -0.5px; }
.logo span { color: var(--accent); }
.month-nav { display: flex; align-items: center; gap: 0.75rem; }
.month-nav button {
  background: var(--surface2);
  border: 1px solid var(--border);
  color: var(--text);
  width: 32px; height: 32px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
  display: flex; align-items: center; justify-content: center;
  transition: border-color .15s, background .15s;
}
.month-nav button:hover { border-color: var(--accent); background: var(--surface); }
#month-label { font-family: var(--mono); font-size: 1rem; font-weight: 500; min-width: 120px; text-align: center; }

/* ── Stats bar ── */
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(140px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}
.stat {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1rem 1.25rem;
  position: relative;
  overflow: hidden;
}
.stat::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 2px;
  background: var(--accent);
  opacity: 0.6;
}
.stat.under::before { background: var(--under); }
.stat.warn::before  { background: var(--warn); }
.stat-label { font-size: 11px; text-transform: uppercase; letter-spacing: .08em; color: var(--muted); margin-bottom: 6px; }
.stat-value { font-family: var(--mono); font-size: 1.5rem; font-weight: 500; line-height: 1; }
.stat-value.neg { color: var(--under); }
.stat-value.pos { color: var(--accent); }
.stat-sub { font-size: 11px; color: var(--muted); margin-top: 4px; font-family: var(--mono); }

/* ── Salary input ── */
.salary-bar {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1rem 1.25rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}
.salary-bar label { font-size: 12px; text-transform: uppercase; letter-spacing: .06em; color: var(--muted); white-space: nowrap; }
.salary-input-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
  background: var(--bg);
  border: 1px solid var(--border2);
  border-radius: 8px;
  padding: 6px 12px;
}
.salary-input-wrap span { font-family: var(--mono); color: var(--accent); font-size: 1rem; }
.salary-input-wrap input {
  background: transparent;
  border: none;
  outline: none;
  color: var(--text);
  font-family: var(--mono);
  font-size: 1rem;
  width: 100px;
}
.std-wrap {
  display: flex;
  align-items: center;
  gap: 6px;
  margin-left: auto;
}
.std-wrap label { font-size: 12px; color: var(--muted); white-space: nowrap; }
.std-wrap input {
  background: var(--bg);
  border: 1px solid var(--border2);
  border-radius: 8px;
  padding: 6px 10px;
  color: var(--text);
  font-family: var(--mono);
  font-size: 0.9rem;
  width: 70px;
  outline: none;
  text-align: center;
}
.std-wrap input:focus { border-color: var(--accent); }

/* ── Add entry panel ── */
.add-panel {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.25rem;
  margin-bottom: 2rem;
}
.add-panel h3 {
  font-size: 11px;
  text-transform: uppercase;
  letter-spacing: .1em;
  color: var(--muted);
  margin-bottom: 1rem;
}
.fields {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(130px, 1fr));
  gap: 0.75rem;
  align-items: end;
}
.field label {
  display: block;
  font-size: 11px;
  color: var(--muted);
  text-transform: uppercase;
  letter-spacing: .06em;
  margin-bottom: 5px;
}
.field input {
  width: 100%;
  background: var(--bg);
  border: 1px solid var(--border2);
  border-radius: 8px;
  padding: 8px 10px;
  color: var(--text);
  font-family: var(--mono);
  font-size: 0.9rem;
  outline: none;
  transition: border-color .15s;
}
.field input:focus { border-color: var(--accent); }
.field input[type=date]::-webkit-calendar-picker-indicator { filter: invert(0.5); cursor: pointer; }

.btn-add {
  background: var(--accent);
  color: #0e0f11;
  border: none;
  border-radius: 8px;
  padding: 8px 20px;
  font-family: var(--sans);
  font-size: 13px;
  font-weight: 600;
  cursor: pointer;
  white-space: nowrap;
  align-self: end;
  transition: background .15s, transform .1s;
  height: 38px;
}
.btn-add:hover { background: var(--accent2); }
.btn-add:active { transform: scale(.97); }

/* ── Table ── */
.table-wrap { overflow-x: auto; }
table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
}
thead th {
  text-align: left;
  padding: 8px 12px;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: .1em;
  color: var(--muted);
  border-bottom: 1px solid var(--border);
  white-space: nowrap;
  font-family: var(--mono);
}
tbody tr {
  border-bottom: 1px solid var(--border);
  transition: background .1s;
}
tbody tr:hover { background: var(--surface2); }
tbody tr.weekend { background: rgba(255,255,255,.015); }
tbody tr.sunday  { background: rgba(248,113,113,.06); }
tbody tr.sunday .td-day { color: var(--under); font-weight: 500; }
td {
  padding: 10px 12px;
  vertical-align: middle;
  white-space: nowrap;
}
.td-date { font-family: var(--mono); font-size: 12px; }
.td-day  { color: var(--muted); font-size: 12px; }
.td-time { font-family: var(--mono); font-size: 13px; }
.td-mins { font-family: var(--mono); }
.td-hrs  { font-family: var(--mono); }
.badge {
  display: inline-block;
  padding: 2px 8px;
  border-radius: 20px;
  font-family: var(--mono);
  font-size: 11px;
  font-weight: 500;
}
.badge.over  { background: rgba(110,231,183,.12); color: var(--over);  border: 1px solid rgba(110,231,183,.25); }
.badge.under { background: rgba(248,113,113,.12); color: var(--under); border: 1px solid rgba(248,113,113,.25); }
.badge.exact { background: rgba(251,191,36,.12);  color: var(--warn);  border: 1px solid rgba(251,191,36,.25); }
.td-pay { font-family: var(--mono); font-weight: 500; color: var(--accent); }
.btn-del {
  background: none;
  border: none;
  color: var(--muted);
  cursor: pointer;
  font-size: 1rem;
  padding: 2px 6px;
  border-radius: 4px;
  transition: color .15s, background .15s;
}
.btn-del:hover { color: var(--under); background: rgba(248,113,113,.1); }

.empty-state {
  text-align: center;
  padding: 3rem;
  color: var(--muted);
  font-size: 13px;
}
.empty-state .icon { font-size: 2rem; margin-bottom: 0.5rem; opacity: .4; }

/* ── Footer totals ── */
tfoot td {
  padding: 10px 12px;
  border-top: 1px solid var(--border2);
  font-family: var(--mono);
  font-size: 12px;
  font-weight: 500;
  color: var(--text);
}
tfoot .label { color: var(--muted); font-size: 10px; text-transform: uppercase; letter-spacing: .06em; }

.field select {
  width: 100%;
  background: var(--bg);
  border: 1px solid var(--border2);
  border-radius: 8px;
  padding: 8px 10px;
  color: var(--text);
  font-family: var(--sans);
  font-size: 0.9rem;
  outline: none;
  transition: border-color .15s;
  cursor: pointer;
}
.field select:focus { border-color: var(--accent); }
.field select option { background: var(--surface); }

tbody tr.leave-paid   { background: rgba(110,231,183,.05); }
tbody tr.leave-unpaid { background: rgba(248,113,113,.06); }
tbody tr.leave-half   { background: rgba(251,191,36,.05); }
tbody tr.leave-sick   { background: rgba(167,139,250,.05); }

.badge.paid   { background: rgba(110,231,183,.15); color: var(--over);  border: 1px solid rgba(110,231,183,.3); }
.badge.unpaid { background: rgba(248,113,113,.15); color: var(--under); border: 1px solid rgba(248,113,113,.3); }
.badge.half   { background: rgba(251,191,36,.15);  color: var(--warn);  border: 1px solid rgba(251,191,36,.3); }
.badge.sick   { background: rgba(167,139,250,.15); color: #a78bfa;      border: 1px solid rgba(167,139,250,.3); }

/* ── Pay summary boxes ── */
.pay-box {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.25rem 1.5rem;
  position: relative;
  overflow: hidden;
}
.pay-box::before {
  content: '';
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 3px;
  background: var(--border2);
}
.pay-box.accent::before { background: var(--accent); }
.pay-box-label { font-size: 11px; text-transform: uppercase; letter-spacing: .08em; color: var(--muted); margin-bottom: 8px; }
.pay-box-value { font-family: var(--mono); font-size: 2rem; font-weight: 500; line-height: 1; color: var(--text); }
.pay-box.accent .pay-box-value { color: var(--accent); }
.pay-box-value.neg { color: var(--under); }
.pay-box-value.pos { color: var(--over); }
.pay-box-sub { font-size: 11px; color: var(--muted); margin-top: 6px; font-family: var(--mono); }

/* ── Export ── */
.export-bar {
  display: flex;
  gap: 0.75rem;
  margin-top: 1.5rem;
  justify-content: flex-end;
  flex-wrap: wrap;
}
.btn-export {
  background: var(--surface);
  border: 1px solid var(--border);
  color: var(--muted);
  border-radius: 8px;
  padding: 7px 16px;
  font-size: 12px;
  font-family: var(--sans);
  cursor: pointer;
  transition: border-color .15s, color .15s;
}
.btn-export:hover { border-color: var(--accent); color: var(--accent); }

/* ── Toast ── */
.toast {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  background: var(--accent);
  color: #0e0f11;
  padding: 10px 18px;
  border-radius: 8px;
  font-size: 13px;
  font-weight: 600;
  transform: translateY(80px);
  opacity: 0;
  transition: all .25s cubic-bezier(.34,1.56,.64,1);
  z-index: 99;
  pointer-events: none;
}
.toast.show { transform: translateY(0); opacity: 1; }

@media(max-width:600px){
  .fields { grid-template-columns: 1fr 1fr; }
  .stats  { grid-template-columns: 1fr 1fr; }
}
</style>
</head>
<body>
<div class="app">

  <!-- Header -->
  <header>
    <div class="logo">Work<span>Log</span></div>
    <div class="month-nav">
      <button onclick="changeMonth(-1)">&#8249;</button>
      <div id="month-label"></div>
      <button onclick="changeMonth(1)">&#8250;</button>
    </div>
  </header>

  <!-- Salary & Settings bar -->
  <div class="salary-bar">
    <label>Monthly Salary</label>
    <div class="salary-input-wrap">
      <span>₹</span>
      <input type="number" id="salary-input" placeholder="e.g. 30000" step="1" oninput="saveSalary()">
    </div>
    <div class="std-wrap">
      <label>Std hours/day</label>
      <input type="number" id="std-input" value="8" min="1" max="24" step="0.5" oninput="saveStd()">
    </div>
    <div class="rate-display" id="rate-display" style="display:none">
      <span style="font-size:11px;text-transform:uppercase;letter-spacing:.06em;color:var(--muted);">Auto Hourly Rate</span>
      <span style="font-family:var(--mono);font-size:1.1rem;font-weight:500;color:var(--accent);margin-left:8px;" id="rate-display-val">₹0.00/hr</span>
      <span style="font-size:11px;color:var(--muted);margin-left:6px;" id="rate-display-days"></span>
    </div>
    <div style="margin-left:auto;display:flex;align-items:center;gap:6px;font-size:11px;color:var(--muted);">
      <span style="display:inline-block;width:10px;height:10px;background:rgba(248,113,113,.25);border-radius:2px;border:1px solid rgba(248,113,113,.4);"></span>
      Sunday = Holiday
    </div>
  </div>

  <!-- Stats -->
  <div class="stats">
    <div class="stat" id="stat-days">
      <div class="stat-label">Days Logged</div>
      <div class="stat-value" id="s-days">0</div>
      <div class="stat-sub">this month</div>
    </div>
    <div class="stat" id="stat-hrs">
      <div class="stat-label">Total Hours</div>
      <div class="stat-value" id="s-hrs">0h 0m</div>
      <div class="stat-sub" id="s-hrs-sub">0 minutes</div>
    </div>
    <div class="stat" id="stat-balance">
      <div class="stat-label">Balance</div>
      <div class="stat-value" id="s-balance">0m</div>
      <div class="stat-sub" id="s-balance-sub">over / under</div>
    </div>
    <div class="stat" id="stat-pay">
      <div class="stat-label">Est. Pay</div>
      <div class="stat-value" id="s-pay">₹0.00</div>
      <div class="stat-sub" id="s-pay-sub">based on hours</div>
    </div>
    <div class="stat">
      <div class="stat-label">Overtime days</div>
      <div class="stat-value pos" id="s-over">0</div>
      <div class="stat-sub">days over target</div>
    </div>
    <div class="stat under">
      <div class="stat-label">Undertime days</div>
      <div class="stat-value neg" id="s-under">0</div>
      <div class="stat-sub">days under target</div>
    </div>
    <div class="stat">
      <div class="stat-label">Paid Leaves</div>
      <div class="stat-value pos" id="s-paid-leave">0</div>
      <div class="stat-sub">full salary days</div>
    </div>
    <div class="stat under">
      <div class="stat-label">Unpaid Leaves</div>
      <div class="stat-value neg" id="s-unpaid-leave">0</div>
      <div class="stat-sub" id="s-unpaid-deduct">₹0 deducted</div>
    </div>
  </div>

  <!-- Add Entry -->
  <div class="add-panel">
    <h3 id="panel-title">&#43; Add / Edit Entry</h3>
    <div class="fields">
      <div class="field">
        <label>Date</label>
        <input type="date" id="f-date" onchange="onDateChange(this.value)">
      </div>
      <div class="field">
        <label>Leave Type</label>
        <select id="f-leave" onchange="onLeaveChange(this.value)">
          <option value="">— Working Day —</option>
          <option value="paid">✅ Paid Leave</option>
          <option value="unpaid">❌ Unpaid Leave</option>
          <option value="half">🌗 Half Day Leave</option>
          <option value="sick">🤒 Sick Leave</option>
        </select>
      </div>
      <div class="field" id="field-in">
        <label>Check In</label>
        <input type="time" id="f-in" placeholder="09:00">
      </div>
      <div class="field" id="field-out">
        <label>Check Out</label>
        <input type="time" id="f-out" placeholder="17:00">
      </div>
      <div class="field" id="field-b1">
        <label>Break 1 (hh:mm)</label>
        <input type="text" id="f-b1" placeholder="00:30" maxlength="5">
      </div>
      <div class="field" id="field-b2">
        <label>Extra Break (hh:mm)</label>
        <input type="text" id="f-b2" placeholder="00:00" maxlength="5">
      </div>
      <button class="btn-add" onclick="addEntry()">Save Entry</button>
    </div>
  </div>

  <!-- Table -->
  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Date</th>
          <th>Day</th>
          <th>In</th>
          <th>Out</th>
          <th>Break 1</th>
          <th>Break 2</th>
          <th>Minutes</th>
          <th>Hours</th>
          <th>+/− vs target</th>
          <th>Daily Pay</th>
          <th></th>
        </tr>
      </thead>
      <tbody id="tbody">
        <tr><td colspan="11" class="empty-state"><div class="icon">⏱</div><div>No entries yet — add your first day above</div></td></tr>
      </tbody>
      <tfoot id="tfoot" style="display:none">
        <tr>
          <td colspan="6" class="label">Month Total</td>
          <td id="f-mins"></td>
          <td id="f-hrs-total"></td>
          <td id="f-bal"></td>
          <td id="f-pay-total"></td>
          <td></td>
        </tr>
      </tfoot>
    </table>
  </div>

  <!-- Monthly Pay Summary -->
  <div id="pay-summary" style="display:none; margin-top:1.5rem;">
    <div style="display:grid; grid-template-columns:repeat(auto-fit,minmax(180px,1fr)); gap:1rem;">
      <div class="pay-box">
        <div class="pay-box-label">Total Minutes Worked</div>
        <div class="pay-box-value" id="ps-mins">0</div>
        <div class="pay-box-sub" id="ps-mins-sub">0h 0m</div>
      </div>
      <div class="pay-box accent">
        <div class="pay-box-label">Monthly Pay Earned</div>
        <div class="pay-box-value" id="ps-pay">₹0.00</div>
        <div class="pay-box-sub" id="ps-pay-sub">based on hours worked</div>
      </div>
      <div class="pay-box" id="ps-bal-box">
        <div class="pay-box-label">Monthly Balance</div>
        <div class="pay-box-value" id="ps-bal">+0h 0m</div>
        <div class="pay-box-sub" id="ps-bal-sub">vs target hours</div>
      </div>
      <div class="pay-box">
        <div class="pay-box-label">Avg Daily Pay</div>
        <div class="pay-box-value" id="ps-avg">₹0.00</div>
        <div class="pay-box-sub" id="ps-avg-sub">per working day</div>
      </div>
      <div class="pay-box" id="ps-leave-box" style="display:none">
        <div class="pay-box-label">Leave Deductions</div>
        <div class="pay-box-value neg" id="ps-leave-deduct">−₹0.00</div>
        <div class="pay-box-sub" id="ps-leave-sub">unpaid leave days</div>
      </div>
      <div class="pay-box accent" id="ps-net-box" style="display:none">
        <div class="pay-box-label">Net Monthly Pay</div>
        <div class="pay-box-value" id="ps-net-pay">₹0.00</div>
        <div class="pay-box-sub" id="ps-net-sub">after leave deductions</div>
      </div>
    </div>
  </div>

  <div id="no-rate-hint" style="display:none; margin-top:1rem; padding:0.75rem 1rem; background:rgba(251,191,36,.08); border:1px solid rgba(251,191,36,.2); border-radius:8px; font-size:12px; color:var(--warn);">
    ⚠️ Enter your monthly salary or hourly rate above to see pay calculations
  </div>

  <div class="export-bar">
    <button class="btn-export" onclick="exportCSV()">Export CSV</button>
    <button class="btn-export" onclick="clearMonth()">Clear Month</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ── State ──────────────────────────────────────────────────────────────────
let currentYear  = new Date().getFullYear();
let currentMonth = new Date().getMonth(); // 0-indexed

function storageKey() { return `worklog_${currentYear}_${currentMonth}`; }
function settingsKey() { return `worklog_settings`; }

function loadEntries() {
  try { return JSON.parse(localStorage.getItem(storageKey())) || []; } catch { return []; }
}
function saveEntries(arr) { localStorage.setItem(storageKey(), JSON.stringify(arr)); }

function loadSettings() {
  try { return JSON.parse(localStorage.getItem(settingsKey())) || {}; } catch { return {}; }
}
function saveSettings(obj) { localStorage.setItem(settingsKey(), JSON.stringify(obj)); }

// ── Helpers ────────────────────────────────────────────────────────────────
const DAYS = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
const MONTHS = ['January','February','March','April','May','June','July','August','September','October','November','December'];

function parseTime(str) {
  if (!str) return null;
  const [h, m] = str.split(':').map(Number);
  return h * 60 + m;
}

function parseBreak(str) {
  if (!str || str === '00:00') return 0;
  const clean = str.replace(/[^0-9:]/g,'');
  if (!clean.includes(':')) return 0;
  const [h, m] = clean.split(':').map(Number);
  return (h || 0) * 60 + (m || 0);
}

function fmtMins(m) {
  const sign = m < 0 ? '-' : '';
  const abs  = Math.abs(m);
  return sign + Math.floor(abs/60) + 'h ' + String(abs%60).padStart(2,'0') + 'm';
}

function fmtBalance(m) {
  const sign = m >= 0 ? '+' : '−';
  const abs  = Math.abs(m);
  return sign + Math.floor(abs/60) + 'h ' + String(abs%60).padStart(2,'0') + 'm';
}

function getWorkingDaysInMonth(year, month) {
  // Count all non-Sunday days in the month
  const days = new Date(year, month + 1, 0).getDate();
  let count = 0;
  for (let d = 1; d <= days; d++) {
    const day = new Date(year, month, d).getDay();
    if (day !== 0) count++; // 0 = Sunday
  }
  return count;
}

function getHourlyRate() {
  const s = loadSettings();
  const salary = parseFloat(s.salary);
  const std    = parseFloat(s.std || 8);
  if (salary > 0 && std > 0) {
    const workDays = getWorkingDaysInMonth(currentYear, currentMonth);
    return salary / (workDays * std);
  }
  return 0;
}

function updateRateDisplay() {
  const s       = loadSettings();
  const salary  = parseFloat(s.salary);
  const std     = parseFloat(s.std || 8);
  const display = document.getElementById('rate-display');
  if (salary > 0 && std > 0) {
    const workDays = getWorkingDaysInMonth(currentYear, currentMonth);
    const rate     = salary / (workDays * std);
    display.style.display = '';
    document.getElementById('rate-display-val').textContent  = '₹' + rate.toFixed(2) + '/hr';
    document.getElementById('rate-display-days').textContent = '(' + workDays + ' working days this month)';
  } else {
    display.style.display = 'none';
  }
}

function getStdMins() {
  const s = loadSettings();
  return parseFloat(s.std || 8) * 60;
}

function calcWorkedMins(entry) {
  // Leave days: paid/sick = full std hours, half = half std hours, unpaid = 0
  if (entry.leave === 'paid' || entry.leave === 'sick') return getStdMins();
  if (entry.leave === 'half') return Math.round(getStdMins() / 2);
  if (entry.leave === 'unpaid') return 0;
  const inM  = parseTime(entry.checkIn);
  const outM = parseTime(entry.checkOut);
  if (inM === null || outM === null) return 0;
  let worked = outM - inM;
  if (worked < 0) worked += 1440;
  worked -= parseBreak(entry.break1);
  worked -= parseBreak(entry.break2);
  return Math.max(0, worked);
}

function payForEntry(entry, mins) {
  const rate = getHourlyRate();
  if (!rate) return 0;
  // Unpaid leave = no pay; half day = half daily pay; paid/sick = full daily pay
  if (entry.leave === 'unpaid') return 0;
  if (entry.leave === 'paid' || entry.leave === 'sick') return (getStdMins() / 60) * rate;
  if (entry.leave === 'half') return (getStdMins() / 60) * rate * 0.5;
  return (mins / 60) * rate;
}

// ── UI ─────────────────────────────────────────────────────────────────────
function updateMonthLabel() {
  document.getElementById('month-label').textContent = MONTHS[currentMonth] + ' ' + currentYear;
}

function changeMonth(dir) {
  currentMonth += dir;
  if (currentMonth < 0)  { currentMonth = 11; currentYear--; }
  if (currentMonth > 11) { currentMonth = 0;  currentYear++; }
  updateMonthLabel();
  updateRateDisplay();
  render();
}

function render() {
  const entries = loadEntries().sort((a,b) => a.date.localeCompare(b.date));
  const tbody   = document.getElementById('tbody');
  const tfoot   = document.getElementById('tfoot');
  const stdMins = getStdMins();
  const rate    = getHourlyRate();

  let totalMins = 0, totalPay = 0, overDays = 0, underDays = 0, totalBal = 0;
  let paidLeaves = 0, unpaidLeaves = 0, halfLeaves = 0, sickLeaves = 0, totalDeduct = 0;

  if (!entries.length) {
    tbody.innerHTML = `<tr><td colspan="11" class="empty-state"><div class="icon">⏱</div><div>No entries for ${MONTHS[currentMonth]} — add your first day above</div></td></tr>`;
    tfoot.style.display = 'none';
    updateStats(0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0);
    updatePaySummary(0, 0, 0, 0, 0, 0);
    return;
  }

  tbody.innerHTML = entries.map((e, idx) => {
    const d       = new Date(e.date + 'T12:00:00');
    const dayNum  = d.getDay();
    const dayName = DAYS[dayNum];
    const isSun   = dayNum === 0;
    const isSat   = dayNum === 6;
    const leave   = e.leave || '';
    const mins    = calcWorkedMins(e);
    const bal     = leave ? 0 : mins - stdMins; // leave days don't count in balance
    const pay     = payForEntry(e, mins);

    totalMins += mins; totalPay += pay;
    if (!leave) { totalBal += mins - stdMins; }
    if (!leave && !isSun) { if (mins - stdMins > 0) overDays++; else if (mins - stdMins < 0) underDays++; }

    // Track leave counts & deductions
    const dailyPay = (getStdMins() / 60) * getHourlyRate();
    if (leave === 'paid')   { paidLeaves++;   }
    if (leave === 'sick')   { sickLeaves++;   }
    if (leave === 'half')   { halfLeaves++;   totalDeduct += dailyPay * 0.5; }
    if (leave === 'unpaid') { unpaidLeaves++; totalDeduct += dailyPay; }

    // Row class & badge
    let rowClass = isSun ? 'sunday' : isSat ? 'weekend' : '';
    if (leave) rowClass = 'leave-' + leave;
    const dayLabel = isSun ? dayName + ' 🔴' : dayName;

    let badgeClass, badgeLabel;
    if (isSun)             { badgeClass = 'exact';  badgeLabel = '🏖 Holiday'; }
    else if (leave === 'paid')   { badgeClass = 'paid';   badgeLabel = '✅ Paid Leave'; }
    else if (leave === 'unpaid') { badgeClass = 'unpaid'; badgeLabel = '❌ Unpaid Leave'; }
    else if (leave === 'half')   { badgeClass = 'half';   badgeLabel = '🌗 Half Day'; }
    else if (leave === 'sick')   { badgeClass = 'sick';   badgeLabel = '🤒 Sick Leave'; }
    else {
      const b = mins - stdMins;
      badgeClass = b > 0 ? 'over' : b < 0 ? 'under' : 'exact';
      badgeLabel = b === 0 ? 'On target' : fmtBalance(b);
    }

    const dateStr = e.date.split('-').reverse().join('/');
    const isLeaveDay = !!leave;

    return `<tr class="${rowClass}" onclick="onDateChange('${e.date}')" style="cursor:pointer" title="Click row to edit">
      <td class="td-date">${dateStr}</td>
      <td class="td-day">${dayLabel}</td>
      <td class="td-time">${isLeaveDay ? '<span style="color:var(--muted);font-size:11px;">—</span>' : (e.checkIn || '—')}</td>
      <td class="td-time">${isLeaveDay ? '<span style="color:var(--muted);font-size:11px;">—</span>' : (e.checkOut || '—')}</td>
      <td class="td-time">${isLeaveDay ? '' : (e.break1 || '—')}</td>
      <td class="td-time">${isLeaveDay ? '' : (e.break2 || '—')}</td>
      <td class="td-mins">${isLeaveDay ? '<span style="color:var(--muted)">—</span>' : mins}</td>
      <td class="td-hrs">${isLeaveDay ? '<span style="color:var(--muted)">—</span>' : fmtMins(mins)}</td>
      <td><span class="badge ${badgeClass}">${badgeLabel}</span></td>
      <td class="td-pay" style="font-weight:500; color:${leave === 'unpaid' ? 'var(--under)' : rate > 0 ? 'var(--accent)' : 'var(--muted)'}">
        ${leave === 'unpaid' ? '₹0.00' : '₹' + pay.toFixed(2)}
      </td>
      <td><button class="btn-del" onclick="event.stopPropagation();deleteEntry('${e.date}')" title="Delete">×</button></td>
    </tr>`;
  }).join('');

  // Footer
  tfoot.style.display = '';
  document.getElementById('f-mins').textContent      = totalMins + ' min';
  document.getElementById('f-hrs-total').textContent = fmtMins(totalMins);
  document.getElementById('f-bal').textContent       = fmtBalance(totalBal);
  document.getElementById('f-pay-total').textContent = '₹' + totalPay.toFixed(2);

  updateStats(entries.length, totalMins, totalBal, totalPay, overDays, underDays, paidLeaves, unpaidLeaves, halfLeaves, sickLeaves, totalDeduct);
  updatePaySummary(totalMins, totalPay, totalBal, entries.length, totalDeduct, unpaidLeaves + halfLeaves);
}

function updateStats(days, mins, bal, pay, over, under, paidL, unpaidL, halfL, sickL, deduct) {
  document.getElementById('s-days').textContent    = days;
  document.getElementById('s-hrs').textContent     = fmtMins(mins);
  document.getElementById('s-hrs-sub').textContent = mins + ' minutes total';

  const balEl = document.getElementById('s-balance');
  balEl.textContent  = fmtBalance(bal);
  balEl.className    = 'stat-value ' + (bal >= 0 ? 'pos' : 'neg');
  document.getElementById('s-balance-sub').textContent = bal >= 0 ? 'ahead of target' : 'behind target';
  document.getElementById('stat-balance').className = 'stat ' + (bal < 0 ? 'under' : '');

  const rate = getHourlyRate();
  document.getElementById('s-pay').textContent     = '₹' + pay.toFixed(2);
  document.getElementById('s-pay-sub').textContent = rate > 0 ? 'based on hours worked' : 'set salary above ↑';

  document.getElementById('s-over').textContent  = over;
  document.getElementById('s-under').textContent = under;

  document.getElementById('s-paid-leave').textContent   = (paidL || 0) + (sickL || 0);
  document.getElementById('s-unpaid-leave').textContent = (unpaidL || 0) + (halfL ? halfL + ' (½)' : '');
  document.getElementById('s-unpaid-deduct').textContent = deduct > 0 ? '−₹' + deduct.toFixed(0) + ' deducted' : 'no deduction';
}

function updatePaySummary(totalMins, totalPay, totalBal, dayCount, totalDeduct, leaveDeductDays) {
  const rate    = getHourlyRate();
  const hasRate = rate > 0;
  const summary = document.getElementById('pay-summary');
  const hint    = document.getElementById('no-rate-hint');

  if (dayCount === 0) {
    summary.style.display = 'none';
    hint.style.display    = 'none';
    return;
  }

  summary.style.display = '';
  hint.style.display    = hasRate ? 'none' : '';

  document.getElementById('ps-mins').textContent     = totalMins;
  document.getElementById('ps-mins-sub').textContent = fmtMins(totalMins);

  document.getElementById('ps-pay').textContent     = '₹' + totalPay.toFixed(2);
  document.getElementById('ps-pay-sub').textContent = hasRate
    ? 'at ₹' + rate.toFixed(2) + '/hr'
    : 'enter salary to calculate';

  const balEl = document.getElementById('ps-bal');
  balEl.textContent = fmtBalance(totalBal);
  balEl.className   = 'pay-box-value ' + (totalBal >= 0 ? 'pos' : 'neg');
  document.getElementById('ps-bal-sub').textContent = totalBal >= 0 ? 'ahead of target' : 'behind target';

  const workDays = dayCount - leaveDeductDays;
  const avg = workDays > 0 ? totalPay / workDays : 0;
  document.getElementById('ps-avg').textContent     = '₹' + avg.toFixed(2);
  document.getElementById('ps-avg-sub').textContent = workDays + ' working day' + (workDays !== 1 ? 's' : '') + ' logged';

  // Leave deduction cards
  const leaveBox = document.getElementById('ps-leave-box');
  const netBox   = document.getElementById('ps-net-box');
  if (totalDeduct > 0 && hasRate) {
    leaveBox.style.display = '';
    netBox.style.display   = '';
    document.getElementById('ps-leave-deduct').textContent = '−₹' + totalDeduct.toFixed(2);
    document.getElementById('ps-leave-sub').textContent    = leaveDeductDays + ' unpaid/half leave day' + (leaveDeductDays !== 1 ? 's' : '');
    const netPay = totalPay - totalDeduct;
    document.getElementById('ps-net-pay').textContent = '₹' + Math.max(0, netPay).toFixed(2);
    document.getElementById('ps-net-sub').textContent = 'salary − leave deductions';
  } else {
    leaveBox.style.display = 'none';
    netBox.style.display   = 'none';
  }
}

// ── Auto-fill form when date is selected ───────────────────────────────────
function onDateChange(dateVal) {
  const dateInput = document.getElementById('f-date');
  dateInput.value = dateVal;

  // Check all months' storage for this date (search current month first, then all)
  const allEntries = loadEntries();
  const existing   = allEntries.find(e => e.date === dateVal);

  const panelTitle = document.getElementById('panel-title');

  if (existing) {
    document.getElementById('f-leave').value = existing.leave  || '';
    document.getElementById('f-in').value    = existing.checkIn  || '';
    document.getElementById('f-out').value   = existing.checkOut || '';
    document.getElementById('f-b1').value    = existing.break1   || '';
    document.getElementById('f-b2').value    = existing.break2   || '';
    onLeaveChange(existing.leave || '');
    panelTitle.textContent = '✏️ Editing — ' + dateVal.split('-').reverse().join('/');
    panelTitle.style.color = 'var(--warn)';
    document.querySelector('.add-panel').scrollIntoView({behavior:'smooth', block:'nearest'});
    showToast('Entry loaded — edit & save ✏️');
  } else {
    document.getElementById('f-leave').value = '';
    document.getElementById('f-in').value    = '';
    document.getElementById('f-out').value   = '';
    document.getElementById('f-b1').value    = '';
    document.getElementById('f-b2').value    = '';
    onLeaveChange('');
    panelTitle.textContent = '+ Add Entry — ' + dateVal.split('-').reverse().join('/');
    panelTitle.style.color = '';
  }
}

// ── Leave toggle ───────────────────────────────────────────────────────────
function onLeaveChange(val) {
  const hide = !!val;
  ['field-in','field-out','field-b1','field-b2'].forEach(id => {
    document.getElementById(id).style.opacity = hide ? '0.3' : '1';
    document.getElementById(id).style.pointerEvents = hide ? 'none' : '';
  });
  if (hide) {
    document.getElementById('f-in').value  = '';
    document.getElementById('f-out').value = '';
    document.getElementById('f-b1').value  = '';
    document.getElementById('f-b2').value  = '';
  }
}

// ── Add entry ──────────────────────────────────────────────────────────────
function addEntry() {
  const date     = document.getElementById('f-date').value;
  const leave    = document.getElementById('f-leave').value;
  const checkIn  = document.getElementById('f-in').value;
  const checkOut = document.getElementById('f-out').value;
  const break1   = document.getElementById('f-b1').value || '00:00';
  const break2   = document.getElementById('f-b2').value || '00:00';

  if (!date) { showToast('Please select a date ⚠️'); return; }
  if (!leave && (!checkIn || !checkOut)) { showToast('Please fill check in & out ⚠️'); return; }
  if (!leave && break1 && !/^\d{1,2}:\d{2}$/.test(break1)) { showToast('Break 1 format: hh:mm (e.g. 00:30)'); return; }
  if (!leave && break2 && !/^\d{1,2}:\d{2}$/.test(break2)) { showToast('Break 2 format: hh:mm (e.g. 00:30)'); return; }

  const entries = loadEntries().filter(e => e.date !== date);
  entries.push({ date, leave, checkIn, checkOut, break1, break2 });
  saveEntries(entries);
  render();
  showToast(leave ? 'Leave entry saved ✓' : 'Entry saved ✓');

  const pt = document.getElementById('panel-title');
  pt.textContent = '+ Add Entry';
  pt.style.color = '';

  document.getElementById('f-leave').value = '';
  document.getElementById('f-in').value    = '';
  document.getElementById('f-out').value   = '';
  document.getElementById('f-b1').value    = '';
  document.getElementById('f-b2').value    = '';
  onLeaveChange('');
}

function deleteEntry(date) {
  const entries = loadEntries().filter(e => e.date !== date);
  saveEntries(entries);
  render();
  showToast('Entry removed');
}

function clearMonth() {
  if (!confirm(`Clear all entries for ${MONTHS[currentMonth]} ${currentYear}?`)) return;
  localStorage.removeItem(storageKey());
  render();
  showToast('Month cleared');
}

// ── Settings ───────────────────────────────────────────────────────────────
function saveSalary() {
  const s = loadSettings();
  s.salary = document.getElementById('salary-input').value;
  saveSettings(s);
  updateRateDisplay();
  render();
}

function saveStd() {
  const s = loadSettings();
  s.std = document.getElementById('std-input').value;
  saveSettings(s);
  updateRateDisplay();
  render();
}

function loadSettingsToUI() {
  const s = loadSettings();
  if (s.salary) document.getElementById('salary-input').value = s.salary;
  if (s.std)    document.getElementById('std-input').value    = s.std;
  updateRateDisplay();
}

// ── Export CSV ─────────────────────────────────────────────────────────────
function exportCSV() {
  const entries = loadEntries().sort((a,b)=>a.date.localeCompare(b.date));
  if (!entries.length) { showToast('No entries to export'); return; }
  const stdMins = getStdMins();
  const rate    = getHourlyRate();
  const rows    = [['Date','Day','Check In','Check Out','Break 1','Break 2','Minutes','Hours','Balance (mins)','Daily Pay (₹)']];
  entries.forEach(e => {
    const d    = new Date(e.date+'T12:00:00');
    const mins = calcWorkedMins(e);
    const bal  = mins - stdMins;
    const pay  = payForMins(mins);
    rows.push([e.date, DAYS[d.getDay()], e.checkIn, e.checkOut, e.break1||'', e.break2||'', mins, (mins/60).toFixed(2), bal, rate>0?pay.toFixed(2):'']);
  });
  const csv  = rows.map(r => r.join(',')).join('\n');
  const blob = new Blob([csv], {type:'text/csv'});
  const a    = document.createElement('a');
  a.href     = URL.createObjectURL(blob);
  a.download = `worklog_${MONTHS[currentMonth]}_${currentYear}.csv`;
  a.click();
  showToast('CSV downloaded ✓');
}

// ── Toast ──────────────────────────────────────────────────────────────────
let toastTimer;
function showToast(msg) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  clearTimeout(toastTimer);
  toastTimer = setTimeout(() => t.classList.remove('show'), 2200);
}

// ── Init ───────────────────────────────────────────────────────────────────
document.addEventListener('DOMContentLoaded', () => {
  const today = new Date().toISOString().split('T')[0];
  updateMonthLabel();
  loadSettingsToUI();
  render();
  onDateChange(today);
});
</script>
</body>
</html>
