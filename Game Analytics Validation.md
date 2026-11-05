# Riset: Hubungan Game Analytics dengan Quality Assurance (QA)

## Pendahuluan

Salah satu hal penting yang perlu dijadikan dasar dalam riset ini adalah bahwa **Game Analytics** dan **Quality Assurance (QA)** memiliki tujuan yang berbeda, tetapi keduanya saling melengkapi dalam menjaga kualitas game.

- **Game Analytics** digunakan untuk memahami apa yang terjadi ketika pemain memainkan game, bagaimana perilaku pemain, serta bagaimana performa fitur berdasarkan data.
- **Quality Assurance (QA)** digunakan untuk memastikan game berjalan sesuai spesifikasi, menemukan defect, memverifikasi perilaku sistem, dan memastikan data yang dihasilkan game memang benar.

Hal yang perlu digarisbawahi adalah bahwa QA tidak otomatis bertanggung jawab atas validitas seluruh hasil analisis data. Tanggung jawabnya bergantung pada jenis validasi yang dilakukan. QA dapat memvalidasi apakah data yang dikirim game sesuai dengan perilaku aktual game, sedangkan Data Analyst lebih berwenang memvalidasi metode analisis dan interpretasi hasilnya.

---

# 1. Hubungan antara Data Analytics dan QA dalam Game Development

## 1.1 Perbedaan Tanggung Jawab

| Aspek | Game Analytics / Data Analyst | Quality Assurance |
|---------|---------|---------|
| Tujuan | Memahami perilaku dan performa game melalui data | Memastikan kualitas dan kesesuaian game |
| Fokus | Data pemain, statistik, tren, pola | Functionality, bug, usability, performance |
| Aktivitas | Mengumpulkan, mengolah, menganalisis data | Testing, reproduksi bug, verifikasi |
| Output | Dashboard, report, KPI, insight | Test report, defect report, test result |
| Pertanyaan Utama | Apa yang terjadi dan mengapa? | Apakah sistem berjalan sesuai ekspektasi? |

### Keterkaitan Analytics dan QA

```text
Game Development
        ↓
Analytics Tracking Implementation
        ↓
Game Analytics Event Collection
        ↓
QA Validation
        ↓
Data Analysis
        ↓
Development Decision
```

Keterangan:

- Programmer mengimplementasikan fitur dan analytics tracking.
- Game mengirimkan event dan parameter berdasarkan aktivitas pemain.
- QA memastikan event yang dikirim sesuai dengan kejadian aktual dalam game.
- Data Analyst mengolah event menjadi metrik, statistik, dan insight.
- Developer dan Game Designer menggunakan insight tersebut untuk pengambilan keputusan.

> Diagram di atas merupakan model alur kerja konseptual, bukan urutan kerja yang wajib diterapkan seluruh studio game.

---

## 1.2 Contoh Hubungan Analytics dan QA

Misalkan developer membuat level bernama `Level 5`.

### Data yang Dicatat Analytics

- Pemain memulai Level 5.
- Pemain menyelesaikan Level 5.
- Pemain gagal menyelesaikan Level 5.
- Waktu yang dibutuhkan untuk menyelesaikan level.
- Penyebab kegagalan (jika diimplementasikan sebagai event).

### Hasil Analytics

| Analytics Output | Hasil |
|----------|----------|
| Total pemain yang mencoba | 2.000 |
| Pemain berhasil | 460 |
| Pemain gagal | 1.540 |
| Success Rate | 23% |
| Failure Rate | 77% |

Misalnya ditemukan bahwa sebagian besar kegagalan berkaitan dengan:

- Damage boss terlalu tinggi.
- Pemain jatuh ke dalam pit.
- Pemain kehabisan waktu.

QA kemudian dapat melakukan investigasi berikut:

| Temuan Analytics | Aktivitas QA | Hasil yang Diharapkan |
|----------|----------|----------|
| Banyak pemain mati karena boss | Menguji damage dan attack pattern boss | Damage sesuai desain |
| Banyak pemain jatuh ke pit | Menguji collision dan batas area | Collision berjalan benar |
| Banyak pemain gagal karena waktu | Menguji timer dan kondisi penyelesaian | Timer sesuai spesifikasi |
| Banyak pemain berhenti di Level 5 | Menguji progression dan kemungkinan softlock | Tidak ada hambatan teknis |

### Catatan Penting

Failure rate sebesar 77% tidak otomatis menunjukkan adanya bug.

Kemungkinan penyebab:

- Level memang dirancang sulit.
- Terdapat masalah balancing.
- Terdapat masalah desain level.
- Terdapat defect teknis.

Analytics menunjukkan pola yang perlu diperiksa, sedangkan QA membantu memverifikasi apakah pola tersebut berasal dari masalah teknis.

---

## 1.3 Manfaat Analytics bagi QA

### 1. Identifikasi Area Berisiko

Analytics dapat menunjukkan fitur, level, atau sistem yang memiliki:

- Failure rate tinggi.
- Error tinggi.
- Abandonment tinggi.

QA dapat memprioritaskan investigasi pada area tersebut.

### 2. Menemukan Masalah yang Sulit Direproduksi

Data analytics dapat membantu QA mempersempit kondisi reproduksi bug yang hanya muncul pada:

- Device tertentu.
- Kondisi jaringan tertentu.
- Kombinasi aktivitas tertentu.

### 3. Mendukung Regression Testing

Setelah bug diperbaiki:

- QA menguji ulang fitur.
- Analytics digunakan untuk membandingkan hasil sebelum dan sesudah perbaikan.

### 4. Validasi Pengalaman Pemain

QA melakukan pengujian terkontrol, sedangkan analytics memberikan gambaran perilaku ribuan pemain pada kondisi nyata.

---

# 2. Penggunaan Game Analytics oleh Developer

## 2.1 Bagaimana Data Dikumpulkan?

Developer biasanya mengintegrasikan SDK atau sistem tracking ke dalam game.

Contoh platform:

- Unity Analytics
- Unity Gaming Services
- GameAnalytics
- Firebase Analytics
- SDK analytics lainnya

### Alur Pengumpulan Data

1. Developer menentukan aktivitas yang ingin dicatat.
2. Developer membuat event dan parameter.
3. Event tracking diimplementasikan melalui kode game dan SDK.
4. Ketika aktivitas terjadi, game mengirimkan data.
5. Platform analytics menerima dan mengolah data.
6. Data ditampilkan dalam dashboard atau laporan.

### Contoh Event

```json
{
  "event": "level_completed",
  "level_id": "level_05",
  "completion_time": 185,
  "player_death_count": 2,
  "build_version": "1.3.1"
}
```

Keterangan:

| Parameter | Fungsi |
|------------|---------|
| event | Aktivitas yang terjadi |
| level_id | Identitas level |
| completion_time | Waktu penyelesaian |
| player_death_count | Jumlah kematian |
| build_version | Versi game |

---

## 2.2 Jenis Data yang Umum Dikumpulkan

| Kategori | Data yang Dikumpulkan | Contoh |
|------------|------------|------------|
| Session | Aktivitas sesi pemain | Session Start, Session End |
| Progression | Perjalanan pemain | Level Start, Fail, Complete |
| Resource | Pergerakan resource | Gold Earned, Gold Spent |
| Business | Transaksi | In-App Purchase |
| Design | Aktivitas gameplay | Tutorial, Skill Usage |
| Ad | Aktivitas iklan | Ad Click, Ad Impression |
| Error | Error dan exception | Crash, Exception |
| Performance | Kondisi teknis | FPS, Memory Usage |

---

## 2.3 Contoh Penggunaan Analytics

### A. Progression Analytics

Digunakan untuk melihat perkembangan pemain.

Contoh:

- Completion Rate
- Failure Rate
- Drop-off Rate
- Average Completion Time

### B. Resource Analytics

Digunakan untuk memantau ekonomi game.

Contoh:

- Total Gold Earned
- Total Gold Spent
- Resource Distribution
- Economy Balance

### C. Design Analytics

Digunakan untuk memahami interaksi pemain terhadap fitur.

Contoh:

- Skill Usage Frequency
- Tutorial Completion
- Weapon Preference
- Feature Usage

### D. Performance Analytics

Digunakan untuk memantau kondisi teknis game.

Contoh:

- Average FPS
- Memory Usage
- Device Performance
- Error Frequency

---

# 3. Bagaimana Memastikan Data Analytics Valid?

Bagian ini merupakan bagian terpenting dalam riset.

Data yang masuk ke dashboard belum tentu benar.

Contoh:

Dashboard menunjukkan bahwa 1.000 pemain menyelesaikan Level 5.

Namun ternyata event `level_completed` juga terkirim ketika pemain baru memasuki level.

Secara teknis:

✅ Data berhasil dikirim

✅ Data tersimpan

Tetapi:

❌ Data tidak merepresentasikan kejadian sebenarnya

Karena itu validasi analytics tidak cukup hanya memastikan bahwa event berhasil dikirim.

---

## 3.1 Enam Dimensi Kualitas Data

| Dimensi | Penjelasan | Contoh dalam Game |
|----------|----------|----------|
| Accuracy | Data sesuai kejadian aktual | Jumlah kematian sesuai kejadian |
| Completeness | Tidak ada data wajib yang hilang | Semua completion memiliki level ID |
| Consistency | Format data konsisten | Format level ID seragam |
| Validity | Sesuai aturan dan batasan | FPS tidak negatif |
| Timeliness | Data tersedia tepat waktu | Event muncul tanpa delay tidak wajar |
| Uniqueness | Tidak ada duplikasi | Completion tidak tercatat dua kali |

---

## 3.2 Siapa yang Bertanggung Jawab Memvalidasi Data?

Tidak ada satu posisi yang bertanggung jawab terhadap seluruh validitas Game Analytics.

| Posisi | Tanggung Jawab |
|----------|----------|
| Game Designer / Product Owner | Mendefinisikan event |
| Game Programmer | Implementasi event |
| QA Tester | Verifikasi event terhadap gameplay |
| Data Engineer | Validasi pipeline dan integritas data |
| Data Analyst | Validasi analisis dan interpretasi |
| QA Lead / Manager | Validasi proses QA |

### Pembagian Tanggung Jawab

1. Game Designer menentukan expected behavior.
2. Programmer mengimplementasikan tracking.
3. QA memverifikasi implementasi.
4. Data Engineer memvalidasi pipeline.
5. Data Analyst memvalidasi hasil analisis.

Ringkasnya:

- QA → memvalidasi data yang dihasilkan game.
- Data Engineer → memvalidasi pemrosesan data.
- Data Analyst → memvalidasi analisis data.
- Game Designer → memvalidasi kesesuaian dengan tujuan desain.

---

## 3.3 Bagaimana QA Memvalidasi Game Analytics?

QA dapat menggunakan pendekatan **Black Box Testing**.

Contoh test case:

### Skenario: Validasi Event Penyelesaian Level

| Tahapan | Expected Result |
|----------|----------|
| Masuk Level 5 | level_started terkirim 1 kali |
| Mengalahkan boss | boss_defeated tercatat |
| Menyelesaikan level | level_completed terkirim 1 kali |
| Mengulang level | Event baru tercatat |
| Keluar sebelum selesai | Tidak ada level_completed |

### Hasil Verifikasi

| Data | Expected | Actual |
|---------|---------|---------|
| Level Started | 1 | 1 |
| Boss Defeated | 1 | 1 |
| Level Completed | 1 | 2 |
| Duplicate Event | 0 | 1 |

**Status: FAIL**

Terjadi duplikasi event `level_completed`.

QA dapat membuat defect report karena event tidak sesuai dengan aktivitas aktual pemain.

---

## 3.4 Contoh Validasi Menggunakan Unity Analytics

Unity menyediakan fitur **Event Browser** untuk:

- Memeriksa event yang dikirim.
- Melihat event yang gagal validasi.
- Mengetahui penyebab event ditolak.
- Memeriksa payload event.

### Hal yang Dapat Divalidasi

- Nama parameter sesuai schema.
- Parameter bersifat case-sensitive.
- Tipe data sesuai definisi.
- Event tidak melanggar aturan backend.

Namun:

> Lolos validasi schema tidak otomatis membuktikan bahwa event merepresentasikan gameplay yang benar.

---

# 4. Apa Output dari Game Analytics?

| Output | Penjelasan | Contoh |
|----------|----------|----------|
| Dashboard | Visualisasi kondisi game | Active Players |
| KPI / Metrics | Indikator performa | Retention Rate |
| Funnel Analysis | Alur pemain | Tutorial → Level 1 |
| Cohort Analysis | Segmentasi waktu | Retention berdasarkan tanggal instalasi |
| Segmentation | Kelompok pemain | Casual vs Hardcore |
| Performance Report | Kondisi teknis | FPS, Memory |
| Error Report | Error dan crash | Crash Frequency |
| Monetization Report | Pendapatan | Revenue, Conversion Rate |

---

# 5. Korelasi Hasil Game Analytics dengan Pelaporan QA

Analytics tidak seharusnya langsung menjadi bug report.

Perlu tiga tahapan:

1. Analytics Finding
2. QA Investigation
3. QA Defect Report

---

## 5.1 Contoh Hubungan Analytics Report dan QA Report

### Analytics Finding

| Metric | Result |
|----------|----------|
| Total Attempts | 2.000 |
| Successful Attempts | 460 |
| Failed Attempts | 1.540 |
| Completion Rate | 23% |

### Analisis

Sebagian besar kegagalan terjadi saat melawan boss Level 5.

### Investigasi QA

| Test ID | Skenario | Hasil |
|----------|----------|----------|
| QA_001 | Damage Boss | Pass |
| QA_002 | Boss Collision | Fail |
| QA_003 | Player Death | Pass |
| QA_004 | Level Completion | Pass |

### Defect Ditemukan

**Boss Attack Hitbox Exceeds Visual Range**

Deskripsi:

Hitbox serangan boss lebih besar dibanding visual animasi serangannya sehingga pemain menerima damage meskipun terlihat berada di luar jangkauan.

Analytics menjadi dasar investigasi, sedangkan defect report tetap dibuat berdasarkan hasil pengujian QA.

---

## 5.2 Jika Analytics Tidak Menemukan Defect

Contoh:

- Completion rate Level 5 hanya 23%.
- QA menguji seluruh mekanik.
- Tidak ditemukan bug teknis.
- Semua sistem berjalan sesuai spesifikasi.

Hasil:

| Investigasi | Status |
|------------|------------|
| Boss Damage | Pass |
| Boss Collision | Pass |
| Player Death | Pass |
| Level Completion | Pass |
| Technical Defect | Not Found |

Kesimpulan:

Level mungkin terlalu sulit, tetapi tidak terdapat defect teknis.

Kasus seperti ini menjadi tanggung jawab Game Designer untuk mengevaluasi balancing.

---

# 6. Alur Kerja yang Direkomendasikan

1. **Planning & Event Definition**
   - Output: Tracking Plan

2. **Analytics Implementation**
   - Output: Implemented Analytics Events

3. **Analytics Validation Testing**
   - Output: Analytics Test Report

4. **Data Pipeline Validation**
   - Output: Data Quality Check

5. **Analytics Processing**
   - Output: Dashboard & Analytics Report

6. **Investigation & Improvement**
   - Output: Bug Fixes, Balancing Changes, Regression Test

---

## 6.1 Dokumen yang Sebaiknya Tersedia

| Dokumen | Pemilik Utama | Fungsi |
|----------|----------|----------|
| Analytics Tracking Plan | Designer / Product | Mendefinisikan event |
| Event Dictionary | Developer / Data Team | Definisi parameter |
| Analytics Test Case | QA | Skenario validasi |
| Analytics Test Report | QA | Dokumentasi hasil |
| Data Quality Report | Data Engineer | Integritas data |
| Analytics Report | Data Analyst | Insight bisnis |
| Defect Report | QA | Pelaporan defect |

---

# 7. Kesimpulan untuk PIC

1. Game Analytics digunakan untuk memahami perilaku pemain dan kondisi teknis game.
2. Developer bertanggung jawab mengimplementasikan sistem tracking.
3. QA bertanggung jawab memverifikasi kesesuaian event dengan perilaku aktual game.
4. Data Engineer bertanggung jawab menjaga kualitas dan integritas data.
5. Data Analyst bertanggung jawab melakukan analisis dan interpretasi data.
6. Analytics dapat membantu QA menentukan prioritas investigasi dan pengujian.
7. Tidak semua anomali analytics merupakan bug dan harus diverifikasi terlebih dahulu.

## Rekomendasi Posisi QA

QA sebaiknya:

- Tidak bertanggung jawab mengumpulkan raw telemetry secara mandiri.
- Tidak bertanggung jawab membangun sistem analytics.
- Tidak bertanggung jawab menentukan validitas seluruh metode statistik.

QA sebaiknya menerima:

- Event Specification
- Tracking Plan
- Expected Result

Kemudian memverifikasi implementasinya melalui testing.

Dengan pembagian ini, QA memberikan kontribusi langsung terhadap keandalan Game Analytics tanpa mengambil alih tanggung jawab Developer maupun Data Analyst.

---

# 8. Saran Fokus Riset

Agar riset tidak terlalu melebar, fokus pada tiga pertanyaan utama:

## 1. Siapa yang Menentukan Data yang Dikumpulkan?

**Jawaban:**

- Game Designer / Product Owner menentukan kebutuhan bisnis.
- Programmer mengimplementasikan tracking.
- Data Analyst memberikan masukan kebutuhan analisis.

## 2. Siapa yang Memastikan Data Tersebut Benar?

**Jawaban:**

- QA memvalidasi event terhadap gameplay.
- Data Engineer memvalidasi pipeline.
- Data Analyst memvalidasi analisis dan perhitungan.

## 3. Bagaimana Analytics Berhubungan dengan QA Testing Report?

**Jawaban:**

Analytics memberikan indikasi masalah berdasarkan perilaku pemain dalam skala besar.

QA menggunakan indikasi tersebut untuk:

- Menentukan area investigasi.
- Melakukan reproduksi masalah.
- Menyusun defect report bila ditemukan ketidaksesuaian.

> Nilai utama riset ini bukan hanya menjelaskan Game Analytics, tetapi menunjukkan bagaimana Analytics dapat dimanfaatkan oleh QA tanpa mencampuradukkan tanggung jawab QA, Developer, Data Engineer, dan Data Analyst.
