# 🔄 System Flow: Registration, Scheduling & Hauling Validation

[English Version](#english) | [Versi Bahasa Indonesia](#bahasa-indonesia)

---

<a name="english"></a>
## English Version

### 1. Pre-Operational Flow: Registration & Onboarding
Before the hauling process begins, the system establishes a secure foundation through entity registration to ensure every data point in the field is traceable.

*   **Driver & Vehicle Onboarding**: Drivers must register their legal identity (License/ID) and vehicle license plates. This database serves as the master reference for the "Self-Verification" process at the weighbridge.
*   **Role-Based Access Control (RBAC)**: Distinct permissions for Fleet Admins (schedule management) and Drivers (task execution).

### 2. Scheduling Mechanism: Open Assignment
HaulFlow utilizes a "Pull-System" for task distribution, allowing for better flexibility:
1.  **Schedule Publishing**: Admins upload shifts, routes, and quotas to the central dashboard.
2.  **Marketplace View**: Drivers browse a real-time list of available schedules on their mobile app.
3.  **Task Claiming**: Once a Driver selects a slot, the system triggers a **Concurrency Lock** to prevent double-booking.

### 3. End-to-End Operational Diagram
```mermaid
sequenceDiagram
    participant A as Admin Dashboard
    participant S as HaulFlow System
    participant D as Driver (Mobile App)
    participant P as Security Post (Checkpoint)
    participant W as Weighbridge (Scale)

    Note over A, S: Phase 1: Scheduling
    A->>S: Publish Shifts & Routes
    D->>S: Browse & Claim Available Schedule
    S->>D: Confirm Assignment (Slot Locked)

    Note over D, P: Phase 2: Transit & Validation
    loop Each Designated Checkpoint
        D->>P: Physical Arrival
        D->>S: Submit Live Photo (Checkpoint)
        S->>S: Log Progress & Timestamp
    end

    Note over D, W: Phase 3: Weighing & Completion
    D->>W: Truck Enters Scale
    D->>S: Quick Self-Verification (ID Match)
    W->>S: Sync Netto Weight Data
    S->>S: Finalize Audit Trail (Data Locked)
    S->>D: Task Completed Notification
```

---

<a name="bahasa-indonesia"></a>
## Versi Bahasa Indonesia

### 1. Alur Pra-Operasional: Registrasi & Onboarding
Sebelum proses pengangkutan dimulai, sistem membangun landasan yang aman melalui registrasi entitas untuk memastikan setiap titik data di lapangan dapat dilacak secara akurat.

*   **Onboarding Driver & Kendaraan**: Driver wajib mendaftarkan identitas legal (SIM/KTP) dan nomor plat kendaraan. Database ini menjadi referensi utama untuk proses "Verifikasi Mandiri" di jembatan timbang.
*   **Role-Based Access Control (RBAC)**: Pembagian izin akses yang jelas antara Admin Armada (manajemen jadwal) dan Driver (pelaksanaan tugas).

### 2. Mekanisme Penjadwalan: Penugasan Terbuka (Open Assignment)
HaulFlow menggunakan "Pull-System" (Sistem Tarik) untuk distribusi jadwal hauling agar lebih fleksibel:
1.  **Publikasi Jadwal**: Admin membuat shift, rute, dan kuota ke dashboard pusat.
2.  **Tampilan Marketplace**: Driver menelusuri daftar jadwal yang tersedia secara real-time melalui aplikasi mobile.
3.  **Klaim Tugas**: Setelah Driver memilih slot, sistem mengaktifkan **Concurrency Lock** untuk mencegah pemesanan ganda.

### 3. Diagram Operasional End-to-End
```mermaid
sequenceDiagram
    participant A as Admin Dashboard
    participant S as HaulFlow System
    participant D as Driver (Mobile App)
    participant P as Pos Keamanan (Checkpoint)
    participant W as Jembatan Timbang (Scale)

    Note over A, S: Fase 1: Penjadwalan
    A->>S: Publikasi Shift & Rute
    D->>S: Cari & Klaim Jadwal Tersedia
    S->>D: Konfirmasi Penugasan (Slot Terkunci)

    Note over D, P: Fase 2: Transit & Validasi
    loop Di Setiap Checkpoint
        D->>P: Kedatangan Fisik
        D->>S: Kirim Foto Live (Checkpoint)
        S->>S: Catat Progres & Timestamp
    end

    Note over D, W: Fase 3: Penimbangan & Selesai
    D->>W: Truk Masuk Timbangan
    D->>S: Verifikasi Mandiri Cepat (Cek ID)
    W->>S: Sinkronisasi Data Berat Bersih (Netto)
    S->>S: Finalisasi Audit Trail (Data Terkunci)
    S->>D: Notifikasi Tugas Selesai
```
