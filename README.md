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
   - Judul: `Ceritakan pengalamanmu`
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
2. Hapus semua isi default, lalu **tempelkan skrip kode yang telah diberikan kamu**.
   ```javascript
   function onFormSubmit(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Form Responses 1");
  const lastRow = sheet.getLastRow();
  const text = sheet.getRange(lastRow, 2).getValue(); // Cerita
  const result = analyzeText(text);

  // Masukkan hasil analisis ke kolom
  sheet.getRange(lastRow, 3).setValue(result.sentiment); // Sentimen
  sheet.getRange(lastRow, 4).setValue(result.violence);  // Tipe Kekerasan
  sheet.getRange(lastRow, 5).setValue(result.score);     // Skor Risiko

  // Kirim email jika risiko tinggi
  if (result.score >= 50) {
    const emailTujuan = "ihsanakmalarrazi15@gmail.com"; // Ganti dengan email guru/konselor

    const subject = "‼️ Laporan Risiko Tinggi pada Siswa ‼️";
    const body = `📌 Deteksi Risiko Tinggi Kekerasan\n\n` +
                 `⚠️ Jenis Kekerasan: ${result.violence}\n` +
                 `🔎 Sentimen: ${result.sentiment}\n` +
                 `📊 Skor Risiko: ${result.score}\n` +
                 `📝 Isi Cerita: ${text}\n\n` +
                 `💬 "Jadilah awal dari perubahan, untuk menciptakan keadaan yang lebih aman"`;

    MailApp.sendEmail(emailTujuan, subject, body);
  }
}

  function analyzeText(text) {
  const lower = text.toLowerCase();
  let violence = [];
  let score = 0;

  // Kategori Fisik
  const fisikWords = ["dipukul", "ditonjok", "ditampar", "ditendang", "disikut", "dicubit", "dicekik", "dijambak", "ditarik rambut", "didorong", "dijatuhkan", "dilempar", "diseret", "dijegal", "dihantam", "diludahi", "disundut rokok", "dipukul pakai penggaris", "dipukul pakai sepatu", "ditendang perut", "didorong ke tembok", "dibanting ke lantai", "diludahi wajah", "disakiti secara fisik", "dilukai dengan sengaja", "disakiti di tempat sepi", "diserang diam-diam", "disakiti dalam kelas", "dijebak untuk disakiti", "diborgol mainan", "diikat", "dikeroyok", "dijepit pintu", "dilukai pakai benda tajam", "dipukul dari belakang", "disakiti saat pulang sekolah", "disakiti di toilet", "disakiti tanpa sebab", "dikeroyok rame-rame", "ditarik bajunya", "baju disobek", "seragam dirusak", "tas disiram air", "tas dibuang", "sepatu disembunyikan", "dipukul saat tidur", "dibenturkan kepala", "dipukul kepala", "dibenturkan meja"];
  let fisikCount = 0;
  fisikWords.forEach((word) => {
    if (lower.includes(word)) {
      fisikCount++;
      score += 25;
    }
  });
  if (fisikCount > 0) violence.push("Fisik");

  // Kategori Verbal
  const verbalWords = ["dikatain bodoh", "dikatain jelek", "dikatain miskin", "dikatain bau", "dikatain kampungan", "dikatain gak guna", "dikatain goblok", "dikatain idiot", "dikatain tolol", "dikatain gila", "dikatain gak punya otak", "dikatain pelit", "dikatain bencong", "dikatain banci", "dikatain homo", "dikatain jablay", "dikatain pelacur", "dikatain jalang", "dikatain lemah", "dikatain gendut", "dikatain kerempeng", "dikatain hitam", "dikatain gak pantes sekolah di sini", "dikatain beban", "dikatain gak laku", "dikatain najis", "dikatain jorok", "dikatain najis diajak temenan", "dikatain anak setan", "dikatain penyihir", "dikatain pembawa sial", "dikatain pengacau", "dikatain gagal", "dikatain kutu buku", "dikatain freak", "dikatain loner", "dikatain forever alone", "dikatain cupu", "dikatain murahan", "dikatain gak pantas hidup", "dikatain mati aja", "dikatain mending gak usah lahir", "dikatain bikin jijik", "dikatain lebih baik ilang"];
  let verbalCount = 0;
  verbalWords.forEach((word) => {
    if (lower.includes(word)) {
      verbalCount++;
      score += 10;
    }
  });
  if (verbalCount > 0) violence.push("Verbal");

  // Kategori Sosial
  const sosialWords = ["dijauhi", "dikucilkan", "tidak diajak main", "tidak dianggap", "diasingkan", "disuruh pergi", "disuruh minggir", "tidak boleh gabung", "tidak diajak kerja kelompok", "diabaikan", "dikeluarkan dari grup", "di-remove dari grup", "duduk sendirian", "tidak diajak bicara", "selalu ditinggal", "tidak dilibatkan", "dibicarakan di belakang", "ditertawakan diam-diam", "jadi bahan omongan", "jadi bahan gosip", "dianggap aneh", "dianggap norak", "dicap cupu", "dicap miskin", "dibilang bau", "dianggap tidak pantas berteman", "disebut pembawa sial", "dibenci tanpa alasan", "dianggap penyakit sosial", "disuruh menjauh", "tidak diterima di circle", "tidak masuk lingkaran pertemanan", "tidak punya teman", "tidak ada yang mendekat", "dilihat dengan jijik", "ditatap sinis", "dihindari saat lewat", "nama disebut dengan ejekan", "disindir terus-menerus", "tidak pernah diajak ngobrol", "tidak pernah diajak kerja kelompok", "disuruh diam", "dipotong saat bicara", "tidak diberi giliran bicara", "komentar tidak direspons", "komentar diabaikan", "pendapat disepelekan", "pendapat ditertawakan", "disuruh duduk sendiri", "dijadikan bahan lelucon", "dipaksa menjauh", "dipaksa keluar grup", "tidak dibalas chat", "chat diabaikan", "chat di-read doang", "grup sengaja dikunci", "grup sengaja dibuat tanpa melibatkan", "diblock tanpa alasan", "unfollow massal", "dikeluarin dari komunitas", "tidak dimasukkan dalam pembagian tugas", "nama tidak disebut saat pembagian kelompok", "dilupakan dengan sengaja", "tidak disebut dalam daftar teman", "ditertawakan dari jauh", "selalu disalahkan", "disebut pembawa masalah", "dipandang sebelah mata", "disebut beban kelompok", "dianggap tidak kompeten", "dibilang gak penting", "dibilang gak berguna", "dibilang perusak suasana", "tidak diajak makan bareng", "tidak diundang acara", "dikeluarkan dari dokumentasi kelas", "diabaikan saat minta tolong", "dijauhi tanpa alasan", "satu meja kosong karena ditinggal", "tidak disapa", "tidak dibalas salam", "disuruh pergi tanpa sebab", "dibilang gak cocok di sini", "disuruh pindah meja", "duduk dikelilingi keheningan", "satu kelas tidak peduli", "tidak dianggap bagian dari kelas", "suasana canggung tiap hadir", "kehadiran tidak direspons","bodoh", "jelek", "miskin", "bau", "kampungan", "gak guna", "goblok", "idiot", "tolol", "gila", "gak punya otak", "pelit", "bencong", "banci", "homo", "jablay", "pelacur", "jalang", "lemah", "gendut", "kerempeng", "hitam", "gak pantes", "beban", "gak laku", "najis", "jorok", "anak setan", "penyihir", "pembawa sial", "pengacau", "gagal", "kutu buku", "freak", "loner", "forever alone", "cupu", "murahan", "mati aja", "gak usah lahir", "jijik", "lebih baik ilang"
];
  let sosialCount = 0;
  sosialWords.forEach((word) => {
    if (lower.includes(word)) {
      sosialCount++;
      score += 20;
    }
  });
  if (sosialCount > 0) violence.push("Sosial");

// Kategori Seksual
  const seksualWords = ["digrepe", "dientot", "dipegang dadanya", "pantat dipegang", "dielus paha", "disentuh bagian sensitif", "disentuh tanpa izin", "disentuh paksa", "dipegang dari belakang", "tubuh dipegang", "bagian pribadi dipegang", "dipeluk paksa", "dicium tanpa izin", "dipaksa ciuman", "dicolek bagian pribadi", "dijilat leher", "digesek-gesek", "digesek kelamin", "digesek dari belakang", "disorongkan kelamin", "diraba-raba", "diraba payudara", "disentuh di tempat gelap", "diganggu secara seksual", "digodain cabul", "dikatain mesum", "dikatain seksi", "dikatain genit", "dikatain jalang", "dikatain pelacur", "dibilang penggoda", "dibilang gatel", "dibilang murahan", "dibilang gampangan", "dibilang nafsuan", "dibilang suka seks", "dibilang suka disentuh", "dibilang cocok buat ditiduri", "disuruh buka baju", "disuruh telanjang", "disuruh videoin tubuh", "diminta kirim foto bugil", "diminta kirim video buka baju", "dipaksa kirim foto vulgar", "dipaksa kirim foto payudara", "dipaksa buka rok", "dipaksa buka celana", "dipaksa ngelayanin", "dipaksa ngangkang", "dipaksa dilihat bagian sensitif", "dilecehkan secara verbal", "dilecehkan secara fisik", "dilecehkan secara online", "diomongin alat kelamin", "dikatain mau ngentot", "dikatain enak dilihat", "dikatain mau digarap", "dibilang empuk", "dibilang montok", "dibilang cocok buat disetubuhi", "dibilang pengen dicobain", "dibilang udah matang", "dibilang bisa bikin horny", "digambar tubuhnya di papan", "dijadikan bahan fantasi", "dibilang cocok jadi bini kedua", "dibilang cocok buat video bokep", "disebarin foto tubuh", "dipalsukan jadi video bokep", "dijadikan meme bokep", "dijadikan karakter mesum", "dibilang alat kelamin longgar", "dibilang suka doggy", "dibilang suka dikangkangin", "dibilang jago di ranjang", "dibilang suka oral", "dibilang pelacur kelas", "dibilang cewek kampung gatel", "dibilang cowok hidung belang", "dibilang cowok mesum", "dibilang doyan coli", "dibilang suka nonton bokep", "dipanggil cewek murahan", "dipanggil perek", "dipanggil jablay", "dipanggil jalang", "dipanggil lonte", "dipanggil homo", "dipanggil bencong", "dipanggil banci", "dipanggil sodomi", "dipanggil gay mesum", "dipanggil pelakor", "dipanggil pezinah", "dipanggil penggoda suami orang"];
  let seksualCount = 0;
  seksualWords.forEach((word) => {
    if (lower.includes(word)) {
      seksualCount++;
      score += 35;
    }
  });
  if (seksualCount > 0) violence.push("Seksual");

  // Deteksi Sentimen
  let sentiment = "Netral";
  const emosiNegatif = ["tidak dianggap", "dijauhi", "dilupakan", "sendiri terus", "tidak punya teman", "tidak ada yang peduli", "diabaikan", "tidak pernah diajak", "ditinggal", "tidak dilibatkan", "dibuang", "tidak dihargai", "tidak berharga", "tidak didengar", "tidak penting", "menghilang", "tidak ada", "semua menjauh", "tidak percaya", "asing", "kesepian", "tidak kuat", "hancur", "menyerah", "nangis tiap hari", "gelap terus", "tidak mengerti", "disalahkan", "paling salah", "tidak tahu harus cerita", "takut sekolah", "cemas", "tidak nyaman", "takut ketemu", "pura-pura kuat", "pura-pura senyum", "pura-pura gak sakit", "baik-baik aja", "rumah gak tenang"];
  let emosiCount = 0;
  emosiNegatif.forEach((word) => {
    if (lower.includes(word)) {
      emosiCount++;
      score += 15;
    }
  });
  if (emosiCount > 0) sentiment = "Negatif";

  let level = "Rendah";
if (score >= 50) level = "Sedang";
if (score >= 100) level = "Tinggi";

  return {
    sentiment: sentiment,
    violence: violence.join(", "),
    score: score
  };
}

