<!DOCTYPE html>
<html lang="ml">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Stitching Shop Management</title>
  <!-- FontAwesome Icons -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <style>
    /* Color Theme: Dark Red, Blue, Green */
    :root {
      --primary: #8b0000; /* Dark Red */
      --secondary: #004080; /* Blue */
      --success: #2e7d32; /* Green */
      --bg: #f4f6f9;
      --card-bg: #ffffff;
      --text: #333333;
    }

    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      background-color: var(--bg);
      color: var(--text);
    }

    /* Navigation */
    nav {
      background-color: var(--primary);
      padding: 15px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      position: sticky;
      top: 0;
      z-index: 100;
    }

    nav button {
      background: none;
      border: none;
      color: white;
      font-size: 1.2rem;
      cursor: pointer;
      padding: 8px 16px;
      border-radius: 5px;
      transition: 0.3s;
    }

    nav button.active, nav button:hover {
      background-color: var(--secondary);
    }

    /* Layout Containers */
    .container {
      padding: 20px;
      max-width: 1200px;
      margin: 0 auto;
    }

    .page {
      display: none;
    }

    .page.active {
      display: block;
    }

    /* Dashboard Grid */
    .dashboard-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 15px;
      margin-bottom: 25px;
    }

    .card {
      background-color: var(--card-bg);
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      display: flex;
      align-items: center;
      gap: 15px;
      border-left: 5px solid var(--secondary);
    }

    .card.success { border-left-color: var(--success); }
    .card.danger { border-left-color: var(--primary); }

    .card i {
      font-size: 2.5rem;
      color: var(--secondary);
    }
    .card.success i { color: var(--success); }
    .card.danger i { color: var(--primary); }

    .card-info h3 {
      margin: 0;
      font-size: 0.9rem;
      color: #666;
    }

    .card-info p {
      margin: 5px 0 0 0;
      font-size: 1.5rem;
      font-weight: bold;
    }

    /* Forms and Inputs */
    .form-group {
      margin-bottom: 15px;
    }

    .form-group label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
    }

    .form-group input, .form-group select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      box-sizing: border-box;
    }

    .btn {
      background-color: var(--secondary);
      color: white;
      border: none;
      padding: 10px 15px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 1rem;
    }

    .btn-success { background-color: var(--success); }
    .btn-danger { background-color: var(--primary); }

    /* Tables */
    table {
      width: 100%;
      border-collapse: collapse;
      background-color: var(--card-bg);
      border-radius: 8px;
      overflow: hidden;
      box-shadow: 0 2px 4px rgba(0,0,0,0.05);
      margin-top: 15px;
    }

    th, td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }

    th {
      background-color: var(--secondary);
      color: white;
    }

    .hint {
      font-size: 0.8rem;
      color: #d9534f;
      margin-top: 3px;
    }

    /* Modal */
    .modal {
      display: none;
      position: fixed;
      top: 0; left: 0; width: 100%; height: 100%;
      background: rgba(0,0,0,0.5);
      justify-content: center;
      align-items: center;
    }

    .modal-content {
      background: white;
      padding: 20px;
      border-radius: 8px;
      width: 90%;
      max-width: 500px;
    }
  </style>
</head>
<body>

  <!-- Navigation -->
  <nav>
    <button onclick="showPage('dashboard')" class="active" title="Dashboard"><i class="fa-solid fa-chart-line"></i></button>
    <button onclick="showPage('new-order')" title="New Order"><i class="fa-solid fa-cart-plus"></i></button>
    <button onclick="showPage('employees')" title="Employees"><i class="fa-solid fa-users"></i></button>
    <button onclick="showPage('completed')" title="Completed Orders"><i class="fa-solid fa-circle-check"></i></button>
    <button onclick="showPage('analytics')" title="Monthly Analysis"><i class="fa-solid fa-calendar-days"></i></button>
  </nav>

  <div class="container">

    <!-- DASHBOARD PAGE -->
    <div id="dashboard" class="page active">
      <h2><i class="fa-solid fa-gauge"></i> Dashboard</h2>
      <div class="dashboard-grid">
        <div class="card">
          <i class="fa-solid fa-indian-rupee-sign"></i>
          <div class="card-info">
            <h3>Total Revenue</h3>
            <p id="dash-revenue">₹0</p>
          </div>
        </div>
        <div class="card success">
          <i class="fa-solid fa-chart-pie"></i>
          <div class="card-info">
            <h3>Total Profit</h3>
            <p id="dash-profit">₹0</p>
          </div>
        </div>
        <div class="card danger">
          <i class="fa-solid fa-user-gear"></i>
          <div class="card-info">
            <h3>Owner Profit</h3>
            <p id="dash-owner">₹0</p>
          </div>
        </div>
        <div class="card">
          <i class="fa-solid fa-scissors"></i>
          <div class="card-info">
            <h3>Stitch Charges</h3>
            <p id="dash-stitch">₹0</p>
          </div>
        </div>
        <div class="card success">
          <i class="fa-solid fa-box"></i>
          <div class="card-info">
            <h3>Current Month Orders</h3>
            <p id="dash-month-orders">0</p>
          </div>
        </div>
      </div>

      <h3><i class="fa-solid fa-truck"></i> Next Delivery</h3>
      <div id="next-delivery-card" class="card danger">
        <i class="fa-solid fa-clock"></i>
        <div class="card-info">
          <h3 id="next-cust-name">No upcoming deliveries</h3>
          <p id="next-cust-date" style="font-size:1rem;"></p>
        </div>
      </div>

      <h3><i class="fa-solid fa-list-check"></i> Active Orders</h3>
      <table id="active-orders-table">
        <thead>
          <tr>
            <th>Customer</th>
            <th>Delivery Date</th>
            <th>Employee</th>
            <th>Status / Action</th>
          </tr>
        </thead>
        <tbody id="active-orders-list"></tbody>
      </table>
    </div>

    <!-- NEW ORDER PAGE -->
    <div id="new-order" class="page">
      <h2><i class="fa-solid fa-cart-plus"></i> New Customer / Order</h2>
      <form id="order-form">
        <div class="form-group">
          <label><i class="fa-solid fa-user"></i> Customer Name</label>
          <input type="text" id="cust-name" required>
        </div>
        <div class="form-group">
          <label><i class="fa-brands fa-whatsapp"></i> WhatsApp Number</label>
          <input type="text" id="cust-phone" required>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-tag"></i> Price (₹)</label>
          <input type="number" id="order-price" oninput="calculateDetails()" required>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-wallet"></i> Advance Payment (₹)</label>
          <input type="number" id="order-advance" required>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-coins"></i> Total Cost (₹)</label>
          <input type="number" id="order-cost" oninput="calculateDetails()" value="0">
          <div id="adv-hint" class="hint"></div>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-calendar"></i> Delivery Date</label>
          <input type="date" id="order-date" required>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-user-ninja"></i> Assign Employee</label>
          <select id="order-employee" required></select>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-scissors"></i> Stitch Charge (₹)</label>
          <input type="number" id="stitch-charge" oninput="calculateDetails()" required>
        </div>
        <div class="form-group">
          <label><i class="fa-solid fa-chart-line"></i> Profit (Calculated)</label>
          <input type="number" id="order-profit" readonly>
        </div>

        <button type="submit" class="btn btn-success"><i class="fa-solid fa-floppy-disk"></i> Save Order</button>
      </form>
    </div>

    <!-- EMPLOYEES PAGE -->
    <div id="employees" class="page">
      <h2><i class="fa-solid fa-users"></i> Employee Management</h2>
      <form id="employee-form" style="margin-bottom: 20px;">
        <div class="form-group">
          <label><i class="fa-solid fa-user"></i> Employee Name</label>
          <input type="text" id="emp-name" required>
        </div>
        <div class="form-group">
          <label><i class="fa-brands fa-whatsapp"></i> WhatsApp Number</label>
          <input type="text" id="emp-phone" required>
        </div>
        <button type="submit" class="btn"><i class="fa-solid fa-plus"></i> Add Employee</button>
      </form>

      <h3>Employee Work Status</h3>
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>WhatsApp</th>
            <th>Pending Work</th>
          </tr>
        </thead>
        <tbody id="employee-list"></tbody>
      </table>
    </div>

    <!-- COMPLETED ORDERS PAGE -->
    <div id="completed" class="page">
      <h2><i class="fa-solid fa-circle-check"></i> Completed Orders</h2>
      <table>
        <thead>
          <tr>
            <th>Customer Name</th>
            <th>Total Orders</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody id="completed-orders-list"></tbody>
      </table>
    </div>

    <!-- MONTHLY ANALYTICS -->
    <div id="analytics" class="page">
      <h2><i class="fa-solid fa-calendar-days"></i> Monthly Data Analysis</h2>
      <div class="form-group">
        <label>Select Month</label>
        <input type="month" id="analytics-month" onchange="renderAnalytics()">
      </div>
      <div class="dashboard-grid">
        <div class="card"><div class="card-info"><h3>Total Orders</h3><p id="m-orders">0</p></div></div>
        <div class="card success"><div class="card-info"><h3>Revenue</h3><p id="m-revenue">₹0</p></div></div>
        <div class="card"><div class="card-info"><h3>Profit</h3><p id="m-profit">₹0</p></div></div>
        <div class="card danger"><div class="card-info"><h3>Employee Charges</h3><p id="m-stitch">₹0</p></div></div>
        <div class="card danger"><div class="card-info"><h3>Owner Amount</h3><p id="m-owner">₹0</p></div></div>
      </div>
    </div>

  </div>

  <!-- Customer Detail Modal -->
  <div id="modal" class="modal">
    <div class="modal-content">
      <h3 id="modal-title">Details</h3>
      <div id="modal-body"></div>
      <button class="btn btn-danger" onclick="closeModal()" style="margin-top:15px;"><i class="fa-solid fa-xmark"></i> Close</button>
    </div>
  </div>

  <!-- Firebase SDKs -->
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-firestore.js"></script>

  <script>
    // --- 1. FIREBASE CONFIGURATION ---
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };
    
    firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();

    // Default Initial Employees Data
    const defaultEmployees = [
      { name: "umma", phone: "8943887325" },
      { name: "ani mol", phone: "+919995549582" },
      { name: "meema (v)", phone: "9745081002" },
      { name: "nusrah", phone: "+917510702526" }
    ];

    let employees = [];
    let orders = [];

    // --- NAVIGATION ---
    function showPage(pageId) {
      document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
      document.querySelectorAll('nav button').forEach(b => b.classList.remove('active'));
      document.getElementById(pageId).classList.add('active');
      renderDashboard();
    }

    // --- INIT & REALTIME LISTENERS ---
    window.onload = function() {
      initEmployees();
      listenToOrders();
    };

    function initEmployees() {
      db.collection("employees").onSnapshot(snapshot => {
        if(snapshot.empty) {
          defaultEmployees.forEach(emp => db.collection("employees").add(emp));
        } else {
          employees = [];
          snapshot.forEach(doc => employees.push({id: doc.id, ...doc.data()}));
          renderEmployeeSelect();
          renderEmployeeList();
        }
      });
    }

    function listenToOrders() {
      db.collection("orders").onSnapshot(snapshot => {
        orders = [];
        snapshot.forEach(doc => orders.push({id: doc.id, ...doc.data()}));
        renderDashboard();
        renderCompletedOrders();
      });
    }

    // --- CALCULATIONS & AUTOMATIONS ---
    function calculateDetails() {
      const price = parseFloat(document.getElementById('order-price').value) || 0;
      const advanceInput = document.getElementById('order-advance');
      
      // Automatic Half Price Set for Advance
      if (price > 0 && !advanceInput.dataset.touched) {
        advanceInput.value = price / 2;
      }

      const advance = parseFloat(advanceInput.value) || 0;
      const cost = parseFloat(document.getElementById('order-cost').value) || 0;
      const stitch = parseFloat(document.getElementById('stitch-charge').value) || 0;
      const ownerAmount = 300;

      // Profit Formula = Total Price - (Cost + Stitch Charge + Owner Amount 300)
      const profit = price - (cost + stitch + ownerAmount);
      document.getElementById('order-profit').value = profit;

      // Hint calculation
      const remainingAdv = advance - cost;
      const hint = document.getElementById('adv-hint');
      if (cost > 0) {
        hint.innerText = `Remaining Advance Balance: ₹${remainingAdv >= 0 ? remainingAdv : 0}`;
      } else {
        hint.innerText = '';
      }
    }

    document.getElementById('order-advance').addEventListener('focus', function() {
      this.dataset.touched = "true";
    });

    // --- ORDER WORKFLOW & WHATSAPP MSG ---
    function sendWhatsApp(phone, message) {
      const cleanPhone = phone.replace(/[^0-9]/g, '');
      const url = `https://wa.me/${cleanPhone}?text=${encodeURIComponent(message)}`;
      window.open(url, '_blank');
    }

    // --- SAVE ORDER ---
    document.getElementById('order-form').addEventListener('submit', function(e) {
      e.preventDefault();
      
      const orderData = {
        customerName: document.getElementById('cust-name').value,
        phone: document.getElementById('cust-phone').value,
        price: parseFloat(document.getElementById('order-price').value),
        advance: parseFloat(document.getElementById('order-advance').value),
        cost: parseFloat(document.getElementById('order-cost').value),
        date: document.getElementById('order-date').value,
        employee: document.getElementById('order-employee').value,
        stitchCharge: parseFloat(document.getElementById('stitch-charge').value),
        profit: parseFloat(document.getElementById('order-profit').value),
        ownerAmount: 300,
        status: 'ADVANCE_PENDING', // ADVANCE_PENDING -> FULL_PAID -> FINISHED
        createdAt: new Date().toISOString()
      };

      db.collection("orders").add(orderData).then(() => {
        alert("Order Saved Successfully!");
        document.getElementById('order-form').reset();
        showPage('dashboard');
      });
    });

    // --- STEP ACTIONS & WHATSAPP AUTOMATION ---
    function processOrderStep(orderId) {
      const order = orders.find(o => o.id === orderId);
      const emp = employees.find(e => e.name === order.employee);

      if (order.status === 'ADVANCE_PENDING') {
        // Step 1: Advance paid -> Change to Full Paid & Send Msg
        db.collection("orders").doc(orderId).update({ status: 'FULL_PAID' });
        sendWhatsApp(order.phone, `Hello ${order.customerName}, your advance payment of ₹${order.advance} is completed successfully. Thank you!`);
      
      } else if (order.status === 'FULL_PAID') {
        // Step 2: Full paid -> Stitch Charge Step & Send Msg (Dress Delivered text included)
        db.collection("orders").doc(orderId).update({ status: 'STITCH_PENDING' });
        sendWhatsApp(order.phone, `Hello ${order.customerName}, your full payment is completed and your dress has been delivered! Thank you for shopping with us.`);
      
      } else if (order.status === 'STITCH_PENDING') {
        // Step 3: Finish Order -> Move to Completed & Msg to Employee
        db.collection("orders").doc(orderId).update({ status: 'FINISHED' });
        if(emp) {
          sendWhatsApp(emp.phone, `Hello ${emp.name}, your stitch charge of ₹${order.stitchCharge} for order (${order.customerName}) has been paid.`);
        }
      }
    }

    // --- DASHBOARD RENDER ---
    function renderDashboard() {
      let totalRev = 0, totalProfit = 0, totalOwner = 0, totalStitch = 0;
      const currentMonth = new Date().toISOString().substring(0, 7);
      let currentMonthOrdersCount = 0;

      const activeOrdersList = document.getElementById('active-orders-list');
      activeOrdersList.innerHTML = '';

      let activeOrders = orders.filter(o => o.status !== 'FINISHED');

      // Sort by Delivery Date for "Next Delivery"
      activeOrders.sort((a,b) => new Date(a.date) - new Date(b.date));

      if(activeOrders.length > 0) {
        document.getElementById('next-cust-name').innerText = activeOrders[0].customerName;
        document.getElementById('next-cust-date').innerText = `Delivery: ${activeOrders[0].date}`;
      } else {
        document.getElementById('next-cust-name').innerText = "No upcoming deliveries";
        document.getElementById('next-cust-date').innerText = "";
      }

      orders.forEach(o => {
        totalRev += o.price;
        totalProfit += o.profit;
        totalOwner += o.ownerAmount;
        totalStitch += o.stitchCharge;

        if (o.date.startsWith(currentMonth)) {
          currentMonthOrdersCount++;
        }
      });

      document.getElementById('dash-revenue').innerText = `₹${totalRev}`;
      document.getElementById('dash-profit').innerText = `₹${totalProfit}`;
      document.getElementById('dash-owner').innerText = `₹${totalOwner}`;
      document.getElementById('dash-stitch').innerText = `₹${totalStitch}`;
      document.getElementById('dash-month-orders').innerText = currentMonthOrdersCount;

      // Active Table
      activeOrders.forEach(o => {
        let btnText = "", btnClass = "";
        if(o.status === 'ADVANCE_PENDING') {
          btnText = '<i class="fa-solid fa-wallet"></i> Pay Advance'; btnClass = "btn";
        } else if(o.status === 'FULL_PAID') {
          btnText = '<i class="fa-solid fa-money-check-dollar"></i> Full Paid'; btnClass = "btn btn-success";
        } else if(o.status === 'STITCH_PENDING') {
          btnText = '<i class="fa-solid fa-scissors"></i> Stitch Charge Paid'; btnClass = "btn btn-danger";
        }

        activeOrdersList.innerHTML += `
          <tr>
            <td>${o.customerName}</td>
            <td>${o.date}</td>
            <td>${o.employee}</td>
            <td><button class="${btnClass}" onclick="processOrderStep('${o.id}')">${btnText}</button></td>
          </tr>
        `;
      });
    }

    // --- EMPLOYEES RENDER ---
    function renderEmployeeSelect() {
      const select = document.getElementById('order-employee');
      select.innerHTML = '';
      employees.forEach(e => {
        select.innerHTML += `<option value="${e.name}">${e.name}</option>`;
      });
    }

    function renderEmployeeList() {
      const list = document.getElementById('employee-list');
      list.innerHTML = '';
      
      employees.forEach(e => {
        const pendingCount = orders.filter(o => o.employee === e.name && o.status !== 'FINISHED').length;
        list.innerHTML += `
          <tr>
            <td>${e.name}</td>
            <td>${e.phone}</td>
            <td><i class="fa-solid fa-briefcase"></i> ${pendingCount} Active Works</td>
          </tr>
        `;
      });
    }

    document.getElementById('employee-form').addEventListener('submit', function(e) {
      e.preventDefault();
      const name = document.getElementById('emp-name').value;
      const phone = document.getElementById('emp-phone').value;

      db.collection("employees").add({ name, phone }).then(() => {
        sendWhatsApp(phone, `Hello ${name}, welcome! You have been added to our Stitching App.`);
        document.getElementById('employee-form').reset();
      });
    });

    // --- COMPLETED ORDERS PAGE ---
    function renderCompletedOrders() {
      const list = document.getElementById('completed-orders-list');
      list.innerHTML = '';

      const finishedOrders = orders.filter(o => o.status === 'FINISHED');
      
      // Group by Customer Name
      const customerMap = {};
      finishedOrders.forEach(o => {
        if(!customerMap[o.customerName]) customerMap[o.customerName] = [];
        customerMap[o.customerName].push(o);
      });

      for (let cust in customerMap) {
        list.innerHTML += `
          <tr>
            <td>${cust}</td>
            <td>${customerMap[cust].length} Order(s)</td>
            <td><button class="btn" onclick="viewCustomerDetails('${cust}')"><i class="fa-solid fa-eye"></i> View</button></td>
          </tr>
        `;
      }
    }

    function viewCustomerDetails(custName) {
      const custOrders = orders.filter(o => o.customerName === custName);
      let html = `<h4>History for ${custName}</h4><ul>`;
      custOrders.forEach(o => {
        html += `<li>Date: ${o.date} - Price: ₹${o.price} - Employee: ${o.employee}</li>`;
      });
      html += `</ul>`;
      
      document.getElementById('modal-title').innerText = "Customer Order History";
      document.getElementById('modal-body').innerHTML = html;
      document.getElementById('modal').style.display = 'flex';
    }

    function closeModal() {
      document.getElementById('modal').style.display = 'none';
    }

    // --- MONTHLY ANALYTICS ---
    function renderAnalytics() {
      const selectedMonth = document.getElementById('analytics-month').value;
      if (!selectedMonth) return;

      const filtered = orders.filter(o => o.date.startsWith(selectedMonth));

      let rev = 0, profit = 0, stitch = 0, owner = 0;
      filtered.forEach(o => {
        rev += o.price;
        profit += o.profit;
        stitch += o.stitchCharge;
        owner += o.ownerAmount;
      });

      document.getElementById('m-orders').innerText = filtered.length;
      document.getElementById('m-revenue').innerText = `₹${rev}`;
      document.getElementById('m-profit').innerText = `₹${profit}`;
      document.getElementById('m-stitch').innerText = `₹${stitch}`;
      document.getElementById('m-owner').innerText = `₹${owner}`;
    }
  </script>
</body>
</html>
