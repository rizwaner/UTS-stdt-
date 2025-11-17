# UTS-stdt-

# Jawaban Tugas Sistem Terdistribusi  
CAP, BASE, GraphQL, dan Streaming Replication PostgreSQL

---

# 1. Teorema CAP dan BASE

## CAP Theorem
Dalam sistem terdistribusi, hanya dua dari tiga properti berikut yang bisa dipenuhi secara bersamaan:

| Komponen | Penjelasan |
|---------|------------|
| **Consistency (C)** | Semua node memiliki data yang sama pada waktu yang sama. |
| **Availability (A)** | Sistem tetap memberi respons walau sebagian node gagal. |
| **Partition Tolerance (P)** | Sistem tetap berjalan meski ada gangguan jaringan antar node. |

Karena P wajib dalam sistem terdistribusi, pilihan realistis hanya:
- **CP** (Consistency + Partition Tolerance)
- **AP** (Availability + Partition Tolerance)

## BASE
BASE adalah filosofi sistem yang memilih **AP**:
- **Basically Available:** Sistem selalu memberi respons.
- **Soft-state:** Data bisa berubah sebelum akhirnya stabil.
- **Eventually Consistent:** Konsistensi dicapai secara bertahap.

## Keterkaitan
- **CAP** mendefinisikan batasan teoretis.
- **BASE** adalah prinsip desain untuk sistem yang memilih **AP**.
- Implementasi BASE adalah cara praktis untuk mengatasi batasan CAP.

## Contoh Singkat
Pada e-commerce:
- Server di berbagai lokasi bisa mengalami perbedaan data stok sesaat.
- Sistem tetap berjalan (A), meski data tidak segera konsisten.
- Konsistensi datang belakangan (eventual consistency / BASE).

---

# 2. GraphQL dan Komunikasi Antar Proses

GraphQL bekerja sebagai **API Gateway** atau **Data Orchestrator** dalam arsitektur microservices.  
Klien hanya mengirim satu query, lalu GraphQL berkomunikasi dengan berbagai service untuk mengumpulkan data.

## Peran GraphQL
- Mengurangi beban klien karena tidak perlu memanggil banyak endpoint.
- Mengatur komunikasi antar proses (internal microservices).
- Menggabungkan respons dari berbagai service menjadi satu hasil akhir.

## Diagram (Mermaid)

```mermaid
flowchart LR
    Client --> GQL[GraphQL Gateway]

    GQL --> S1[User Service]
    GQL --> S2[Product Service]
    GQL --> S3[Order Service]

    S1 --> DB1[(DB User)]
    S2 --> DB2[(DB Product)]
    S3 --> DB3[(DB Order)]

    GQL --> Response[Aggregated Response]