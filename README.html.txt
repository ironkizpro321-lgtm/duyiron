<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <title>Quản lý điểm thi</title>
  <script type="module">
    // Import Firebase SDK
    import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
    import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-analytics.js";
    import { getFirestore, collection, addDoc, getDocs, deleteDoc, doc } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

    // Firebase config (code của bạn)
    const firebaseConfig = {
      apiKey: "AIzaSyAlSuF-aKmhGB3btxsflPCJ2_xamRhZZEI",
      authDomain: "duyiron12dz-4b834.firebaseapp.com",
      projectId: "duyiron12dz-4b834",
      storageBucket: "duyiron12dz-4b834.firebasestorage.app",
      messagingSenderId: "325144292460",
      appId: "1:325144292460:web:3d4c5fd82fb1bb20667c49",
      measurementId: "G-FR7EYX3379"
    };

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);
    const db = getFirestore(app);

    // Thêm điểm
    window.addScore = async function () {
      const name = document.getElementById("name").value;
      const subject = document.getElementById("subject").value;
      const score = document.getElementById("score").value;

      if (!name || !subject || !score) {
        alert("Vui lòng nhập đầy đủ thông tin");
        return;
      }

      await addDoc(collection(db, "scores"), {
        name,
        subject,
        score: Number(score)
      });

      loadScores();
    };

    // Load danh sách điểm
    async function loadScores() {
      const table = document.getElementById("scoreTable");
      table.innerHTML = "";

      const querySnapshot = await getDocs(collection(db, "scores"));
      querySnapshot.forEach((docSnap) => {
        const data = docSnap.data();
        const row = `
          <tr>
            <td>${data.name}</td>
            <td>${data.subject}</td>
            <td>${data.score}</td>
            <td><button onclick="deleteScore('${docSnap.id}')">Xóa</button></td>
          </tr>`;
        table.innerHTML += row;
      });
    }

    // Xóa điểm
    window.deleteScore = async function (id) {
      await deleteDoc(doc(db, "scores", id));
      loadScores();
    };

    window.onload = loadScores;
  </script>

  <style>
    body { font-family: Arial; margin: 40px; }
    input { margin: 5px; }
    table { border-collapse: collapse; width: 100%; margin-top: 20px; }
    th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
    th { background: #f2f2f2; }
  </style>
</head>
<body>
  <h2>Trang quản lý điểm thi</h2>

  <div>
    <input id="name" placeholder="Tên sinh viên" />
    <input id="subject" placeholder="Môn học" />
    <input id="score" type="number" placeholder="Điểm" />
    <button onclick="addScore()">Thêm điểm</button>
  </div>

  <table>
    <thead>
      <tr>
        <th>Tên</th>
        <th>Môn</th>
        <th>Điểm</th>
        <th>Thao tác</th>
      </tr>
    </thead>
    <tbody id="scoreTable"></tbody>
  </table>
</body>
</html>
