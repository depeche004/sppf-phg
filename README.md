# SPPF-PHG: Sistem Pertukaran Pegawai Farmasi Pahang

**No. Pendaftaran NMRR:** NMRR ID-26-03968-MPU

Platform pengurusan sumber manusia dan pemarkahan objektif tanpa pelayan (serverless) yang dibangunkan untuk Jabatan Kesihatan Negeri Pahang (JKNP). Sistem ini mendigitalkan proses permohonan dan penilaian pertukaran anggota farmasi dengan menggunakan rubrik pemarkahan 45 mata yang telah disahkan melalui kajian Modified Delphi.

## Ciri-Ciri Utama
*   **Enjin Pemarkahan Objektif:** Pengiraan dan tafsiran masa nyata bagi rubrik 45 mata yang merangkumi beban logistik peribadi, kekangan keluarga, dan implikasi operasi.
*   **Borang Dinamik (Cascading Forms):** Pilihan fasiliti pintar yang beradaptasi secara automatik mengikut kategori Hospital, Pejabat Kesihatan Daerah (PKD), dan Ibu Pejabat (BPF).
*   **Papan Pemuka Pentadbir (Live Dashboard):** Jadual data dinamik yang dilengkapi dengan fungsi susunan (sorting), tapisan (berdasarkan Tahun dan PTj), serta fungsi CRUD untuk mengemas kini status permohonan.
*   **Arkitektur Tanpa Pelayan (Serverless):** 100% *frontend* tanpa pelayan yang direka untuk Cloudflare Pages, menggunakan Google Sheets sebagai pangkalan data yang selamat melalui integrasi Google Apps Script (GAS).

## Susunan Teknologi (Tech Stack)
*   **Frontend:** HTML5, CSS3, Vanilla JavaScript (Arkitektur Single Page Application)
*   **Backend / API:** Google Apps Script (RESTful POST/GET endpoints)
*   **Pangkalan Data:** Google Sheets
*   **Hosting Web:** Cloudflare Pages 

## Panduan Pemasangan & Penggunaan

### 1. Pangkalan Data & Backend (Google Workspace)
1. Cipta fail Google Sheet baharu bernama `Database Pertukaran SPPF`.
2. Tetapkan tajuk lajur pada Baris 1 dari Lajur A hingga X (ID, Tarikh, Nama, KP, Gred, PTj Semasa, Fasiliti Semasa, PTj Diminta, Fasiliti Diminta, Skor P1-P9, Jumlah Skor, Rekod Tatatertib, Latihan Pegawai, Dokumen Sokongan, Ulasan Ketua Jabatan, Status).
3. Buka **Extensions > Apps Script**.
4. Tampal kod Google Apps Script (fungsi `doPost` dan `doGet`) untuk mengendalikan data JSON.
5. Klik **Deploy > New Deployment**.
6. Pilih **Web app**, tetapkan akses kepada **Anyone**, dan sahkan skrip tersebut.
7. Salin URL Web App yang dijana.

### 2. Pemasangan Frontend (Cloudflare Pages)
1. Klon repositori ini.
2. Buka fail `index.html` dan cari pemboleh ubah `SCRIPT_URL`.
3. Gantikan nilai tersebut dengan URL Web App Google anda.
4. Muat naik atau sambungkan repositori ini ke Cloudflare Pages untuk penyiaran (deployment) segera.

## Manual Pengguna
*   **Penilaian KJF/PFK:** Penilai memilih skala 1-5 berdasarkan semakan dokumen fizikal. Sistem akan mentafsir skor secara automatik:
    *   **9 - 11:** Permohonan tidak disokong.
    *   **12 - 22:** Ditangguhkan untuk penilaian semula.
    *   **23 - 34:** Disokong (berkuatkuasa > 3 bulan).
    *   **35 - 45:** Disokong (serta-merta).
*   **Akses Papan Pemuka:** Dilindungi menggunakan kata laluan keselamatan berpusat bagi membolehkan pentadbir peringkat negeri menyemak dan mengemas kini keputusan permohonan.

## Privasi & Keselamatan Data
Semua data anggota (No. Kad Pengenalan, demografi, dan profil kesihatan keluarga) dikodkan dan disimpan di dalam pelayan awan Google Workspace KKM yang disulitkan. Antara muka sistem tidak menyimpan sebarang data secara tempatan (cache), memastikan pematuhan penuh kepada garis panduan Jawatankuasa Etika dan Penyelidikan Perubatan (MREC).

## Lesen & Hak Cipta
© 2026 Jabatan Kesihatan Negeri Pahang (JKNP). Hak Cipta Terpelihara.
Dibangunkan oleh Muhammad Hafizul Aniq bin Khairul Hafidz, RPh. 25588.

Sistem ini merupakan aset hak milik kerajaan yang dibangunkan khusus untuk pengurusan rasmi sumber manusia. Sebarang pengedaran tanpa kebenaran atau manipulasi kerangka pemarkahan adalah dilarang sama sekali.
