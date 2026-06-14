# ERD Klasik — Sistem Reservasi Restoran

Entity Relationship Diagram (ERD) bergaya klasik menggunakan **Chen Notation** dengan simbol: entitas (persegi panjang), atribut (elips/oval), relasi (berlian), dan kardinalitas (1, M, N).

---

## Diagram ERD Klasik

```mermaid
graph TD

%% DEKLARASI ENTITAS (Persegi Panjang)
Users[Users]
Reservasi[Reservasi]
Kategori[Kategori_menu]
Menu[Menu]
Detail[Detail_reservasi]

%% DEKLARASI RELASI (Belah Ketupat / Berlian)
R_Membuat{membuat}
R_Klasifikasi{mengklasifikasi}
R_Memiliki{memiliki}
R_Termasuk{termasuk dalam}

%% -----------------------------------------------------
%% ATRIBUT & PENGHUBUNG ATRIBUT KE ENTITAS (Oval)
%% -----------------------------------------------------

%% Atribut Users
u1([<u>Id_nama</u>])
u2([nama])
u3([Username])
u4([password])
u5([role])
u6([created_at])
Users --- u1 & u2 & u3 & u4 & u5 & u6

%% Atribut Kategori_menu
k1([<u>id</u>])
k2([nama_kategori])
Kategori --- k1 & k2

%% Atribut Menu
m1([<u>id</u>])
m2([nama_menu])
m3([harga])
m4([deskripsi])
m5([tersedia])
Menu --- m1 & m2 & m3 & m4 & m5

%% Atribut Reservasi
r1([<u>Id_reservasi</u>])
r2([nama_pemesan])
r3([Jumlah_orang])
r4([tanggal])
r5([Jam_reservasi])
r6([No_hp])
r7([total_harga])
r8([status])
Reservasi --- r1 & r2 & r3 & r4 & r5 & r6 & r7 & r8

%% Atribut Detail_reservasi
d1([<u>Id_detail</u>])
d2([Jumlah_pesanan])
d3([subtotal])
Detail --- d1 & d2 & d3

%% -----------------------------------------------------
%% HUBUNGAN ANTAR ENTITAS & KARDINALITAS
%% -----------------------------------------------------
Users ---|1| R_Membuat
R_Membuat ---|M| Reservasi

Kategori ---|1| R_Klasifikasi
R_Klasifikasi ---|N| Menu

Reservasi ---|1| R_Memiliki
R_Memiliki ---|N| Detail

Menu ---|1| R_Termasuk
R_Termasuk ---|N| Detail

%% -----------------------------------------------------
%% STYLING
%% -----------------------------------------------------
classDef entitas fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
classDef relasi  fill:#ede7f6,stroke:#4527a0,stroke-width:2px;
classDef atribut fill:#e0f2f1,stroke:#00695c,stroke-width:1px;

class Users,Reservasi,Kategori,Menu,Detail entitas;
class R_Membuat,R_Klasifikasi,R_Memiliki,R_Termasuk relasi;
class u1,u2,u3,u4,u5,u6,k1,k2,m1,m2,m3,m4,m5,r1,r2,r3,r4,r5,r6,r7,r8,d1,d2,d3 atribut;
```

---

## Struktur Entitas

### 1. Tabel `users`

| Kolom | Tipe Data | Keterangan |
|---|---|---|
| `Id_nama` *(PK)* | INT, AUTO_INCREMENT | Primary Key |
| `nama` | VARCHAR(100) | Nama lengkap pengguna |
| `Username` | VARCHAR(100), UNIQUE | Username untuk login |
| `password` | VARCHAR(100) | Password terenkripsi |
| `role` | ENUM('member','admin') | Peran pengguna, default 'member' |
| `created_at` | TIMESTAMP | Waktu pembuatan akun |

---

### 2. Tabel `kategori_menu`

| Kolom | Tipe Data | Keterangan |
|---|---|---|
| `id` *(PK)* | INT, AUTO_INCREMENT | Primary Key |
| `nama_kategori` | VARCHAR(50) | Nama jenis kategori menu |

---

### 3. Tabel `menu`

| Kolom | Tipe Data | Keterangan |
|---|---|---|
| `id` *(PK)* | INT, AUTO_INCREMENT | Primary Key |
| `nama_menu` | VARCHAR(100) | Nama makanan / minuman |
| `Kategori_id` *(FK)* | INT(11) | Foreign Key ke kategori_menu.id |
| `harga` | INT(11) | Harga produk |
| `deskripsi` | TEXT | Deskripsi item menu |
| `tersedia` | TINYINT(1) | Ketersediaan produk (1 = tersedia) |

---

### 4. Tabel `reservasi`

| Kolom | Tipe Data | Keterangan |
|---|---|---|
| `Id_reservasi` *(PK)* | INT | Primary Key |
| `User_id` *(FK)* | INT(11) | Foreign Key ke users.Id_nama |
| `nama_pemesan` | VARCHAR(100) | Nama pelanggan pemesan |
| `No_hp` | INT(11) | Nomor handphone pemesan |
| `Jumlah_orang` | INT, DEFAULT 1 | Jumlah orang yang datang |
| `total_harga` | INT | Total harga keseluruhan |
| `tanggal` | DATE | Tanggal reservasi kunjungan |
| `Jam_reservasi` | TIME | Jam reservasi |
| `status` | ENUM | baru / diproses / selesai / dibatalkan |

---

### 5. Tabel `detail_reservasi`

| Kolom | Tipe Data | Keterangan |
|---|---|---|
| `Id_detail` *(PK)* | INT, AUTO_INCREMENT | Primary Key |
| `Id_reservasi` *(FK)* | INT(11) | Foreign Key ke reservasi.Id_reservasi |
| `Id_menu` *(FK)* | INT(11) | Foreign Key ke menu.id |
| `Jumlah_pesanan` | INT(11) | Jumlah item yang dipesan |
| `subtotal` | DECIMAL(10,2) | Total harga per item pesanan |

---

## Relasi Antar Entitas

| Relasi | Entitas Asal | Kardinalitas | Entitas Tujuan | Keterangan |
|---|---|---|---|---|
| membuat | `users` | **1 : M** | `reservasi` | Satu user membuat banyak reservasi |
| mengklasifikasi | `kategori_menu` | **1 : N** | `menu` | Satu kategori mencakup banyak menu |
| memiliki | `reservasi` | **1 : N** | `detail_reservasi` | Satu reservasi memiliki banyak detail |
| termasuk dalam | `menu` | **1 : N** | `detail_reservasi` | Satu menu muncul di banyak detail reservasi |

---

## Simbol ERD yang Digunakan

| Simbol | Sintaks Mermaid | Keterangan |
|---|---|---|
| Entitas | `Nama[Nama]` | Persegi panjang |
| Relasi | `Nama{Nama}` | Belah ketupat / berlian |
| Atribut | `Nama([Nama])` | Oval / elips |
| Atribut PK | `Nama([<u>Nama</u>])` | Oval dengan garis bawah |
| Kardinalitas | `---|1|`, `---|M|`, `---|N|` | Label pada garis penghubung |
