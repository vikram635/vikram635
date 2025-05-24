<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Login to Attendance App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: 30px auto;
      padding: 0 20px;
      background-color: #f9f9f9;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    input, button, select {
      padding: 8px;
      margin: 5px 0;
      font-size: 1em;
    }
    button {
      cursor: pointer;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 4px;
    }
    button:hover {
      background-color: #2980b9;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: white;
    }
    th, td {
      border: 1px solid #aaa;
      padding: 8px;
      text-align: center;
    }
    th {
      background-color: #ecf0f1;
    }
    #attendanceSection {
      display: none;
    }
  </style>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
</head>
<body>

  <h1>Login</h1>

  <div id="loginSection">
    <button onclick="googleLogin()">Login with Google</button><br><br>

    <div id="recaptcha-container"></div>
    <input type="text" id="phoneNumber" placeholder="+1234567890"><br>
    <button onclick="sendOTP()">Send OTP</button><br>
    <input type="text" id="otp" placeholder="Enter OTP"><br>
    <button onclick="verifyOTP()">Verify OTP</button>
  </div>

  <div id="attendanceSection">
    <h1>Attendance Application</h1>

    <div>
      <label for="nameInput">Add User:</label><br />
      <input type="text" id="nameInput" placeholder="Enter name" />
      <button onclick="addUser()">Add</button>
    </div>

    <div style="margin-top: 20px;">
      <label for="userSelect">Mark Attendance:</label><br />
      <select id="userSelect">
        <option value="" disabled selected>Select user</option>
      </select>
      <select id="statusSelect">
        <option value="Present">Present</option>
        <option value="Absent">Absent</option>
      </select>
      <button onclick="markAttendance()">Mark</button>
    </div>

    <table id="attendanceTable">
      <thead>
        <tr>
          <th>Name</th>
          <th>Attendance Status</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <button style="margin-top: 20px;" onclick="downloadCSV()">Download CSV</button>
  </div>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);

    const auth = firebase.auth();

    function googleLogin() {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then(result => {
          alert("Logged in as " + result.user.displayName);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    function sendOTP() {
      const phoneNumber = document.getElementById('phoneNumber').value;
      const appVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container', {
        'size': 'normal',
        'callback': function (response) {
          console.log('reCAPTCHA solved');
        }
      });
      auth.signInWithPhoneNumber(phoneNumber, appVerifier)
        .then(confirmationResult => {
          window.confirmationResult = confirmationResult;
          alert("OTP sent");
        }).catch(console.error);
    }

    function verifyOTP() {
      const code = document.getElementById('otp').value;
      window.confirmationResult.confirm(code)
        .then(result => {
          alert("Logged in as " + result.user.phoneNumber);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    // Attendance logic
    const users = [];
    const attendance = {};

    function addUser() {
      const nameInput = document.getElementById('nameInput');
      const name = nameInput.value.trim();
      if (!name) {
        alert('Please enter a name');
        return;
      }
      if (users.includes(name)) {
        alert('User already exists');
        return;
      }
      users.push(name);
      attendance[name] = 'Absent';
      updateUserSelect();
      updateAttendanceTable();
      nameInput.value = '';
    }

    function updateUserSelect() {
      const select = document.getElementById('userSelect');
      select.innerHTML = '<option value="" disabled selected>Select user</option>';
      users.forEach(user => {
        const option = document.createElement('option');
        option.value = user;
        option.textContent = user;
        select.appendChild(option);
      });
    }

    function markAttendance() {
      const userSelect = document.getElementById('userSelect');
      const statusSelect = document.getElementById('statusSelect');
      const user = userSelect.value;
      const status = statusSelect.value;
      if (!user) {
        alert('Please select a user');
        return;
      }
      attendance[user] = status;
      updateAttendanceTable();
    }

    function updateAttendanceTable() {
      const tbody = document.getElementById('attendanceTable').querySelector('tbody');
      tbody.innerHTML = '';
      users.forEach(user => {
        const tr = document.createElement('tr');
        const tdName = document.createElement('td');
        <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Login to Attendance App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: 30px auto;
      padding: 0 20px;
      background-color: #f9f9f9;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    input, button, select {
      padding: 8px;
      margin: 5px 0;
      font-size: 1em;
    }
    button {
      cursor: pointer;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 4px;
    }
    button:hover {
      background-color: #2980b9;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: white;
    }
    th, td {
      border: 1px solid #aaa;
      padding: 8px;
      text-align: center;
    }
    th {
      background-color: #ecf0f1;
    }
    #attendanceSection {
      display: none;
    }
  </style>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
</head>
<body>

  <h1>Login</h1>

  <div id="loginSection">
    <button onclick="googleLogin()">Login with Google</button><br><br>

    <div id="recaptcha-container"></div>
    <input type="text" id="phoneNumber" placeholder="+1234567890"><br>
    <button onclick="sendOTP()">Send OTP</button><br>
    <input type="text" id="otp" placeholder="Enter OTP"><br>
    <button onclick="verifyOTP()">Verify OTP</button>
  </div>

  <div id="attendanceSection">
    <h1>Attendance Application</h1>

    <div>
      <label for="nameInput">Add User:</label><br />
      <input type="text" id="nameInput" placeholder="Enter name" />
      <button onclick="addUser()">Add</button>
    </div>

    <div style="margin-top: 20px;">
      <label for="userSelect">Mark Attendance:</label><br />
      <select id="userSelect">
        <option value="" disabled selected>Select user</option>
      </select>
      <select id="statusSelect">
        <option value="Present">Present</option>
        <option value="Absent">Absent</option>
      </select>
      <button onclick="markAttendance()">Mark</button>
    </div>

    <table id="attendanceTable">
      <thead>
        <tr>
          <th>Name</th>
          <th>Attendance Status</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <button style="margin-top: 20px;" onclick="downloadCSV()">Download CSV</button>
  </div>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);

    const auth = firebase.auth();

    function googleLogin() {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then(result => {
          alert("Logged in as " + result.user.displayName);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    function sendOTP() {
      const phoneNumber = document.getElementById('phoneNumber').value;
      const appVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container', {
        'size': 'normal',
        'callback': function (response) {
          console.log('reCAPTCHA solved');
        }
      });
      auth.signInWithPhoneNumber(phoneNumber, appVerifier)
        .then(confirmationResult => {
          window.confirmationResult = confirmationResult;
          alert("OTP sent");
        }).catch(console.error);
    }

    function verifyOTP() {
      const code = document.getElementById('otp').value;
      window.confirmationResult.confirm(code)
        .then(result => {
          alert("Logged in as " + result.user.phoneNumber);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    // Attendance logic
    const users = [];
    const attendance = {};

    function addUser() {
      const nameInput = document.getElementById('nameInput');
      const name = nameInput.value.trim();
      if (!name) {
        alert('Please enter a name');
        return;
      }
      if (users.includes(name)) {
        alert('User already exists');
        return;
      }
      users.push(name);
      attendance[name] = 'Absent';
      updateUserSelect();
      updateAttendanceTable();
      nameInput.value = '';
    }

    function updateUserSelect() {
      const select = document.getElementById('userSelect');
      select.innerHTML = '<option value="" disabled selected>Select user</option>';
      users.forEach(user => {
        const option = document.createElement('option');
        option.value = user;
        option.textContent = user;
        select.appendChild(option);
      });
    }

    function markAttendance() {
      const userSelect = document.getElementById('userSelect');
      const statusSelect = document.getElementById('statusSelect');
      const user = userSelect.value;
      const status = statusSelect.value;
      if (!user) {
        alert('Please select a user');
        return;
      }
      attendance[user] = status;
      updateAttendanceTable();
    }

    function updateAttendanceTable() {
      const tbody = document.getElementById('attendanceTable').querySelector('tbody');
      tbody.innerHTML = '';
      users.forEach(user => {
        const tr = document.createElement('tr');
        const tdName = document.createElement('td');
        tdName.textContent = user;
        const tdStatus = document.createElement('td');
        tdStatus.textContent = attendance[user];
        tr.appendChild(tdName);
        tr.appendChild(tdStatus);
        tbody.appendChild(tr);
      });
    }

    function downloadCSV() {
      let csvContent = "Name,Attendance Status\n";
      users.forEach(user => {
        csvContent += `${user},${attendance[user]}\n`;
      });
      const blob = new Blob([csvContent], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.setAttribute('href', url);
      a.setAttribute('download', 'attendance.csv');
      a.click();
      URL.revokeObjectURL(url);
    }
  </script>
</body>
</html>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Login to Attendance App</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 800px;
      margin: 30px auto;
      padding: 0 20px;
      background-color: #f9f9f9;
      color: #333;
    }
    h1 {
      text-align: center;
      color: #2c3e50;
    }
    input, button, select {
      padding: 8px;
      margin: 5px 0;
      font-size: 1em;
    }
    button {
      cursor: pointer;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 4px;
    }
    button:hover {
      background-color: #2980b9;
    }
    table {
      width: 100%;
      margin-top: 20px;
      border-collapse: collapse;
      background-color: white;
    }
    th, td {
      border: 1px solid #aaa;
      padding: 8px;
      text-align: center;
    }
    th {
      background-color: #ecf0f1;
    }
    #attendanceSection {
      display: none;
    }
  </style>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
</head>
<body>

  <h1>Login</h1>

  <div id="loginSection">
    <button onclick="googleLogin()">Login with Google</button><br><br>

    <div id="recaptcha-container"></div>
    <input type="text" id="phoneNumber" placeholder="+1234567890"><br>
    <button onclick="sendOTP()">Send OTP</button><br>
    <input type="text" id="otp" placeholder="Enter OTP"><br>
    <button onclick="verifyOTP()">Verify OTP</button>
  </div>

  <div id="attendanceSection">
    <h1>Attendance Application</h1>

    <div>
      <label for="nameInput">Add User:</label><br />
      <input type="text" id="nameInput" placeholder="Enter name" />
      <button onclick="addUser()">Add</button>
    </div>

    <div style="margin-top: 20px;">
      <label for="userSelect">Mark Attendance:</label><br />
      <select id="userSelect">
        <option value="" disabled selected>Select user</option>
      </select>
      <select id="statusSelect">
        <option value="Present">Present</option>
        <option value="Absent">Absent</option>
      </select>
      <button onclick="markAttendance()">Mark</button>
    </div>

    <table id="attendanceTable">
      <thead>
        <tr>
          <th>Name</th>
          <th>Attendance Status</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>

    <button style="margin-top: 20px;" onclick="downloadCSV()">Download CSV</button>
  </div>

  <script>
    const firebaseConfig = {
      apiKey: "YOUR_API_KEY",
      authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
      projectId: "YOUR_PROJECT_ID",
      storageBucket: "YOUR_PROJECT_ID.appspot.com",
      messagingSenderId: "YOUR_SENDER_ID",
      appId: "YOUR_APP_ID"
    };
    firebase.initializeApp(firebaseConfig);

    const auth = firebase.auth();

    function googleLogin() {
      const provider = new firebase.auth.GoogleAuthProvider();
      auth.signInWithPopup(provider)
        .then(result => {
          alert("Logged in as " + result.user.displayName);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    function sendOTP() {
      const phoneNumber = document.getElementById('phoneNumber').value;
      const appVerifier = new firebase.auth.RecaptchaVerifier('recaptcha-container', {
        'size': 'normal',
        'callback': function (response) {
          console.log('reCAPTCHA solved');
        }
      });
      auth.signInWithPhoneNumber(phoneNumber, appVerifier)
        .then(confirmationResult => {
          window.confirmationResult = confirmationResult;
          alert("OTP sent");
        }).catch(console.error);
    }

    function verifyOTP() {
      const code = document.getElementById('otp').value;
      window.confirmationResult.confirm(code)
        .then(result => {
          alert("Logged in as " + result.user.phoneNumber);
          document.getElementById("loginSection").style.display = "none";
          document.getElementById("attendanceSection").style.display = "block";
        }).catch(console.error);
    }

    // Attendance logic
    const users = [];
    const attendance = {};

    function addUser() {
      const nameInput = document.getElementById('nameInput');
      const name = nameInput.value.trim();
      if (!name) {
        alert('Please enter a name');
        return;
      }
      if (users.includes(name)) {
        alert('User already exists');
        return;
      }
      users.push(name);
      attendance[name] = 'Absent';
      updateUserSelect();
      updateAttendanceTable();
      nameInput.value = '';
    }

    function updateUserSelect() {
      const select = document.getElementById('userSelect');
      select.innerHTML = '<option value="" disabled selected>Select user</option>';
      users.forEach(user => {
        const option = document.createElement('option');
        option.value = user;
        option.textContent = user;
        select.appendChild(option);
      });
    }

    function markAttendance() {
      const userSelect = document.getElementById('userSelect');
      const statusSelect = document.getElementById('statusSelect');
      const user = userSelect.value;
      const status = statusSelect.value;
      if (!user) {
        alert('Please select a user');
        return;
      }
      attendance[user] = status;
      updateAttendanceTable();
    }

    function updateAttendanceTable() {
      const tbody = document.getElementById('attendanceTable').querySelector('tbody');
      tbody.innerHTML = '';
      users.forEach(user => {
        const tr = document.createElement('tr');
        const tdName = document.createElement('td');
        tdName.textContent = user;
        const tdStatus = document.createElement('td');
        tdStatus.textContent = attendance[user];
        tr.appendChild(tdName);
        tr.appendChild(tdStatus);
        tbody.appendChild(tr);
      });
    }

    function downloadCSV() {
      let csvContent = "Name,Attendance Status\n";
      users.forEach(user => {
        csvContent += `${user},${attendance[user]}\n`;
      });
      const blob = new Blob([csvContent], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);

      const a = document.createElement('a');
      a.setAttribute('href', url);
      a.setAttribute('download', 'attendance.csv');
      a.click();
      URL.revokeObjectURL(url);
    }
  </script>
</body>
</html>
.textContent = user;
        const tdStatus = document.createElement('td');
        tdStatus.textContent = attendance[user];
        tr.appendChild(tdName);
        tr.appendChild(tdStatus);
        tbody.appendChild(tr);
      });
    }

    function downloadCSV() {
      let csvContent = "Name,Attendance Status\n";
      users.forEach(user => {
        csvContent += `${user},${attendance[user]}\n`;
      });
      const blob = new Blob([csvContent], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);

      const a = document.createElement('a');
    