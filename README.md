# Sugar Pals - Final Project Report

**Periode Final Project:** 19 May - 16 June 2026  
**Tema:** Edukasi pencegahan diabetes dan kontrol konsumsi gula  
**SDG:** SDG 3.4 - Good Health & Well-Being

## Ringkasan Proyek

Sugar Pals adalah aplikasi Flutter untuk membantu pengguna memantau konsumsi gula harian, risiko diabetes, dan aktivitas olahraga. Seluruh alur utama dibuat end-to-end dari mobile app ke cloud server melalui Firebase Authentication, Cloud Firestore, push notification, dan integrasi external API.

## Kesesuaian Dengan FP Guidelines

- Individual contribution terpenuhi: setiap anggota memegang satu fitur fungsional utama dengan CRUD penuh.
- Project theme sesuai SDGs: fokus pada kesehatan, pencegahan diabetes, dan pengelolaan gaya hidup.
- Feature complexity terpenuhi: fitur utama menggunakan Create, Read, Update, Delete.
- Version control terpenuhi: seluruh project disimpan dalam satu repository utama.
- User authentication terpenuhi: login dan onboarding memakai Firebase Authentication.
- Cloud infrastructure terpenuhi: data utama tersimpan di Cloud Firestore.
- Project timeline terpenuhi: pengembangan dijalankan bertahap dengan commit rutin.
- API integration terpenuhi: lookup nutrisi/barcode menggunakan Open Food Facts dan Cloud Function proxy.
- AI policy diperhatikan: AI boleh dipakai untuk code generation, tetapi kode harus tetap bisa dijelaskan dan dibetulkan saat demo.

## Technical Requirements

- Firebase Authentication: dipakai untuk login dan identitas user.
- Cloud Firestore: dipakai untuk menyimpan profile, risk assessment, sugar log, activity log, dan challenge.
- Push Notifications: dipakai untuk sinkronisasi dan pengingat notifikasi via Firebase Messaging.
- Navigation Bar: dipakai sebagai navigasi utama antar halaman aplikasi.
- Bonus architecture yang aktif: Firebase Crashlytics untuk real-time crash reporting.

## Pembagian Jobdesk CRUD

| Anggota | Branch | Fitur | Create | Read | Update | Delete | Folder Utama |
|---|---|---|---|---|---|---|---|
| Azka | `feature/risk-assessment` | Kalkulator Risiko Diabetes | Tambah hasil assessment baru | Lihat riwayat assessment | Ubah data assessment | Hapus assessment lama | `lib/screens/risk_screen.dart` |
| Naufal | `feature/sugar-log` | Log Konsumsi Gula Harian | Tambah log gula dan log olahraga | Lihat riwayat gula, riwayat aktivitas, dan ringkasan harian | Edit log gula dan edit log olahraga | Hapus log gula dan hapus log olahraga | `lib/screens/sugar_log_screen.dart` |
| Dea | `feature/challenge-streak` | Tantangan dan Streak | Buat challenge gula atau challenge olahraga | Lihat daftar challenge dan progress harian | Edit, cancel, dan recalculate challenge | Hapus challenge nonaktif | `lib/screens/challenge_screen.dart` |

### Detail CRUD per fitur

#### 1. Kalkulator Risiko Diabetes

- Create: menambahkan hasil assessment ke koleksi `riskAssessments`.
- Read: menampilkan daftar riwayat assessment dari Cloud Firestore.
- Update: mengubah data assessment lama bila user melakukan revisi.
- Delete: menghapus assessment yang sudah tidak dipakai.

#### 2. Log Konsumsi Gula Harian

- Create: menambah log gula manual, dari barcode, atau dari alur olahraga.
- Read: menampilkan ringkasan gula hari ini, riwayat gula, dan riwayat aktivitas.
- Update: mengubah data log gula atau log aktivitas yang sudah tersimpan.
- Delete: menghapus log gula maupun log olahraga lalu menghitung ulang progress challenge.

#### 3. Tantangan dan Streak

- Create: membuat challenge baru untuk target gula atau target olahraga.
- Read: menampilkan daftar challenge aktif, expired, completed, dan progress-nya.
- Update: mengubah data challenge, cancel challenge, dan menghitung ulang progress.
- Delete: menghapus challenge yang sudah nonaktif.

## Alur Cloud dan API

### Firebase Authentication

- User login memakai Firebase Auth.
- Setelah login, aplikasi memeriksa status profil dan mengarahkan user ke onboarding jika data belum lengkap.

### Cloud Firestore

- Profile user disimpan di koleksi `users`.
- Data turunan disimpan di subkoleksi seperti `riskAssessments`, `sugarLogs`, `activityLogs`, dan `challenges`.

### External API

- Open Food Facts dipakai sebagai fallback lookup nutrisi/barcode.
- Cloud Function `lookupNutritionByBarcode` dipakai sebagai proxy backend untuk mengambil data nutrisi dari beberapa provider.
- Provider backend yang dipakai:
  - `c0r.ai`
  - `CalorieAPI`
  - `USDA FoodData Central`
  - `Edamam`
  - `Open Food Facts`

### Catatan perhitungan kalori

- Tracking olahraga dan estimasi kalori dihitung lokal di aplikasi.
- Rumus yang dipakai:

```text
kalori = MET x berat_badan_kg x durasi_jam
```

- Jalan kaki menggunakan MET lebih rendah dari lari.
- Durasi dihitung dari jarak dan kecepatan asumsi.
- Hasil perhitungan dipakai untuk progress challenge olahraga.

## Struktur Fitur Utama

- `lib/main.dart` menangani bootstrap Firebase, auth gate, notifikasi, dan navigasi utama.
- `lib/navigation_shell.dart` menyediakan bottom navigation bar.
- `lib/screens/risk_screen.dart` menangani kalkulator risiko.
- `lib/screens/sugar_log_screen.dart` menangani sugar log, barcode lookup, dan activity log.
- `lib/screens/challenge_screen.dart` menangani challenge dan streak.
- `lib/services/nutrition_lookup_service.dart` menangani lookup barcode ke backend dan fallback ke Open Food Facts.

## Validasi Akhir

- `flutter analyze` - lulus
- `flutter test` - lulus
- `flutter build apk --debug` - lulus

## Catatan Presentasi

- Aplikasi ini cocok dipresentasikan sebagai solusi edukasi kesehatan berbasis SDG 3.4.
- Fokus demo sebaiknya ada di alur login, onboarding, tambah data, update data, hapus data, dan sinkronisasi challenge.
- Saat demo final, siapkan penjelasan pembagian tugas tiap anggota dan alur CRUD end-to-end masing-masing fitur.

