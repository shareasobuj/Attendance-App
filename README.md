# Attendance-App

<html>
<head>
  <title>Smart Attendance App</title>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <script type="module" src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
  <script type="module" src="https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js"></script>
  <script type="module" src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>
  <style>
    body { font-family: Arial, sans-serif; padding: 20px; max-width: 400px; margin: auto; }
    input, button { padding: 10px; margin: 5px 0; width: 100%; }
    #attendanceForm, #logoutSection { display: none; }
  </style>
</head>
<body>
  <h2>Attendance SaaS App</h2>

  <div id="authSection">
    <input type="email" id="email" placeholder="Email" required>
    <input type="password" id="password" placeholder="Password" required>
    <button onclick="login()">Login</button>
    <button onclick="signup()">Sign Up</button>
  </div>

  <div id="attendanceForm">
    <h3>Welcome, <span id="userEmail"></span></h3>
    <input type="text" id="name" placeholder="Your Name">
    <button onclick="markAttendance()">Mark Attendance</button>
  </div>

  <div id="logoutSection">
    <button onclick="logout()">Logout</button>
  </div>

  <script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js";
    import { getAuth, onAuthStateChanged, signInWithEmailAndPassword, createUserWithEmailAndPassword, signOut } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-auth.js";
    import { getFirestore, collection, addDoc, serverTimestamp } from "https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js";

    const firebaseConfig = {
      apiKey: "AIzaSyByatsmgonajt-asVSiUU3iZmvnJs64pZo",
      authDomain: "webapps-7c885.firebaseapp.com",
      projectId: "webapps-7c885",
      storageBucket: "webapps-7c885.firebasestorage.app",
      messagingSenderId: "380965082731",
      appId: "1:380965082731:web:68cd3974d16bc8dcc0c32e",
      measurementId: "G-SG14HX48N0"
    };

    const app = initializeApp(firebaseConfig);
    const auth = getAuth();
    const db = getFirestore(app);

    window.signup = async () => {
      const email = emailEl.value;
      const password = passwordEl.value;
      await createUserWithEmailAndPassword(auth, email, password);
    };

    window.login = async () => {
      const email = emailEl.value;
      const password = passwordEl.value;
      await signInWithEmailAndPassword(auth, email, password);
    };

    window.logout = async () => {
      await signOut(auth);
    };

    window.markAttendance = async () => {
      const name = document.getElementById("name").value;
      const email = auth.currentUser.email;
      await addDoc(collection(db, "attendance"), {
        name,
        email,
        timestamp: serverTimestamp()
      });
      alert("Attendance Marked!");
    };

    const emailEl = document.getElementById("email");
    const passwordEl = document.getElementById("password");

    onAuthStateChanged(auth, user => {
      if (user) {
        document.getElementById("authSection").style.display = "none";
        document.getElementById("attendanceForm").style.display = "block";
        document.getElementById("logoutSection").style.display = "block";
        document.getElementById("userEmail").innerText = user.email;
      } else {
        document.getElementById("authSection").style.display = "block";
        document.getElementById("attendanceForm").style.display = "none";
        document.getElementById("logoutSection").style.display = "none";
      }
    });
  </script>
</body>
</html>
