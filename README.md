# landgenotes.github.io
College notes with login access

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>College Notes - Login</title>
  <style>
    body { font-family: Arial; background-color: #f2f2f2; margin: 0; padding: 0; }
    .container { max-width: 400px; margin: 80px auto; background: white; padding: 30px; border-radius: 10px; box-shadow: 0 0 10px #aaa; }
    input, button { width: 100%; padding: 10px; margin: 10px 0; }
    #dashboard, #addSubjectSection, #notesSection { display: none; }
    .subject { background: #e0e0e0; padding: 10px; margin: 10px 0; cursor: pointer; border-radius: 5px; }
  </style>
</head>
<body>
  <div class="container" id="loginSection">
    <h2>Login</h2>
    <input type="text" id="userId" placeholder="Enter ID">
    <input type="password" id="password" placeholder="Enter Password">
    <button onclick="login()">Login</button>
    <p id="loginError" style="color:red;"></p>
  </div>

  <div class="container" id="dashboard">
    <h2>Welcome LANDGENOTES</h2>
    <button onclick="showAddSubject()">Add New Subject</button>
    <div id="subjectsList"></div>
  </div>

  <div class="container" id="addSubjectSection">
    <h3>Add Subject</h3>
    <input type="text" id="newSubject" placeholder="Subject Name">
    <button onclick="addSubject()">Add</button>
  </div>

  <div class="container" id="notesSection">
    <h3 id="subjectTitle"></h3>
    <textarea id="noteText" placeholder="Enter your note" rows="6" style="width: 100%;"></textarea>
    <input type="file" id="noteImage">
    <button onclick="saveNote()">Save Note</button>
    <div id="notesDisplay"></div>
  </div>

  <script>
    const ID = "LANDGENOTES";
    const PASS = "LANDGE@@@NOTES";
    let currentSubject = "";
    let subjects = {};

    function login() {
      const uid = document.getElementById('userId').value;
      const pwd = document.getElementById('password').value;
      if (uid === ID && pwd === PASS) {
        document.getElementById('loginSection').style.display = 'none';
        document.getElementById('dashboard').style.display = 'block';
        loadSubjects();
      } else {
        document.getElementById('loginError').innerText = 'Invalid ID or Password';
      }
    }

    function showAddSubject() {
      document.getElementById('addSubjectSection').style.display = 'block';
    }

    function addSubject() {
      const sub = document.getElementById('newSubject').value;
      if (sub && !subjects[sub]) {
        subjects[sub] = [];
        loadSubjects();
        document.getElementById('newSubject').value = '';
      }
    }

    function loadSubjects() {
      const list = document.getElementById('subjectsList');
      list.innerHTML = '';
      for (let s in subjects) {
        const div = document.createElement('div');
        div.className = 'subject';
        div.innerText = s;
        div.onclick = () => openSubject(s);
        list.appendChild(div);
      }
    }

    function openSubject(sub) {
      currentSubject = sub;
      document.getElementById('subjectTitle').innerText = sub;
      document.getElementById('dashboard').style.display = 'none';
      document.getElementById('addSubjectSection').style.display = 'none';
      document.getElementById('notesSection').style.display = 'block';
      showNotes();
    }

    function saveNote() {
      const text = document.getElementById('noteText').value;
      const imgFile = document.getElementById('noteImage').files[0];
      const reader = new FileReader();
      reader.onload = function(e) {
        subjects[currentSubject].push({ text: text, img: e.target.result });
        showNotes();
        document.getElementById('noteText').value = '';
        document.getElementById('noteImage').value = '';
      }
      if (imgFile) reader.readAsDataURL(imgFile);
      else {
        subjects[currentSubject].push({ text: text });
        showNotes();
      }
    }

    function showNotes() {
      const display = document.getElementById('notesDisplay');
      display.innerHTML = '';
      subjects[currentSubject].forEach(note => {
        const div = document.createElement('div');
        div.innerHTML = `<p>${note.text}</p>`;
        if (note.img) div.innerHTML += `<img src="${note.img}" style="max-width:100%;"/>`;
        div.style.marginBottom = '20px';
        display.appendChild(div);
      });
    }
  </script>
</body>
</html>
