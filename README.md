<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>SUNRISE MAINTENANCE</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<style>
body{font-family:Calibri,sans-serif;background:#f4f6f8;margin:0}

header{display:flex;justify-content:space-between;align-items:center;background:#1f3c88;color:#fff;padding:15px 20px}
.brand{display:flex;align-items:center;gap:12px}
.brand-logo{height:42px;width:auto;object-fit:contain;background:#fff;border-radius:6px;padding:4px 6px}
header h1{margin:0;display:flex;align-items:center;gap:10px;white-space:nowrap}
.badge{font-size:12px;background:rgba(255,255,255,.2);padding:3px 8px;border-radius:999px}
.logout-btn{width:auto;padding:8px 15px;background:#fff;color:#1f3c88;border:none;border-radius:4px;cursor:pointer}
.logout-btn:hover{background:#e9eef6}

.container{max-width:1200px;margin:20px auto;padding:15px}
.card{background:#fff;padding:15px;border-radius:6px;margin-bottom:20px;box-shadow:0 2px 5px rgba(0,0,0,.1)}
label{font-weight:bold;margin-top:10px;display:block}
input,select,textarea,button{width:100%;padding:10px;margin-top:5px;box-sizing:border-box}
button{background:#1f3c88;color:#fff;border:none;cursor:pointer}
button:hover{background:#162b5b}
table{width:100%;border-collapse:collapse;margin-top:10px}
th,td{border:1px solid #ccc;padding:8px;text-align:left;vertical-align:top}
th{background:#e9eef6}

.kpi{display:flex;gap:10px;flex-wrap:wrap}
.kpi div{flex:1 1 45%;background:#e9eef6;padding:10px;border-radius:6px}

.form-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:15px}
.form-grid .full{grid-column:1/-1}
@media(max-width:768px){.form-grid{grid-template-columns:1fr}}

.nav{display:none;gap:10px;justify-content:center;margin:15px}
.action-btn{width:auto;padding:6px 10px;margin:0 4px 0 0;border-radius:4px;display:inline-block}
.action-btn.danger{background:#b00020}
.action-btn.danger:hover{background:#7f0016}
.muted{color:#666;font-size:12px;margin-top:6px}

.task-list{display:flex;flex-direction:column;gap:8px}
.task-item{display:flex;gap:8px;align-items:center}
.task-item input{flex:1}
.small-btn{width:auto;padding:8px 10px}
.small-btn.secondary{background:#e9eef6;color:#1f3c88}
.small-btn.secondary:hover{background:#dbe4f3}
textarea{min-height:90px;resize:vertical}

@media(max-width:480px){
  .brand-logo{height:32px}
  header h1{font-size:18px}
}
</style>

<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
</head>

<body>

<header>
  <div class="brand">
    <img src="logo.png" class="brand-logo" alt="Sunrise Logo">
    <h1>
      SUNRISE MAINTENANCE
      <span class="badge" id="roleBadge" style="display:none"></span>
    </h1>
  </div>
  <button id="logoutBtn" class="logout-btn" style="display:none">Log Out</button>
</header>

<div class="nav" id="navBar">
  <button id="btnTool" type="button">Tool & Enquiry</button>
  <button id="btnMaint" type="button">Maintenance Performance</button>
</div>

<!-- LOGIN -->
<div class="container" id="loginScreen">
  <div class="card">
    <h2>Login / Sign Up</h2>
    <label>Email</label><input id="email" type="email" autocomplete="username">
    <label>Password</label><input id="password" type="password" autocomplete="current-password">

    <label>Role (for Sign Up)</label>
    <select id="role">
      <option value="Technician">Technician</option>
      <option value="Engineer">Engineer</option>
      <option value="Admin">Admin</option>
    </select>
    <div class="muted">
      User Rules: Admin (edit/assign/delete), Engineer (add/assign), Technician (add only)
    </div>

    <button id="loginBtn" type="button">Login</button>
    <button id="signupBtn" type="button">Sign Up</button>
  </div>
</div>

<!-- TOOL & ENQUIRY -->
<div class="container" id="mainScreen" style="display:none">

  <div class="card">
    <h2>Register Tool</h2>
    <div class="form-grid">
      <div><label>Date Register</label><input type="date" id="dateRegister"></div>
      <div><label>Gate Pass No</label><input id="gatePass"></div>
      <div><label>Customer</label><input id="customer"></div>
      <div><label>Serial</label><input id="serial"></div>
      <div><label>Model</label><input id="model"></div>
      <div>
        <label>Status</label>
        <select id="status">
          <option value="Received">Received</option>
          <option value="Under Repair">Under Repair</option>
          <option value="Completed">Completed</option>
          <option value="Returned">Returned</option>
        </select>
      </div>
      <div>
        <label>Tool Condition</label>
        <select id="toolCondition">
          <option value="Good">Good</option>
          <option value="Average">Average</option>
          <option value="Poor">Poor</option>
        </select>
      </div>

      <div><label>User</label><input id="toolUser"></div>
      <div><label>User Contact No</label><input id="toolContact" placeholder="e.g. 012-3456789"></div>

      <div>
        <label>PIC</label>
        <select id="pic">
          <option value="Haziq">Haziq</option>
          <option value="Wan">Wan</option>
          <option value="Aziz">Aziz</option>
          <option value="Logen">Logen</option>
          <option value="Mohd">Mohd</option>
        </select>
      </div>
      <div><label>Image</label><input type="file" id="image" accept="image/*"></div>
      <div class="full"><button id="saveToolBtn" type="button">Save Tool</button></div>
    </div>
  </div>

  <div class="card">
    <h2>Enquiry Status (Quotation & PO)</h2>
    <div class="form-grid">
      <div><label>Serial</label><select id="enquirySerial"></select></div>
      <div><label>Query No</label><input id="queryNo"></div>
      <div><label>Quotation No</label><input id="quotationNo"></div>
      <div><label>Date Quotation Out</label><input type="date" id="quotationDate"></div>
      <div><label>PO No</label><input id="poNo"></div>
      <div><label>Date PO Received</label><input type="date" id="poDate"></div>
      <div class="full"><button id="saveEnquiryBtn" type="button">Save Enquiry</button></div>
    </div>

    <h3>Quotation & PO Records</h3>
    <table>
      <thead><tr>
        <th>Serial</th><th>Query No</th><th>Quotation No</th><th>Quotation Date</th>
        <th>PO No</th><th>PO Date</th><th>Action</th>
      </tr></thead>
      <tbody id="enquiryTable"></tbody>
    </table>
  </div>

  <div class="card">
    <h2>KPI Dashboard</h2>
    <div class="kpi">
      <div>Total Tools: <span id="total">0</span></div>
      <div>Completed: <span id="completed">0</span></div>
      <div>Avg Days: <span id="avg">0</span></div>
    </div>

    <h3>Tool Records</h3>
    <table>
      <thead><tr>
        <th>Date</th><th>Gate Pass</th><th>Customer</th>
        <th>Serial</th><th>Model</th><th>Status</th>
        <th>Condition</th><th>User</th><th>Contact No</th><th>PIC</th><th>Image</th><th>Action</th>
      </tr></thead>
      <tbody id="toolTable"></tbody>
    </table>

    <button id="exportBtn" type="button">Export Excel</button>
  </div>
</div>

<!-- MAINTENANCE PERFORMANCE -->
<div class="container" id="maintenanceTab" style="display:none">

  <!-- STAFF DAILY TASKS -->
  <div class="card">
    <h2>Staff Daily Tasks</h2>
    <div class="form-grid">
      <div><label>Staff</label><input id="dailyStaff"></div>
      <div><label>Date Assign</label><input type="date" id="dateAssign"></div>

      <div class="full">
        <label>Tasks (you can add multiple)</label>
        <div class="task-list" id="taskList"></div>
        <button type="button" class="small-btn secondary" id="addTaskRowBtn">+ Add Task</button>
      </div>

      <div class="full">
        <label>Task Update</label>
        <textarea id="taskUpdate" placeholder="Progress update / remarks..."></textarea>
      </div>

      <div><label>Date Complete</label><input type="date" id="dateComplete"></div>
      <div class="full"><button id="saveDailyBtn" type="button">Save Daily Tasks</button></div>
    </div>

    <table>
      <thead>
        <tr>
          <th>Staff</th>
          <th>Date Assign</th>
          <th>Tasks</th>
          <th>Task Update</th>
          <th>Date Complete</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody id="dailyTable"></tbody>
    </table>
  </div>

  <!-- OUTSTANDING TASK -->
  <div class="card">
    <h2>Outstanding Task</h2>
    <div class="form-grid">
      <div><label>Customer</label><input id="outCustomer"></div>
      <div><label>Date Complete</label><input type="date" id="outComplete"></div>

      <div class="full">
        <label>Issues (you can add multiple)</label>
        <div class="task-list" id="issueList"></div>
        <button type="button" class="small-btn secondary" id="addIssueRowBtn">+ Add Issue</button>
      </div>

      <div class="full"><button id="saveOutstandingBtn" type="button">Save</button></div>
    </div>

    <table>
      <thead><tr>
        <th>Customer</th><th>Date Complete</th><th>Issues</th><th>Action</th>
      </tr></thead>
      <tbody id="outTable"></tbody>
    </table>
  </div>

  <!-- IMPROVEMENT (UPDATED: MULTIPLE DESCRIPTIONS + DATE EXECUTE) -->
  <div class="card">
    <h2>Improvement</h2>
    <div class="form-grid">
      <div>
        <label>Type</label>
        <select id="improveType">
          <option value="Internal">Internal</option>
          <option value="External">External</option>
        </select>
      </div>
      <div>
        <label>Date Execute</label>
        <input type="date" id="improveDate">
      </div>

      <div class="full">
        <label>Descriptions (you can add multiple)</label>
        <div class="task-list" id="improveList"></div>
        <button type="button" class="small-btn secondary" id="addImproveRowBtn">+ Add Description</button>
      </div>

      <div class="full"><button id="saveImprovementBtn" type="button">Save</button></div>
    </div>

    <table>
      <thead><tr><th>Type</th><th>Date Execute</th><th>Descriptions</th><th>Action</th></tr></thead>
      <tbody id="improveTable"></tbody>
    </table>
  </div>
</div>

<!-- FIREBASE + APP LOGIC -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut }
  from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
import {
  getFirestore, collection, addDoc, getDocs,
  doc, setDoc, getDoc, deleteDoc
} from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
import { getStorage, ref, uploadBytes, getDownloadURL }
  from "https://www.gstatic.com/firebasejs/10.7.1/firebase-storage.js";

const firebaseConfig = {
  apiKey: "AIzaSyCJImLHqMY_5BFGVy046lPfJPwB5QdPHHQ",
  authDomain: "sunrise-maintenance-database.firebaseapp.com",
  projectId: "sunrise-maintenance-database",
  storageBucket: "sunrise-maintenance-database.firebasestorage.app",
  appId: "1:366419483397:web:6719c78433217913bef56c"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
const storage = getStorage(app);

const $ = (id) => document.getElementById(id);

/* ======= FIX HELPERS (PREVENT undefined in Firestore) ======= */
function v(el, fallback=""){
  const val = el && "value" in el ? el.value : fallback;
  return (val === undefined || val === null) ? fallback : val;
}
function clean(obj){
  if (obj === undefined) return "";
  if (obj === null) return null;
  if (Array.isArray(obj)) return obj.map(clean);
  if (typeof obj === "object"){
    const out = {};
    for (const [k, val] of Object.entries(obj)){
      const cv = clean(val);
      out[k] = (cv === undefined) ? "" : cv;
    }
    return out;
  }
  return obj;
}
/* ============================================================ */

// Screens
const loginScreen = $("loginScreen");
const mainScreen = $("mainScreen");
const maintenanceTab = $("maintenanceTab");
const navBar = $("navBar");
const logoutBtn = $("logoutBtn");
const roleBadge = $("roleBadge");

// Role
let currentRole = "Technician";
let currentUid = "";

// Tool fields
const dateRegister = $("dateRegister");
const gatePass = $("gatePass");
const customer = $("customer");
const serial = $("serial");
const model = $("model");
const statusEl = $("status");
const toolConditionEl = $("toolCondition");
const picEl = $("pic");
const toolUser = $("toolUser");
const toolContact = $("toolContact");
const image = $("image");

// Enquiry fields
const enquirySerial = $("enquirySerial");
const queryNo = $("queryNo");
const quotationNo = $("quotationNo");
const quotationDate = $("quotationDate");
const poNo = $("poNo");
const poDate = $("poDate");
const enquiryTable = $("enquiryTable");

// KPI + tables
const toolTable = $("toolTable");
const total = $("total");
const completed = $("completed");
const avg = $("avg");

// Daily tasks fields
const dailyStaff = $("dailyStaff");
const dateAssign = $("dateAssign");
const taskList = $("taskList");
const addTaskRowBtn = $("addTaskRowBtn");
const taskUpdate = $("taskUpdate");
const dateComplete = $("dateComplete");
const saveDailyBtn = $("saveDailyBtn");
const dailyTable = $("dailyTable");

// Outstanding
const outCustomer = $("outCustomer");
const outComplete = $("outComplete");
const issueList = $("issueList");
const addIssueRowBtn = $("addIssueRowBtn");
const outTable = $("outTable");

// Improvement (UPDATED)
const improveType = $("improveType");
const improveDate = $("improveDate");
const improveList = $("improveList");
const addImproveRowBtn = $("addImproveRowBtn");
const improveTable = $("improveTable");

// Buttons
$("loginBtn").addEventListener("click", login);
$("signupBtn").addEventListener("click", signup);
logoutBtn.addEventListener("click", logout);

$("btnTool").addEventListener("click", () => showTab("mainScreen"));
$("btnMaint").addEventListener("click", () => showTab("maintenanceTab"));

$("saveToolBtn").addEventListener("click", saveTool);
$("saveEnquiryBtn").addEventListener("click", saveEnquiry);
$("exportBtn").addEventListener("click", exportExcel);

$("saveImprovementBtn").addEventListener("click", saveImprovement);

// Daily tasks buttons
addTaskRowBtn.addEventListener("click", () => addTaskRow(""));
saveDailyBtn.addEventListener("click", saveDailyTasks);

// Outstanding buttons
addIssueRowBtn.addEventListener("click", () => addIssueRow(""));
$("saveOutstandingBtn").addEventListener("click", saveOutstanding);

// Improvement buttons (UPDATED)
addImproveRowBtn.addEventListener("click", () => addImproveRow(""));

// default dates + default select values
dateRegister.value = new Date().toISOString().split("T")[0];
dateAssign.value = new Date().toISOString().split("T")[0];
outComplete.value = "";
if (improveDate) improveDate.value = new Date().toISOString().split("T")[0];
if (statusEl && !statusEl.value) statusEl.value = "Received";
if (toolConditionEl && !toolConditionEl.value) toolConditionEl.value = "Good";

addTaskRow("");
addIssueRow("");
addImproveRow("");

// Tab switching
function showTab(id){
  mainScreen.style.display = "none";
  maintenanceTab.style.display = "none";
  $(id).style.display = "block";
}

/* ROLE PERMISSIONS */
function applyRolePermissions(){
  const isAdmin = currentRole === "Admin";

  roleBadge.style.display = "inline-block";
  roleBadge.textContent = currentRole;

  document.querySelectorAll(".delete-btn").forEach(btn=>{
    btn.style.display = isAdmin ? "inline-block" : "none";
  });

  const editDisabled = !isAdmin;
  statusEl.disabled = editDisabled;
  toolConditionEl.disabled = editDisabled;
  picEl.disabled = editDisabled;
}

/* AUTH */
async function signup(){
  try{
    const emailVal = $("email").value.trim();
    const passVal = $("password").value;
    const roleVal = $("role").value;

    const cred = await createUserWithEmailAndPassword(auth, emailVal, passVal);

    await setDoc(doc(db, "users", cred.user.uid), {
      email: emailVal,
      role: roleVal
    });

    alert("Sign up successful. Please login.");
  }catch(err){
    alert("Sign up failed: " + (err?.message || err));
    console.error(err);
  }
}

async function login(){
  try{
    const cred = await signInWithEmailAndPassword(auth, $("email").value.trim(), $("password").value);
    currentUid = cred.user.uid;

    const snap = await getDoc(doc(db, "users", currentUid));
    currentRole = snap.exists() ? (snap.data().role || "Technician") : "Technician";

    loginScreen.style.display = "none";
    navBar.style.display = "flex";
    logoutBtn.style.display = "inline-block";

    applyRolePermissions();
    showTab("mainScreen");
    await loadAll();
  }catch(err){
    alert("Login failed: " + (err?.message || err));
    console.error(err);
  }
}

async function logout(){
  try{
    await signOut(auth);

    currentRole = "Technician";
    currentUid = "";

    mainScreen.style.display = "none";
    maintenanceTab.style.display = "none";
    navBar.style.display = "none";
    logoutBtn.style.display = "none";
    roleBadge.style.display = "none";
    loginScreen.style.display = "block";

    $("email").value = "";
    $("password").value = "";
  }catch(err){
    alert("Logout failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadAll(){
  await Promise.all([
    loadTools(),
    loadEnquiries(),
    loadDailyTasks(),
    loadOutstanding(),
    loadImprovements()
  ]);
  applyRolePermissions();
}

/* TOOL SAVE/LOAD (FIXED) */
async function saveTool(){
  try{
    if (statusEl && !statusEl.value) statusEl.value = "Received";
    if (toolConditionEl && !toolConditionEl.value) toolConditionEl.value = "Good";

    if(!dateRegister.value) return alert("Please select Date Register.");
    if(!serial.value.trim()) return alert("Please fill Serial.");
    if(!customer.value.trim()) return alert("Please fill Customer.");

    let imgUrl = "";
    if(image.files && image.files[0]){
      const file = image.files[0];
      const r = ref(storage, `tools/${Date.now()}_${file.name}`);
      await uploadBytes(r, file);
      imgUrl = await getDownloadURL(r);
    }

    const payload = clean({
      dateRegister: dateRegister.value,
      gatePass: gatePass.value.trim(),
      customer: customer.value.trim(),
      serial: serial.value.trim(),
      model: model.value.trim(),
      status: v(statusEl, "Received"),
      toolCondition: v(toolConditionEl, "Good"),
      user: toolUser.value.trim(),
      contactNo: toolContact.value.trim(),
      pic: v(picEl, ""),
      image: imgUrl
    });

    await addDoc(collection(db,"tools"), payload);

    gatePass.value = "";
    customer.value = "";
    serial.value = "";
    model.value = "";
    toolUser.value = "";
    toolContact.value = "";
    image.value = "";
    statusEl.value = "Received";
    toolConditionEl.value = "Good";

    await loadTools();
    await loadEnquiries();
    applyRolePermissions();
    alert("Tool saved successfully.");
  }catch(err){
    alert("Save tool failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadTools(){
  toolTable.innerHTML = "";
  enquirySerial.innerHTML = "";

  let t = 0, c = 0;

  const snap = await getDocs(collection(db,"tools"));
  snap.forEach(d=>{
    const x = d.data() || {};
    if(x.serial){
      enquirySerial.innerHTML += `<option value="${escapeHtml(x.serial)}">${escapeHtml(x.serial)}</option>`;
    }

    toolTable.innerHTML += `<tr>
      <td>${escapeHtml(x.dateRegister || "")}</td>
      <td>${escapeHtml(x.gatePass || "")}</td>
      <td>${escapeHtml(x.customer || "")}</td>
      <td>${escapeHtml(x.serial || "")}</td>
      <td>${escapeHtml(x.model || "")}</td>
      <td>${escapeHtml(x.status || "")}</td>
      <td>${escapeHtml(x.toolCondition || "")}</td>
      <td>${escapeHtml(x.user || "")}</td>
      <td>${escapeHtml(x.contactNo || "")}</td>
      <td>${escapeHtml(x.pic || "")}</td>
      <td>${x.image ? `<img src="${x.image}" width="40">` : ""}</td>
      <td>
        <button class="action-btn danger delete-btn" type="button"
          onclick="deleteDocById('tools','${d.id}')">Delete</button>
      </td>
    </tr>`;
    t++;
    if(x.status === "Completed") c++;
  });

  total.innerText = t;
  completed.innerText = c;
  avg.innerText = c ? (t / c).toFixed(1) : "0";
}

/* ENQUIRY SAVE/LOAD */
async function saveEnquiry(){
  try{
    if(!enquirySerial.value) return alert("Please select Serial.");

    await addDoc(collection(db,"enquiries"), clean({
      serial: enquirySerial.value || "",
      queryNo: queryNo.value.trim() || "",
      quotationNo: quotationNo.value.trim() || "",
      quotationDate: quotationDate.value || "",
      poNo: poNo.value.trim() || "",
      poDate: poDate.value || ""
    }));

    queryNo.value = "";
    quotationNo.value = "";
    quotationDate.value = "";
    poNo.value = "";
    poDate.value = "";

    await loadEnquiries();
    applyRolePermissions();
    alert("Enquiry saved successfully.");
  }catch(err){
    alert("Save enquiry failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadEnquiries(){
  enquiryTable.innerHTML = "";
  const snap = await getDocs(collection(db,"enquiries"));
  snap.forEach(d=>{
    const e = d.data() || {};
    enquiryTable.innerHTML += `<tr>
      <td>${escapeHtml(e.serial || "")}</td>
      <td>${escapeHtml(e.queryNo || "")}</td>
      <td>${escapeHtml(e.quotationNo || "")}</td>
      <td>${escapeHtml(e.quotationDate || "")}</td>
      <td>${escapeHtml(e.poNo || "")}</td>
      <td>${escapeHtml(e.poDate || "")}</td>
      <td>
        <button class="action-btn danger delete-btn" type="button"
          onclick="deleteDocById('enquiries','${d.id}')">Delete</button>
      </td>
    </tr>`;
  });
}

/* DAILY TASKS */
function addTaskRow(value=""){
  const row = document.createElement("div");
  row.className = "task-item";
  row.innerHTML = `
    <input class="taskInput" placeholder="Task description..." value="${escapeHtml(value)}">
    <button type="button" class="small-btn danger">Remove</button>
  `;
  row.querySelector("button").addEventListener("click", ()=> row.remove());
  taskList.appendChild(row);
}

function getTaskInputs(){
  return Array.from(document.querySelectorAll(".taskInput"))
    .map(i=>i.value.trim())
    .filter(v=>v.length>0);
}

async function saveDailyTasks(){
  try{
    if(!dailyStaff.value.trim()) return alert("Please fill Staff.");
    if(!dateAssign.value) return alert("Please select Date Assign.");
    const tasks = getTaskInputs();
    if(tasks.length === 0) return alert("Please add at least 1 task.");

    await addDoc(collection(db,"daily_tasks"), clean({
      staff: dailyStaff.value.trim(),
      dateAssign: dateAssign.value,
      tasks,
      taskUpdate: taskUpdate.value.trim() || "",
      dateComplete: dateComplete.value || ""
    }));

    dailyStaff.value = "";
    dateAssign.value = new Date().toISOString().split("T")[0];
    taskUpdate.value = "";
    dateComplete.value = "";
    taskList.innerHTML = "";
    addTaskRow("");

    await loadDailyTasks();
    applyRolePermissions();
    alert("Daily tasks saved successfully.");
  }catch(err){
    alert("Save daily tasks failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadDailyTasks(){
  dailyTable.innerHTML = "";
  const snap = await getDocs(collection(db,"daily_tasks"));
  snap.forEach(d=>{
    const x = d.data() || {};
    const tasksHtml = (x.tasks || []).map((t,i)=>`${i+1}. ${escapeHtml(t)}`).join("<br>");

    const assign = x.dateAssign || x.date || "";
    const update = x.taskUpdate || "";
    const complete = x.dateComplete || "";

    dailyTable.innerHTML += `<tr>
      <td>${escapeHtml(x.staff || "")}</td>
      <td>${escapeHtml(assign)}</td>
      <td>${tasksHtml}</td>
      <td>${escapeHtml(update).replaceAll("\n","<br>")}</td>
      <td>${escapeHtml(complete)}</td>
      <td>
        <button class="action-btn danger delete-btn" type="button"
          onclick="deleteDocById('daily_tasks','${d.id}')">Delete</button>
      </td>
    </tr>`;
  });
}

/* OUTSTANDING */
function addIssueRow(value=""){
  const row = document.createElement("div");
  row.className = "task-item";
  row.innerHTML = `
    <input class="issueInput" placeholder="Issue description..." value="${escapeHtml(value)}">
    <button type="button" class="small-btn danger">Remove</button>
  `;
  row.querySelector("button").addEventListener("click", ()=> row.remove());
  issueList.appendChild(row);
}

function getIssueInputs(){
  return Array.from(document.querySelectorAll(".issueInput"))
    .map(i=>i.value.trim())
    .filter(v=>v.length>0);
}

async function saveOutstanding(){
  try{
    if(!outCustomer.value.trim()) return alert("Please fill Customer.");
    const issues = getIssueInputs();
    if(issues.length === 0) return alert("Please add at least 1 issue.");

    await addDoc(collection(db,"outstanding"), clean({
      customer: outCustomer.value.trim(),
      dateComplete: outComplete.value || "",
      issues
    }));

    outCustomer.value = "";
    outComplete.value = "";
    issueList.innerHTML = "";
    addIssueRow("");

    await loadOutstanding();
    applyRolePermissions();
    alert("Outstanding task saved successfully.");
  }catch(err){
    alert("Save outstanding failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadOutstanding(){
  outTable.innerHTML = "";
  const snap = await getDocs(collection(db,"outstanding"));
  snap.forEach(d=>{
    const o = d.data() || {};
    const issuesHtml = (o.issues || []).map((t,i)=>`${i+1}. ${escapeHtml(t)}`).join("<br>");
    outTable.innerHTML += `<tr>
      <td>${escapeHtml(o.customer || "")}</td>
      <td>${escapeHtml(o.dateComplete || o.targetDate || "")}</td>
      <td>${issuesHtml}</td>
      <td>
        <button class="action-btn danger delete-btn" type="button"
          onclick="deleteDocById('outstanding','${d.id}')">Delete</button>
      </td>
    </tr>`;
  });
}

/* IMPROVEMENTS (UPDATED: MULTIPLE DESCRIPTIONS + DATE EXECUTE) */
function addImproveRow(value=""){
  const row = document.createElement("div");
  row.className = "task-item";
  row.innerHTML = `
    <input class="improveInput" placeholder="Improvement description..." value="${escapeHtml(value)}">
    <button type="button" class="small-btn danger">Remove</button>
  `;
  row.querySelector("button").addEventListener("click", ()=> row.remove());
  improveList.appendChild(row);
}
function getImproveInputs(){
  return Array.from(document.querySelectorAll(".improveInput"))
    .map(i=>i.value.trim())
    .filter(v=>v.length>0);
}

async function saveImprovement(){
  try{
    const descriptions = getImproveInputs();
    if(descriptions.length === 0) return alert("Please add at least 1 description.");

    await addDoc(collection(db,"improvements"), clean({
      type: improveType.value || "Internal",
      dateExecute: improveDate.value || "",
      descriptions
    }));

    // clear
    if (improveDate) improveDate.value = new Date().toISOString().split("T")[0];
    improveList.innerHTML = "";
    addImproveRow("");

    await loadImprovements();
    applyRolePermissions();
    alert("Improvement saved successfully.");
  }catch(err){
    alert("Save improvement failed: " + (err?.message || err));
    console.error(err);
  }
}

async function loadImprovements(){
  improveTable.innerHTML = "";
  const snap = await getDocs(collection(db,"improvements"));
  snap.forEach(d=>{
    const i = d.data() || {};
    const descHtml = (i.descriptions || (i.description ? [i.description] : []))
      .map((t,idx)=>`${idx+1}. ${escapeHtml(t)}`).join("<br>");

    improveTable.innerHTML += `<tr>
      <td>${escapeHtml(i.type || "")}</td>
      <td>${escapeHtml(i.dateExecute || "")}</td>
      <td>${descHtml}</td>
      <td>
        <button class="action-btn danger delete-btn" type="button"
          onclick="deleteDocById('improvements','${d.id}')">Delete</button>
      </td>
    </tr>`;
  });
}

/* DELETE (ADMIN ONLY) */
window.deleteDocById = async (collectionName, docId) => {
  try{
    if(currentRole !== "Admin") return alert("Only Admin can delete data.");
    if(!confirm("Confirm delete?")) return;

    await deleteDoc(doc(db, collectionName, docId));

    if(collectionName === "tools") { await loadTools(); await loadEnquiries(); }
    if(collectionName === "enquiries") await loadEnquiries();
    if(collectionName === "daily_tasks") await loadDailyTasks();
    if(collectionName === "outstanding") await loadOutstanding();
    if(collectionName === "improvements") await loadImprovements();

    applyRolePermissions();
  }catch(err){
    alert("Delete failed: " + (err?.message || err));
    console.error(err);
  }
};

/* EXPORT */
function exportExcel(){
  const wb = XLSX.utils.book_new();
  document.querySelectorAll("table").forEach((t,idx)=>{
    XLSX.utils.book_append_sheet(wb, XLSX.utils.table_to_sheet(t), "Sheet" + (idx+1));
  });
  XLSX.writeFile(wb, "SUNRISE_MAINTENANCE.xlsx");
}

/* XSS-safe helper */
function escapeHtml(str){
  return String(str || "")
    .replaceAll("&","&amp;")
    .replaceAll("<","&lt;")
    .replaceAll(">","&gt;")
    .replaceAll('"',"&quot;")
    .replaceAll("'","&#039;");
}
</script>

</body>
</html>
