# Memahami Pola Serangan Siber melalui Analisis Log Keamanan: Insight Data-Driven untuk Strategi Perlindungan Organisasi dengan Model Granite Instruct dari IBM

## Project Overview  
Di era digital saat ini, banyak organisasi — termasuk startup dan perusahaan besar — mulai melakukan digitalisasi terhadap sistem operasional mereka. Meskipun digitalisasi membawa efisiensi dan kemudahan, hal ini juga membuka celah bagi meningkatnya risiko ancaman siber, terutama ketika sistem mulai terhubung dengan jaringan publik.

Sebagai bentuk mitigasi, proyek ini memanfaatkan Large Language Model (LLM), khususnya **Granite Instruct dari IBM**, untuk melakukan **analisis pola serangan siber melalui log keamanan jaringan**. Log keamanan merupakan komponen penting dalam sistem keamanan organisasi, namun sering kali berukuran sangat besar dan kompleks sehingga menyulitkan proses audit secara manual.

Melalui pendekatan berbasis LLM, proyek ini bertujuan untuk menjawab beberapa pertanyaan berikut ini:
- Apa jenis ancaman yang paling dominan dalam sistem kami?
- Jalur atau aset sistem mana yang paling sering diserang oleh ancaman?
- Pada jam berapa ancaman paling sering terjadi dalam sistem kami?
- Apa insight strategis yang dapat digunakan untuk pengambilan keputusan keamanan organisasi berdasarkan hasil analisis ini?

Dataset yang digunakan adalah **Cybersecurity Threat Detection Logs**, sebuah dataset simulasi yang bersifat publik dan tersedia di platform Kaggle. Dataset ini meniru karakteristik log asli dari sistem deteksi ancaman dan lalu lintas jaringan, mencakup informasi seperti alamat IP sumber dan tujuan, protokol, tindakan sistem (blocked/allowed), serta label ancaman (benign, suspicious, malicious). Penggunaan dataset sintetis ini memungkinkan eksplorasi kemampuan LLM dalam lingkungan yang aman dan terkendali, tanpa melibatkan data sensitif dari organisasi sebenarnya.

## Raw dataset link  
- **Nama Dataset:** Cybersecurity Threat Detection Logs  
- **Sumber:** [Kaggle – Cybersecurity Threat Detection Logs](https://www.kaggle.com/datasets/aryan208/cybersecurity-threat-detection-logs)  
- **Deskripsi:** Dataset sintetis ini berisi sekitar **6 juta entri log** yang dirancang untuk mensimulasikan lalu lintas jaringan dan peristiwa keamanan siber dalam lingkungan organisasi. Setiap entri mencakup informasi seperti protokol komunikasi (TCP, UDP, ICMP, HTTP, HTTPS), tindakan keamanan (`allowed`/`blocked`), klasifikasi ancaman (`benign`, `suspicious`, `malicious`), serta metadata penting seperti alamat IP sumber dan tujuan, *user agent*, dan *request path*.
- **Penggunaan dalam Proyek:**  
  Untuk keperluan percobaan analisis menggunakan model LLM (Granite Instruct IBM), diambil **sampel sebanyak 3.000 baris log** dari total 6 juta data. Jumlah ini dipilih agar representatif namun tetap efisien untuk dianalisis menggunakan LLM dengan batasan token input.
## Insight & findings  
### Insight 1: Dominasi Jenis Ancaman
Distribusi label ancaman dari 3.000 baris log menunjukkan hasil sebagai berikut:

- **benign:** 2.767 (92,2%)
- **suspicious:** 165 (5,5%)
- **malicious:** 68 (2,3%)

**Temuan Utama:**

- **Mayoritas Aktivitas Tidak Mengancam:** Sebagian besar log (lebih dari 92%) diklasifikasikan sebagai `benign`, yang berarti aktivitas jaringan terpantau normal dan tidak menunjukkan perilaku mencurigakan. Hal ini mengindikasikan bahwa sistem keamanan organisasi bekerja dengan baik dalam menyaring lalu lintas umum.

- **Ancaman Tetap Ada di Jumlah Kecil:** Meski jumlahnya kecil, log `suspicious` dan `malicious` tetap menjadi perhatian penting. Keduanya dapat merepresentasikan upaya akses tidak sah, eksploitasi celah, atau aktivitas berbahaya lainnya.

- **Rasio Noise:** Rasio yang tinggi dari log benign juga menunjukkan tantangan umum dalam keamanan siber, yaitu tingginya *"noise"* data. Ini mempertegas pentingnya alat bantu otomatis (seperti LLM) untuk mengidentifikasi ancaman nyata di tengah ribuan data rutin.

### **Rekomendasi:**   
Organisasi perlu terus melakukan pemantauan berkelanjutan dan meningkatkan kemampuan deteksi, terutama untuk kategori `suspicious` yang berpotensi menjadi insiden besar jika diabaikan.
___

### Insight 2: Jalur Sistem yang Paling Sering Diserang

Berdasarkan frekuensi akses dalam log keamanan, berikut adalah 10 *request path* yang paling sering muncul:

`/` → 1371 kali  
`/login` → 219 kali  
`/admin/config` → 125 kali  
`/secure` → 113 kali  
`/auth` → 111 kali  
`/wp-login.php` → 103 kali  
`/api/login` → 99 kali  
`/index.php` → 95 kali  
`/admin` → 91 kali  
`/api/v1/data` → 90 kali  

**Temuan Utama:**

- **Endpoint Umum Jadi Target:** Jalur seperti `/login`, `/admin`, `/auth`, dan `/api/login` merupakan endpoint standar untuk autentikasi dan administrasi sistem. Endpoint ini sering menjadi sasaran utama brute force attack dan eksploitasi akses tidak sah.

- **Target Spesifik WordPress:** Jalur `/wp-login.php` menunjukkan kemungkinan adanya serangan otomatis terhadap sistem berbasis WordPress, yang dikenal memiliki celah keamanan jika tidak diperbarui dengan benar.

- **API Rentan Diserang:** Jalur seperti `/api/login` dan `/api/v1/data` menunjukkan bahwa API juga menjadi titik rawan yang kerap dicoba diakses oleh pelaku ancaman untuk mencuri data atau menyusup ke sistem backend.

### **Rekomendasi:** 
Organisasi disarankan untuk menerapkan proteksi tambahan seperti:
  - Multi-factor authentication (MFA),
  - Rate-limiting dan IP blocking,
  - Validasi akses API,
  - Pengamanan endpoint administratif agar tidak dapat diakses publik.
### **Rekomendasi Perlindungan Tingkat Tinggi:**
- Implementasi Rate Limiting: Batasi jumlah percobaan login per IP pada endpoint seperti `/login dan /wp-login.php` untuk mencegah brute force attack.
- Perkuat Mekanisme Autentikasi: Terapkan autentikasi yang kuat dan Multi-Factor Authentication (MFA) pada semua endpoint sensitif, khususnya `/login` dan endpoint API seperti `/api/login`. MFA menambahkan lapisan keamanan ekstra jika kredensial dicuri.
- Audit dan Update Berkala: Lakukan audit keamanan secara rutin dan pastikan semua perangkat lunak, termasuk CMS seperti WordPress, selalu diperbarui untuk menutup celah keamanan yang diketahui.

Temuan ini mengindikasikan bahwa pelaku ancaman cenderung menargetkan jalur login dan konfigurasi, sehingga proteksi di level aplikasi menjadi sangat krusial.
___
### Insight 3: Profil Penyerang – Source IPs & User Agents

Berdasarkan data dari log, berikut adalah 10 alamat IP sumber dan user agent yang paling sering muncul:  
`192.168.1.242`      → 16 kali  
`98.153.120.136`     → 16 kali  
`245.200.237.136`    → 15 kali  
`109.9.8.24`         → 15 kali  
`181.18.12.170`      → 15 kali  
`216.197.199.15`     → 15 kali  
`42.119.98.70`       → 14 kali  
`221.28.64.50`       → 14 kali  
`192.168.1.130`      → 14 kali  
`93.225.253.213`     → 14 kali  

**Top User Agent:**  
`Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/117.0.0.0 Safari/537.36`    → 620 kali  
`SQLMap/1.6-dev`  → 610 kali  
`Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36`   → 596 kali  
`curl/7.64.1` → 596 kali  
`Nmap Scripting Engine`  → 578 kali  

**Temuan Utama:**

- **IP dan User Agent Mencurigakan:**
  - **Alamat IP internal yang terdeteksi di luar:** IP seperti `192.168.1.242` dan `98.153.120.136` muncul cukup sering di log. IP internal (`192.168.x.x`) seharusnya tidak muncul di log eksternal, yang menunjukkan adanya kemungkinan ancaman internal atau perangkat yang salah konfigurasi.
  - **User Agents Otomatis dan Alat Pemindai:** User agents seperti `SQLMap/1.6-dev` dan `Nmap Scripting Engine` adalah alat yang digunakan untuk eksploitasi otomatis, yang menunjukkan bahwa sistem sedang dipindai atau diserang secara otomatis oleh bot atau alat pemindai.

- **Potensi Ancaman dari Serangan Terautomasi:**
  - User agent `SQLMap` adalah alat yang digunakan untuk mengeksploitasi celah SQL injection, yang bisa sangat merusak jika tidak terdeteksi dengan baik.
  - Alat seperti `Nmap` digunakan untuk memetakan sistem dan mencari potensi kerentanannya, yang biasanya dilakukan oleh penyerang untuk merencanakan langkah selanjutnya.

### **Rekomendasi Perlindungan Tingkat Tinggi:**
- **Segmentasi Jaringan dan Kontrol Akses:** Perkuat segmentasi jaringan untuk memisahkan jaringan internal dari akses eksternal. Terapkan kontrol akses yang lebih ketat dan tinjau izin untuk IP internal agar mencegah akses tidak sah.
- **Implementasi Web Application Firewall (WAF):** Pasang WAF untuk mengurangi risiko serangan SQL injection dengan menyaring dan memantau lalu lintas HTTP antara aplikasi web dan Internet. Secara rutin perbarui aturan WAF untuk melawan ancaman yang baru.
- **Penilaian Kerentanannya Rutin dan Manajemen Patch:** Lakukan pemindaian kerentanan secara berkala dan terapkan patch segera untuk meminimalkan paparan terhadap kerentanannya yang diketahui dan sering menjadi target alat pemindai otomatis.
- **Sistem Deteksi/Pencegahan Intrusi (IDS/IPS):** Implementasikan sistem IDS/IPS untuk mendeteksi dan mencegah aktivitas berbahaya sebelum dapat mengakses sistem lebih jauh.

---
### Insight 4: Pola Waktu Serangan

Distribusi ancaman berdasarkan jam menunjukkan bahwa **jam 0 (00:00 - 01:00)** memiliki angka tertinggi, dengan **233 kejadian ancaman** tercatat dalam satu jam tersebut.

**Temuan Utama:**

- **Jam Ancaman Terpusat:** Data menunjukkan konsentrasi ancaman yang tinggi pada **jam 0**, dengan 233 kejadian ancaman (suspicious atau malicious) pada jam tersebut. Ini jauh lebih tinggi dibandingkan dengan rata-rata kejadian per jam lainnya, yang mengindikasikan bahwa serangan terfokus pada jam tersebut.

- **Pola Waktu yang Bisa Diprediksi:** Pola waktu ini mungkin disebabkan oleh beberapa faktor. Salah satunya adalah penyerang cenderung menargetkan sistem saat jam tidak sibuk, ketika pemantauan lebih sedikit, dan celah keamanan dapat dimanfaatkan. Selain itu, bisa juga serangan terotomatisasi yang mengikuti jadwal tertentu atau dipicu oleh kejadian eksternal (misalnya serangan global yang terkoordinasi).

---

### **Rekomendasi Perlindungan Tingkat Tinggi:**

- **Jadwal Pemantauan yang Ditingkatkan:** Organisasi perlu mempertimbangkan untuk menyesuaikan jadwal pemantauan untuk meningkatkan kewaspadaan selama jam-jam risiko tinggi ini, memastikan staf yang cukup tersedia dan siap merespons dengan cepat.

- **Sistem Peringatan Canggih:** Implementasikan atau perbaiki sistem peringatan untuk memprioritaskan dan memberi notifikasi kepada tim keamanan tentang aktivitas mencurigakan selama jam-jam ancaman puncak, dengan mempertimbangkan penggunaan machine learning untuk memprediksi dan mengidentifikasi pola anomali.

- **Ketersediaan Tim Respons yang Fleksibel:** Pastikan tim respons keamanan siap sedia atau memiliki jam kerja fleksibel selama periode berisiko tinggi untuk meminimalkan waktu respons terhadap insiden keamanan.

- **Pembaruan Intelijen Ancaman Berkala:** Tetap terinformasi dengan tren dan pola ancaman global untuk mengantisipasi dan mempersiapkan serangan yang terkoordinasi. Sesuaikan pertahanan dan strategi untuk menghadapi ancaman yang baru.

- **Latihan Simulasi:** Lakukan serangan simulasi secara rutin selama jam-jam puncak ancaman untuk menguji efektivitas postur keamanan saat ini, protokol respons, dan kesiapan tim, memungkinkan perbaikan yang berkelanjutan.

Dengan analisis pola waktu serangan ini, organisasi dapat meningkatkan kesiapsiagaan dan efisiensi respons terhadap ancaman yang terjadi pada waktu-waktu tertentu.
___
### Insight 5: Evaluasi Efektivitas Sistem Keamanan

Berdasarkan distribusi hasil tindakan sistem terhadap jenis ancaman, berikut adalah ringkasan dari analisis **Action vs Threat**:

| Action  | benign | malicious | suspicious |
| ------- | ------ | --------- | ---------- |
| allowed | 1395   | 30        | 79         |
| blocked | 1372   | 38        | 86         |


**Temuan Utama:**

- **Efektivitas Sistem Keamanan:**  
  Sistem keamanan berhasil **memblokir 96,5%** (1372/1421) dari aktivitas berbahaya (`malicious`), yang menunjukkan respons yang kuat terhadap ancaman potensial.

- **Masalah Deteksi:**  
  Meskipun banyak ancaman berhasil diblokir, terdapat beberapa **ancaman yang salah diklasifikasikan**, seperti:
  - **30 aktivitas benign** yang salah dikategorikan sebagai `malicious` (false positives), yang mewakili 2,1% dari total klasifikasi `malicious`.
  - **86 aktivitas suspicious** yang diizinkan, menunjukkan kemungkinan adanya **kelemahan dalam mendeteksi dan menangani ancaman yang lebih ambigu**.

- **Efisiensi Sistem:**  
  Secara keseluruhan, efisiensi sistem keamanan tergolong tinggi, mengingat sebagian besar serangan `malicious` berhasil diblokir. Namun, **akurasi perlu ditingkatkan** untuk mengurangi false positives dan meningkatkan kemampuan dalam mendeteksi ancaman yang lebih samar.

---

### **Rekomendasi Perlindungan Tingkat Tinggi:**

- **Implementasi Algoritma Pembelajaran Mesin:**  
  Terapkan algoritma pembelajaran mesin untuk **menyempurnakan klasifikasi ancaman**, mengurangi tingkat false positives, dan meningkatkan deteksi terhadap aktivitas yang mencurigakan.

- **Proses Review Rutin Terhadap Ancaman yang Salah Klasifikasi:**  
  Bangun proses tinjauan rutin terhadap ancaman yang salah diklasifikasikan (baik false positives maupun ancaman yang terlewat) untuk **menyempurnakan sensitivitas sistem** dan menyesuaikan dengan ancaman yang berkembang.

Dengan rekomendasi ini, organisasi dapat meningkatkan **akurasi sistem deteksi ancaman** dan memperbaiki **respons terhadap ancaman yang lebih ambigu**, sehingga mengurangi risiko keamanan secara keseluruhan.


## AI support explanation  
Dalam proyek ini, kami menggunakan model **Granite Instruct** dari IBM, sebuah **Large Language Model (LLM)** yang dirancang untuk menganalisis dan menghasilkan **ringkasan (summarization)** dari data besar dan kompleks. Teknik **summarization** ini memungkinkan kami untuk mengolah ribuan entri log menjadi wawasan yang lebih mudah dipahami dan dapat langsung digunakan dalam pengambilan keputusan strategis.

### Penggunaan Teknik Summarization dalam Proyek:

1. **Ringkasan Data Log Keamanan:**
   Dataset yang digunakan mengandung lebih dari 6 juta entri log yang mencatat berbagai aktivitas jaringan dan ancaman siber. Untuk menyederhanakan dan mempercepat analisis, kami melakukan **pra-pemrosesan dan agregasi** data, kemudian menggunakan **Granite LLM** untuk **membuat ringkasan** dari temuan-temuan utama. AI menyaring data log yang besar, menyatukan informasi penting, dan menghasilkan insight berbasis pola yang ada.

2. **Summarization untuk Insight Bisnis:**
   Setelah data dianalisis dan dikelompokkan dalam kategori tertentu (misalnya ancaman, jalur yang paling sering diserang, dan IP sumber yang mencurigakan), **Granite LLM** digunakan untuk **menyaring dan merangkum** hasil-hasil tersebut menjadi ringkasan yang mudah dipahami. Dengan menggunakan teknik **summarization**, AI memberikan penjelasan yang lebih **terstruktur dan jelas**, sehingga manajemen bisa dengan cepat memahami keadaan dan mengambil tindakan yang tepat.

3. **Penyaringan dan Penyusunan Rekomendasi:**
   LLM tidak hanya menyarikan data statistik, tetapi juga menyarikan **rekomendasi berbasis data** yang relevan. AI digunakan untuk menghasilkan saran-saran strategis yang terfokus pada pengurangan risiko dan penguatan pertahanan keamanan, berdasarkan hasil **summarization** yang didapat dari analisis data log.

4. **Efisiensi Waktu dan Akurasi:**
   Teknik **summarization** memungkinkan kami untuk mengurangi **waktu pemrosesan** yang diperlukan untuk menganalisis ribuan baris log secara manual. Dengan memanfaatkan AI untuk mengurangi volume data yang perlu dibaca dan diproses, **Granite LLM** memastikan **akurasi** dalam mengidentifikasi pola-pola ancaman yang penting, serta menyajikan **informasi yang lebih relevan** dan **fokus** pada rekomendasi keamanan yang bisa langsung diterapkan.

### Kesimpulan:
Teknik **summarization** yang diterapkan dengan **Granite LLM** memungkinkan kami untuk **menyaring informasi penting** dari dataset yang sangat besar dan mengonversinya menjadi **ringkasan strategis** yang mudah dipahami. Ini meningkatkan efisiensi proses analisis, **mengurangi beban manual**, dan membantu dalam membuat keputusan keamanan yang lebih cepat dan tepat.
