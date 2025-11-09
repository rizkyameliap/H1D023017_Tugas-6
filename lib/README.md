# H1D023017_Tugas6 — Flutter Form Data Mahasiswa

**Deskripsi singkat**
Aplikasi Flutter sederhana untuk menginput dan menampilkan data mahasiswa. Menunjukkan mekanisme *passing data* antar halaman dengan contoh praktis (form → tampilan hasil).

---

## Daftar Isi

1. [Fitur](#fitur)
2. [Struktur Project](#struktur-project)
3. [Alur Passing Data](#alur-passing-data)
4. [Contoh Implementasi](#contoh-implementasi)

   * `lib/ui/form_data.dart`
   * `lib/ui/tampil_data.dart`
5. [Alternatif & Best Practice](#alternatif--best-practice)
6. [Menjalankan Aplikasi](#menjalankan-aplikasi)
7. [Commit & Push ke GitHub](#commit--push-ke-github)
8. [Catatan](#catatan)

---

## Fitur

* Form input data mahasiswa: **Nama**, **NIM**, **Tahun Lahir**
* Validasi input sederhana
* Passing data via constructor saat navigasi
* Tampilan data dan perhitungan usia otomatis

---

## Struktur Project (ringkasan)

```
lib/
├── main.dart
└── ui/
    ├── form_data.dart      // Halaman input form
    └── tampil_data.dart    // Halaman tampilan data
```

---

## Alur Passing Data

1. Pengguna mengisi form di `FormDataScreen`.
2. Saat submit, data diambil dari `TextEditingController`.
3. Data dikirim ke `TampilDataScreen` melalui constructor menggunakan `Navigator.push` + `MaterialPageRoute`.
4. `TampilDataScreen` menerima data sebagai `final` fields, menampilkan dan menghitung usia.

**Singkat:** `Form Input → Navigator.push(...) → Constructor Parameter → TampilDataScreen`.

---

## Contoh Implementasi

### `lib/ui/form_data.dart` (potongan utama)

```dart
// Ambil data dari controller, validasi, lalu kirim
Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => TampilDataScreen(
      nama: _namaController.text,
      nim: _nimController.text,
      tahunLahir: _tahunLahirController.text,
    ),
  ),
);
```

### `lib/ui/tampil_data.dart` (potongan utama)

```dart
class TampilDataScreen extends StatelessWidget {
  final String nama;
  final String nim;
  final String tahunLahir;

  const TampilDataScreen({
    super.key,
    required this.nama,
    required this.nim,
    required this.tahunLahir,
  });

  int _hitungUsia() {
    final tahunSekarang = DateTime.now().year;
    final tahunLahirInt = int.tryParse(tahunLahir) ?? tahunSekarang;
    return tahunSekarang - tahunLahirInt;
  }

  @override
  Widget build(BuildContext context) {
    final usia = _hitungUsia();
    return Scaffold(
      appBar: AppBar(title: const Text('Data Mahasiswa')),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text('Nama: $nama'),
            Text('NIM: $nim'),
            Text('Tahun Lahir: $tahunLahir'),
            Text('Usia: $usia tahun'),
          ],
        ),
      ),
    );
  }
}
```

---

## Alternatif & Best Practice

* **Model class** (direkomendasikan jika data kompleks): buat `class Mahasiswa { ... }` dan kirim objeknya.
* **State management** (Provider / Bloc) untuk aplikasi skala besar atau bila data perlu di-share antar banyak layar.
* **Routing named** untuk organisasi navigasi yang lebih baik.

Contoh pengiriman model:

```dart
class Mahasiswa { final String nama, nim, tahunLahir; Mahasiswa({required this.nama, required this.nim, required this.tahunLahir}); }

Navigator.push(context, MaterialPageRoute(
  builder: (context) => TampilDataScreen(mahasiswa: Mahasiswa(...)),
));
```

---

## Menjalankan Aplikasi

```bash
# Install dependencies
flutter pub get

# Jalankan aplikasi
flutter run
```

---

## Commit & Push ke GitHub (langkah ringkas)

```bash
# Pastikan branch utama bernama 'main'
git branch -M main

# Tambah file README.md
git add README.md

# Commit
git commit -m "Add README.md: penjelasan passing data"

# Push ke remote (origin harus sudah di-setup)
git push -u origin main
```

> Tip: Buat `.gitignore` untuk Flutter (jika belum) agar folder `build/`, `.dart_tool/`, `/.idea`, dan `*.iml` tidak ikut ter-push.

---

## Catatan

* Pendekatan constructor + Navigator cocok untuk data sederhana dan menjaga tipe data tetap eksplisit.
* Untuk tugas/penilaian, jelaskan juga alasan pemilihan pattern ini (keuntungan: immutability, type safety, mudah di-debug).

---

