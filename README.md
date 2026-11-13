# AccessBoard-Forms-Accessibility
Plaintext
accessboard-forms/
├── index.html
├── style.css
└── script.js
1. index.html
Bu faylda har bir input va select uchun maxsus <label> elementlari qilingan, shuningdek ekran o'quvchilar uchun role="alert" va klaviatura orqali boshqariluvchi fayl biriktirish qismi qo'shilgan.

HTML
<!DOCTYPE html>
<html lang="uz">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AccessBoard - Forms Accessibility & File API</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="container">
    <h1>AccessBoard — Karta va Fayl Qo'shish</h1>

    <!-- Validatsiya xatolari uchun ARIA alert -->
    <div id="error-announcer" class="sr-only" aria-live="assertive" role="alert"></div>

    <form id="task-form" novalidate>
      <!-- Xatoliklar chiqadigan vizual quti -->
      <div id="form-error" class="error-message hidden" aria-hidden="true"></div>

      <!-- Vazifa nomi (To'g'ri label bog'lanishi) -->
      <div class="form-group">
        <label for="task-title">Vazifa sarlavhasi <span class="required" aria-hidden="true">*</span></label>
        <input type="text" id="task-title" name="taskTitle" aria-required="true">
      </div>

      <!-- Holati tanlash -->
      <div class="form-group">
        <label for="task-status">Vazifa holati</label>
        <select id="task-status" name="taskStatus">
          <option value="todo">⏳ Kutilmoqda</option>
          <option value="progress">⚙️ Jarayonda</option>
          <option value="done">✅ Bajarildi</option>
        </select>
      </div>

      <!-- Drag-and-Drop va Input File birlashgan qismi (Klaviatura va Sichqoncha uchun) -->
      <div class="form-group">
        <label id="file-label">Fayl biriktirish (Drag & Drop yoki tanlash)</label>
        
        <div id="dropzone" class="dropzone" tabindex="0" role="region" aria-labelledby="file-label">
          <p>Fayllarni shu yerga sudrab tashlang yoki</p>
          
          <!-- Yashirin, lekin Tab bilan fokuslanadigan input type="file" -->
          <input type="file" id="file-input" name="taskFile" multiple class="sr-only-focusable">
          
          <!-- Klaviatura va sichqoncha uchun bosiladigan maxsus tugma -->
          <button type="button" id="browse-btn" class="btn-secondary">Fayl tanlash</button>
        </div>
        
        <!-- Tanlangan fayllar ro'yxati -->
        <ul id="file-list" class="file-list" aria-live="polite"></ul>
      </div>

      <button type="submit" class="btn-primary">Kartani Saqlash</button>
    </form>
  </main>

  <script src="script.js"></script>
</body>
</html>
2. style.css
Bu yerda interfeys qulayligi va fokus holatlari (klaviaturada yurgan paytda yaqqol ko'rinishi uchun) chiroyli qilib sozlangan.

CSS
:root {
  --bg-color: #f3f4f6;
  --card-bg: #ffffff;
  --text-main: #111827;
  --border-color: #4b5563;
  --primary: #1d4ed8;
  --primary-hover: #1e40af;
  --error-bg: #fee2e2;
  --error-text: #991b1b;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  background-color: var(--bg-color);
  color: var(--text-main);
  padding: 2rem 1rem;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: var(--card-bg);
  padding: 2rem;
  border-radius: 8px;
  border: 2px solid #d1d5db;
}

h1 {
  font-size: 1.5rem;
  margin-bottom: 1.5rem;
  text-align: center;
}

.form-group {
  margin-bottom: 1.25rem;
  display: flex;
  flex-direction: column;
  gap: 0.35rem;
}

label {
  font-weight: 600;
  font-size: 0.95rem;
}

.required {
  color: #dc2626;
}

input[type="text"], select {
  padding: 0.65rem;
  font-size: 1rem;
  border: 2px solid var(--border-color);
  border-radius: 6px;
}

/* Fokus qilinganda aniq рамка (Accessibility uchun muhim) */
input:focus, select:focus, button:focus, .dropzone:focus {
  outline: 3px solid #2563eb;
  outline-offset: 2px;
}

/* Dropzone dizayni */
.dropzone {
  border: 2px dashed var(--border-color);
  padding: 1.5rem;
  text-align: center;
  border-radius: 6px;
  background-color: #f9fafb;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.75rem;
}

.dropzone.dragover {
  background-color: #dbeafe;
  border-color: var(--primary);
}

.btn-secondary {
  background-color: #e5e7eb;
  color: #111827;
  border: 2px solid var(--border-color);
  padding: 0.5rem 1rem;
  font-weight: 600;
  border-radius: 4px;
  cursor: pointer;
}

.btn-secondary:hover {
  background-color: #d1d5db;
}

.btn-primary {
  background-color: var(--primary);
  color: #ffffff;
  border: none;
  padding: 0.75rem 1.25rem;
  font-size: 1rem;
  font-weight: 600;
  border-radius: 6px;
  cursor: pointer;
  width: 100%;
  margin-top: 1rem;
}

.btn-primary:hover {
  background-color: var(--primary-hover);
}

/* Ekran o'quvchilar uchun yashirin, lekin o'qiladigan klass */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

/* Tab orqali kirish mumkin bo'lgan yashirin input */
.sr-only-focusable {
  position: absolute;
  width: 1px;
  height: 1px;
  opacity: 0;
  pointer-events: none;
}

.error-message {
  background-color: var(--error-bg);
  color: var(--error-text);
  padding: 0.75rem;
  border-radius: 6px;
  border: 1px solid #f87171;
  font-weight: 600;
  margin-bottom: 1rem;
}

.hidden {
  display: none;
}

.file-list {
  list-style: none;
  padding: 0;
  margin-top: 0.5rem;
  font-size: 0.9rem;
}
3. script.js
Bu fayl drag-and-drop hodisalarini, klaviatura orqali (Enter yoki Space yordamida) fayl tanlashni va role="alert" orqali xatolarni ovozli o'qittirish logikasini bajaradi.

JavaScript
const form = document.getElementById('task-form');
const taskTitle = document.getElementById('task-title');
const dropzone = document.getElementById('dropzone');
const fileInput = document.getElementById('file-input');
const browseBtn = document.getElementById('browse-btn');
const fileList = document.getElementById('file-list');
const errorAnnouncer = document.getElementById('error-announcer');
const formError = document.getElementById('form-error');

let attachedFiles = [];

// 1. "Fayl tanlash" tugmasi bosilganda yashirin input'ni ochish (Sichqoncha va Klaviatura uchun)
browseBtn.addEventListener('click', () => {
  fileInput.click();
});

// Dropzone'ning o'zida Enter yoki Space bosilsa ham fayl oynasini ochish (Klaviatura accessibility)
dropzone.addEventListener('keydown', (e) => {
  if (e.key === 'Enter' || e.key === ' ') {
    e.preventDefault();
    fileInput.click();
  }
});

// Input orqali fayllar tanlanganda
fileInput.addEventListener('change', (e) => {
  handleFiles(e.target.files);
});

// 2. Drag-and-Drop hodisalari (Sichqoncha uchun)
['dragenter', 'dragover'].forEach(eventName => {
  dropzone.addEventListener(eventName, (e) => {
    e.preventDefault();
    dropzone.classList.add('dragover');
  });
});

['dragleave', 'drop'].forEach(eventName => {
  dropzone.addEventListener(eventName, (e) => {
    e.preventDefault();
    dropzone.classList.remove('dragover');
  });
});

dropzone.addEventListener('drop', (e) => {
  const dt = e.dataTransfer;
  const files = dt.files;
  handleFiles(files);
});

// 3. Fayllarni boshqarish va ro'yxatga qo'shish
function handleFiles(files) {
  for (let i = 0; i < files.length; i++) {
    attachedFiles.push(files[i]);
  }
  renderFileList();
}

function renderFileList() {
  fileList.innerHTML = '';
  attachedFiles.forEach((file, index) => {
    const li = document.createElement('li');
    li.textContent = `📄 ${file.name} (${Math.round(file.size / 1024)} KB)`;
    fileList.appendChild(li);
  });
}

// 4. Formani jo'natish va Validatsiya (role="alert" orqali e'lon qilish)
form.addEventListener('submit', (e) => {
  e.preventDefault();
  
  if (!taskTitle.value.trim()) {
    const errorText = "Diqqat: Vazifa sarlavhasi bo'sh bo'lishi mumkin emas. Iltimos, sarlavha kiriting.";
    
    // Vizual chiqarish
    formError.textContent = errorText;
    formError.classList.remove('hidden');
    formError.setAttribute('aria-hidden', 'false');
    
    // ARIA alert orqali ekran o'quvchiga DARHOL o'qittirish
    errorAnnouncer.textContent = errorText;
    
    // Fokusni xato chiqqan inputga qaratish
    taskTitle.focus();
  } else {
    // Xatolik bo'lmasa
    formError.classList.add('hidden');
    formError.setAttribute('aria-hidden', 'true');
    errorAnnouncer.textContent = '';
    
    alert("Forma muvaffaqiyatli saqlandi!");
    form.reset();
    attachedFiles = [];
    fileList.innerHTML = '';
  }
});
