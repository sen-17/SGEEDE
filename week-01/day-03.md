# 📘 Day 3 – Odoo ORM Relationship Notes

## 🔗 Relational Fields in Odoo

Odoo supports several types of relational fields to define relationships between models:

### ✅ Many2one

* **Arah**: Banyak ke satu
* **Contoh**: Banyak `Karyawan` berada dalam satu `Departemen`
* **Contoh kode**:

  ```python
  departemen_id = fields.Many2one('departemen', string="Departemen")
  ```

### ✅ One2many

* **Arah**: Satu ke banyak
* **Contoh**: Satu `Departemen` memiliki banyak `Karyawan`
* **Contoh kode**:

  ```python
  daftar_karyawan_ids = fields.One2many('karyawan', 'departemen_id', string="Daftar Karyawan")
  ```

> **Catatan Penting**:
> Setiap field `One2many` **harus selalu memiliki inverse field** `Many2one`. Biasanya relasi ini dibuat dalam model baru yang disebut dengan nama seperti `model_line`.

Contoh implementasi:

```python
class Departemen(models.Model):
    _name = "departemen"
    nama = fields.Char("Nama Departemen")
    daftar_karyawan_ids = fields.One2many('karyawan', 'departemen_id', string="Daftar Karyawan")

class Karyawan(models.Model):
    _name = "karyawan"
    nama = fields.Char("Nama Karyawan")
    departemen_id = fields.Many2one('departemen', string="Departemen")
```

---

### ✅ Many2many

* **Arah**: Banyak ke banyak
* **Contoh**: Banyak `Karyawan` bisa tergabung di banyak `Proyek`
* **Contoh kode**:

  ```python
  anggota_tim_ids = fields.Many2many('anggota.tim', string="Anggota Tim")
  ```

---

## 📄 XML View Tips: Using One2many with Editable Form

Jika kamu ingin menampilkan dan mengedit data `One2many` langsung dari parent model (misalnya dari form `Departemen` melihat dan mengedit daftar `Karyawan`), kamu bisa menggunakan struktur seperti ini di XML:

```xml
<field name="daftar_karyawan_ids">
  <tree editable="bottom">
    <field name="nama_karyawan"/>
    <field name="departemen_id"/>
  </tree>
</field>
```

### 🧾 Penjelasan `editable="bottom"`:

* `editable="bottom"` memungkinkan user untuk **menambahkan data langsung dari tampilan tree** di bawah daftar.
* Menambahkan baris baru akan menampilkan form input di bawah list (bukan dalam dialog atau halaman terpisah).
* Berguna untuk membuat pengalaman pengguna lebih cepat dan intuitif saat menambahkan atau mengedit banyak item secara langsung.

---

## 📊 Ringkasan Perbedaan Relasi

| Jenis Relasi  | Arah Hubungan    | Penyimpanan Data                                                   | Keterangan                                              |
| ------------- | ---------------- | ------------------------------------------------------------------ | ------------------------------------------------------- |
| **Many2one**  | Banyak ke Satu   | Foreign key di model "anak"                                        | Paling umum, menghubungkan banyak catatan ke satu induk |
| **One2many**  | Satu ke Banyak   | Tidak menyimpan data langsung, hanya referensi balik ke `Many2one` | Menampilkan daftar "anak" dari satu "induk"             |
| **Many2many** | Banyak ke Banyak | Tabel bantu otomatis                                               | Digunakan untuk relasi non-eksklusif antara dua sisi    |

---

## 🧠 Tips

* **Gunakan `notebook`** di tampilan XML jika kamu ingin mengorganisir `One2many` dalam tab-tab.
* Gunakan `model_line` saat mendesain model relasi untuk menyimpan child data, agar tidak mengacaukan model utama.

---

