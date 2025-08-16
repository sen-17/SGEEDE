## 📅 14 Agustus – Tambahan Pembelajaran

### 👤 Cara Ambil User ID yang Sedang Modif

* Gunakan `self.env.user.id` untuk mendapatkan ID user aktif saat ini.
* Berguna saat ingin log/change field sesuai user yang sedang melakukan perubahan.

### 🧩 Membuat Widget Gambar + Nama di Field

* Custom widget pada XML untuk menampilkan avatar + nama user.
* Contoh penggunaan atribut `widget="many2one_avatar_user"` agar tampil foto + nama.

### ▶️ Biasakan Menembak Action dengan Nama Modul

* Saat menjalankan action dari Python, gunakan format lengkap `nama_modul.action_xxx` agar lebih jelas & maintainable.

```python
action = self.env.ref('nama_modul.action_model_form').read()[0]
return action
```
