# SGEEDE Learning Notes (15 August)

## 🔍 Perbedaan Context vs Domain di Odoo

**Context**

* Digunakan untuk memberikan *default value* atau memfilter *state* ketika membuka form baru (`create` mode).
* Contoh: Mengatur supaya field tertentu otomatis terisi / state default saat membuat record baru.

**Domain**

* Digunakan untuk memfilter record yang muncul pada *tree/list view*.
* Berfungsi membatasi record yang ditampilkan sesuai kondisi tertentu.

| Aspek        | Context                    | Domain                           |
| ------------ | -------------------------- | -------------------------------- |
| Fungsi       | Default value di form baru | Filter tampilan record di list   |
| Dipakai di   | `context={}`               | `domain=[]`                      |
| Contoh pakai | state: draft saat create   | menampilkan hanya active records |

## 🔍 Search Function

* Berfungsi untuk mencari record tertentu menggunakan domain pencarian.
* Biasanya digunakan di method Python `search()` pada model.

```python
records = self.env['model.name'].search([('field', '=', 'value')])
```

Digunakan untuk output list of record yang sesuai domain pencarian.