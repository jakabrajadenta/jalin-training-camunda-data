# Jalin Training Camunda Data

Kumpulan file BPMN, DMN, dan Form untuk pelatihan Camunda 8 (Camunda Cloud) di Jalin Pembayaran Nusantara. Repository ini berisi contoh proses bisnis bertahap — dari konsep dasar hingga integrasi Kafka — yang digunakan sebagai bahan praktik selama sesi training.

## Prasyarat

- [Camunda Modeler](https://camunda.com/download/modeler/) versi 5.x ke atas
- Akses ke **Camunda 8 SaaS** (Camunda Cloud) atau instalasi **Camunda 8 Self-Managed**
- Execution Platform: Camunda Cloud 8.7.0+

## Cara Menggunakan

### 1. Buka File di Camunda Modeler

1. Download dan install [Camunda Modeler](https://camunda.com/download/modeler/).
2. Buka file `.bpmn`, `.dmn`, atau `.form` langsung dari Camunda Modeler via **File → Open File**.
3. Pastikan execution platform dikonfigurasi ke **Camunda 8 (Zeebe)** di settings modeler.

### 2. Deploy ke Camunda 8

1. Hubungkan Camunda Modeler ke cluster Camunda 8 via **Deploy Current Diagram**.
2. Masukkan **Cluster URL**, **Client ID**, dan **Client Secret** dari Camunda Console.
3. Klik **Deploy** — proses siap dijalankan dari Camunda Tasklist atau Operate.

### 3. Menjalankan Proses

- Gunakan **Camunda Tasklist** untuk memulai instance proses dan mengerjakan user task.
- Gunakan **Camunda Operate** untuk memonitor dan men-debug instance proses yang berjalan.
- Untuk service task, implementasikan Zeebe job worker yang subscribe ke job type yang sesuai.

---

## Struktur File

File dikelompokkan berdasarkan topik dan hari pelatihan.

### Dasar — Konsep Awal BPMN

| File | Process ID | Deskripsi |
|------|-----------|-----------|
| `BPM_Hello_World.bpmn` | `Hello_BPM` | Proses paling sederhana: satu user task "Hello BPM". Titik awal untuk memahami struktur BPMN. |
| `BPM_Demo_Form.bpmn` | — | Demo penggunaan form di dalam user task. |
| `BPM_Form_Berantai.bpmn` | — | Proses multi-step dengan form yang saling terhubung secara berurutan. |
| `BPM_Multiparty.bpmn` | — | Proses yang melibatkan beberapa pihak/role berbeda dalam satu alur. |
| `BPM_Exclusive_Gateway.bpmn` | `Online_Loan` | Proses pengajuan pinjaman online dengan **exclusive gateway** yang merutekan alur berdasarkan jenis pinjaman (`PP`, `RR`, `KR`) ke divisi yang berbeda (Education / Renovation / House), lalu keputusan approval atau rejection. |
| `BPM_Pengajuan_Pinjaman.bpmn` | `Pengajuan_Pinjaman` | Alur pengajuan pinjaman multi-pengguna: Fill Form (`demo`) → Data Verification (`jaka1`) → Decision (`jaka2`) → Fund Disbursement (`jaka3`). Contoh assignment ke user spesifik. |

**Form pendukung (dasar):**

| File | Deskripsi |
|------|-----------|
| `FORM_Basic_Check.form` | Form pengecekan awal |
| `FORM_Pengajuan.form` | Form pengajuan pinjaman |
| `FORM_Approval_Loan.form` | Form persetujuan pinjaman |
| `FORM_Persetujuan.form` | Form persetujuan umum |
| `FORM_Input_Nama.form` | Form input nama |
| `FORM_Input_Umur.form` | Form input umur |
| `FORM_Input_Alamat.form` | Form input alamat |
| `FORM_Lihat_Data.form` | Form tampilan data |

---

### Day 2 — Gateway, DMN, dan Sub-Process

| File | Deskripsi |
|------|-----------|
| `D2_BPM_Inclusive_Gateway.bpmn` | Contoh **inclusive gateway** — satu atau lebih jalur dapat aktif sekaligus, berbeda dengan exclusive gateway. |
| `D2_BPM_Read_DMN.bpmn` | Integrasi **BPMN + DMN**: business rule task memanggil DMN `DMN_Credit_Score` untuk evaluasi credit score, hasilnya ditampilkan ke user. |
| `D2_BPM_Sub_Process.bpmn` | Penggunaan **sub-process** untuk mengenkapsulasi sekumpulan task dalam satu blok yang dapat dikelola terpisah. |
| `D2_DMN_Credit_Score.dmn` | **Decision table** untuk penilaian credit score. Input: data nasabah; output: kategori kelayakan kredit. |

**Form Day 2:**

| File | Deskripsi |
|------|-----------|
| `D2_Form_Data_Check.form` | Form pengecekan data |
| `D2_Form_Evaluation.form` | Form hasil evaluasi |
| `D2_Form_Interview.form` | Form wawancara |
| `D2_Form_Marriage.form` | Form status pernikahan |
| `D2_Form_Pay_Slip.form` | Form slip gaji |
| `D2_Form_Review.form` | Form review |
| `D2_Form_SIUP.form` | Form SIUP (izin usaha) |
| `D2_Form_Verification.form` | Form verifikasi data |

---

### Day 3 — Service Task dan Integrasi Worker

| File | Deskripsi |
|------|-----------|
| `D3_BPM_Calculate.bpmn` | Proses kalkulasi: Input angka (user task) → **service task** `calculate-value` memproses → hasil ditampilkan ke user. Contoh dasar integrasi Zeebe job worker. |
| `D3_BPM_Customer_Verification.bpmn` | Proses verifikasi data customer dengan kombinasi user task dan service task. |

**Form Day 3:**

| File | Deskripsi |
|------|-----------|
| `D3_Form_Input_Number.form` | Form input angka untuk proses kalkulasi |
| `D3_Form_Result_Number.form` | Form tampilan hasil kalkulasi |
| `D3_Form_Customer_Verification.form` | Form verifikasi customer |

---

### Day 4 — Payment Switching

| File | Deskripsi |
|------|-----------|
| `D4_BPM_Payment_Switching.bpmn` | Simulasi **payment switching**: Fill Transaction Data → **Validate Transaction** (service task) → **Route Transaction** (service task) → **Authorize Transaction** (service task) → Show Result. Merepresentasikan alur switching transaksi pembayaran dengan beberapa tahap otomatis. |

**Form Day 4:**

| File | Deskripsi |
|------|-----------|
| `D4_Form_Fill_Transaction_Data.form` | Form input data transaksi |
| `D4_Form_Transaction_Result.form` | Form tampilan hasil transaksi |

---

### Day 5 — Integrasi Kafka & Routing Process

| File | Deskripsi |
|------|-----------|
| `D5_BPM_Payment_Process.bpmn` | Proses pembayaran dengan **integrasi Kafka**: Payment Data (user task) → Produce Kafka (`send-request`) → Consume Kafka (`wait-result`) → **exclusive gateway** berdasarkan `result` (success/failed) → end event berbeda. |
| `D5_BPM_Routing_Process.bpmn` | Proses routing yang berjalan sebagai consumer/responder dari proses D5_Payment_Process. |

**Form Day 5:**

| File | Deskripsi |
|------|-----------|
| `D5_Form_Payment_Data.form` | Form input data pembayaran |
| `D5_Form_Routing.form` | Form routing transaksi |

---

## Job Types (Service Task)

Untuk menjalankan service task, diperlukan Zeebe job worker yang subscribe ke job type berikut:

| Job Type | Digunakan Oleh | Deskripsi |
|----------|---------------|-----------|
| `calculate-value` | `D3_BPM_Calculate` | Memproses kalkulasi angka |
| `validate-transaction` | `D4_BPM_Payment_Switching` | Validasi data transaksi |
| `route-transaction` | `D4_BPM_Payment_Switching` | Routing transaksi ke tujuan |
| `authorize-transaction` | `D4_BPM_Payment_Switching` | Otorisasi transaksi |
| `send-request` | `D5_BPM_Payment_Process` | Produce pesan ke Kafka |
| `wait-result` | `D5_BPM_Payment_Process` | Consume hasil dari Kafka |

---

## Tools yang Digunakan

- **Camunda Modeler** 5.36.1 — desain BPMN, DMN, dan Form
- **Camunda 8 (Zeebe)** — execution engine
- **Camunda Tasklist** — UI untuk user task
- **Camunda Operate** — monitoring dan observability proses
