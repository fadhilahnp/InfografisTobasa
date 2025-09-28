
# 📊 Aplikasi Infografis Sekolah

Aplikasi web sederhana untuk menampilkan **infografis data sekolah** yang terhubung dengan **Google Sheets** melalui **Google Apps Script**.  
Proyek ini memvisualisasikan data dalam bentuk **tabel interaktif**, **diagram pie**, **diagram batang**, serta ringkasan KPI.

## 🚀 Fitur Utama
- **Dashboard Utama (index.html)**  
  Menu utama dengan card navigasi menuju:
  - **Tahfidz** → capaian hafalan surah siswa  
  - **Absensi** → rekap kehadiran siswa (sakit, izin, alpha)  
  - **Ekstrakurikuler** → penilaian keterampilan, sikap, dan fisik  

- **Tahfidz**
  - KPI jumlah data, siswa, surah, kelas
  - Tabel dengan **pagination (20 baris per halaman)**
  - Chart pie distribusi surah
  - Chart bar top penghafal

- **Absensi**
  - KPI total siswa
  - Donut chart rekap sakit, izin, alpha
  - Tabel absensi per siswa
  - Tombol kembali ke home (ikon rumah di kanan atas)

- **Ekstrakurikuler**
  - Diagram batang horizontal per siswa
  - Penilaian: keterampilan & pengetahuan, sikap & karakter, fisik & mental
  - Skala nilai 1–100

## 🛠️ Teknologi yang Digunakan
- **Frontend**: HTML, CSS, [Tailwind CSS](https://tailwindcss.com/) (dev mode via CDN)  
- **Charting**: [Chart.js](https://www.chartjs.org/)  
- **Backend API**: Google Apps Script (JSONP)  
- **Database**: Google Sheets  

## 📂 Struktur Proyek
├── index.html # Halaman utama dashboard\
├── tahfidz.html # Infografis hafalan Al-Qur'an\
├── absensi.html # Infografis absensi siswa\
├── ekskul.html # Infografis ekstrakurikuler\
└── README.md

## ⚙️ Konfigurasi Google Sheets & Apps Script
1. Buat Google Spreadsheet dengan sheet:
   - `Tahfidz` → kolom: `Name | Class | Surah | Date`
   - `Absensi` → kolom: `Name | Class | Status | Date`
   - `Ekskul` → kolom: `Name | Class | Keterampilan dan Pengetahuan | Sikap dan Karakter | Fisik dan Mental`

2. Buka **Extensions > Apps Script**, lalu paste kode berikut:

       function doGet(e) {
         var sheetName = e.parameter.sheet || "Tahfidz";
         var callback = e.parameter.callback || "callback";
         
         var ss = SpreadsheetApp.getActiveSpreadsheet();
         var sheet = ss.getSheetByName(sheetName);
         if (!sheet) {
           return ContentService.createTextOutput(callback + "({\"error\":\"Sheet tidak ditemukan\"})")
             .setMimeType(ContentService.MimeType.JAVASCRIPT);
         }
         
         var data = sheet.getDataRange().getValues();
         var headers = data.shift();
         var rows = [];
         
         data.forEach(function(row) {
           var record = {};
           headers.forEach(function(h, i) {
             record[h.toString().trim().toLowerCase()] = row[i];
           });
           rows.push(record);
         });
         
         var result = callback + "(" + JSON.stringify({ data: rows }) + ")";
         return ContentService.createTextOutput(result).setMimeType(ContentService.MimeType.JAVASCRIPT);
       }

3. Deploy sebagai Web App:

   - Publish → Deploy as Web App
   - Access: Anyone with the link
   - Salin URL Web App (misal: https://script.google.com/macros/s/.../exec)

4. Ganti `YOUR_SCRIPT_URL` pada file HTML dengan URL Web App tersebut.

▶️ Menjalankan Aplikasi
   - Buka index.html di browser, atau host menggunakan GitHub Pages / Netlify.
   - Navigasikan ke halaman tahfidz, absensi, atau ekskul untuk melihat infografis.
