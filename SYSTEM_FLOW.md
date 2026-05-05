# 🔄 System Flow: Registration, Scheduling & Hauling Validation

Dokumentasi ini merinci logika teknis di balik sistem **HaulFlow**, mulai dari manajemen data entitas hingga proses validasi fisik di lapangan.

---

## 1. Alur Pra-Operasional (Registration & Scheduling)

Sebelum operasional dimulai, sistem memastikan bahwa semua entitas telah terverifikasi dan jadwal terdistribusi dengan efisien.

### A. Registrasi Entitas (Onboarding)
Untuk menjaga keamanan data, setiap unit harus terdaftar di database pusat:
*   **Driver & Vehicle**: Identitas driver (SIM/Profil) dan nomor polisi kendaraan harus terdaftar agar dapat melakukan "Verifikasi Diri" di jembatan timbang.
*   **Role Management**: Penentuan hak akses antara Admin (pembuat jadwal) dan Driver (pelaksana lapangan).

### B. Mekanisme Pemilihan Jadwal (Open Assignment)
Sistem memberikan fleksibilitas kepada Driver dengan metode pemilihan mandiri:
1.  **Admin** mengunggah daftar jadwal kerja (Shift & Rute).
2.  **Driver** mengakses aplikasi untuk melihat daftar slot yang tersedia.
3.  **Driver** memilih jadwal yang sesuai; sistem secara otomatis mengunci slot tersebut untuk mencegah duplikasi penugasan (*Double Booking Prevention*).

---

## 2. Diagram Alur Operasional (End-to-End)

Diagram berikut menggambarkan transisi data dari pemilihan jadwal hingga tugas dinyatakan selesai.

```mermaid
sequenceDiagram
    participant A as Admin
    participant S as HaulFlow System
    participant D as Driver (Mobile App)
    participant P as Security Post (Pos)
    participant W as Weighbridge (Timbangan)

    Note over A, D: 1. Penjadwalan & Seleksi
    A->>S: Input Daftar Jadwal/Shift
    D->>S: Lihat & Pilih Jadwal Tersedia
    S->>D: Konfirmasi Jadwal Terkunci

    Note over D, P: 2. Perjalanan & Checkpoint
    loop Tiap Pos Pemeriksaan
        D->>P: Tiba di Pos
        D->>S: Ambil Foto Checkpoint (Real-time)
        S->>S: Update Progres Perjalanan
    end

    Note over D, W: 3. Verifikasi & Penimbangan
    D->>W: Masuk ke Jembatan Timbang
    D->>S: Verifikasi Diri Singkat (Konfirmasi Identitas)
    W->>S: Kirim Data Berat Muatan (Netto)
    S->>S: Kunci Data & Selesai (Completed)
