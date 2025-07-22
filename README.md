# Inventory_Bullying_Sekolah
ini adalah inventory bullying agar murid yang terkena bullying bisa mengadu lewat gform yang diberikan sehabis itu akan diproses oleh sheet yang sudah dicustom scriptnya

---

## 🧠 Fitur Unggulan

- Integrasi otomatis Google Form ke Google Sheets.
- Analisis otomatis menggunakan Google Apps Script.
- Deteksi berbagai jenis perundungan (4 kategori).
- Bisa digunakan tanpa perlu install software tambahan.
- Didesain untuk dipakai guru non-teknis.

---

## 🛠️ Langkah Instalasi dan Integrasi

### 1. Buat Google Form

1. Masuk ke akun Google (disarankan akun institusi/sekolah).
2. Buka [Google Form](https://forms.google.com).
3. Buat formulir baru.
4. Tambahkan **1 pertanyaan esai panjang**:
   - Judul: `Apa yang ingin kamu sampaikan?`
   - Wajib diisi: ✅

---

### 2. Hubungkan ke Google Sheets

1. Klik ikon spreadsheet di atas form (`Link ke spreadsheet`).
2. Pilih `Buat spreadsheet baru` → Klik **Buat**.
3. Spreadsheet akan otomatis menerima data dari Form.
4. Tambahkan Kolom untuk **Analisis Sentimen**, **Tipe Kekerasan**, **Skor Risiko**

---

### 3. Tambahkan Google Apps Script

1. Di Google Sheets, klik:
   - `Extensions` → `Apps Script`.
2. Hapus semua isi default, lalu **tempelkan skrip kode yang telah diberikan**.
3. Simpan dengan nama Detector Code

---

### 4. Set Trigger Otomatis

1. Masuk ke Apps Script → Triggers (ikon jam).
2. Klik + Add Trigger.
   - Function to run: onFormSubmit
   - Deployment: Head
   - Event source: From spreadsheet
   - Event Type: On Form Submit
3. Klik Save
4. Berikan izin ke Apps Script jika diminta (klik Review Permission → pilih akun → klik Izinkan).
   - Jika Danger Geser Ke Bawah Lalu Klik Advance Lalu Izinkan

---

### 5. Test

1. Kirim 1 Form Untuk di test


