# Odoo 17 Reports (PDF & XLSX) Notes

## 🧾 PDF Reports (QWeb Reports)

### ✅ Basics

* Defined using **ir.actions.report** (model: `ir.actions.report`)
* Use **QWeb templates** to generate HTML → rendered into PDF
* Template inheritance via `<t t-call>`

### ⚙️ Define Action:

```xml
<record id="report_sale_order_pdf" model="ir.actions.report">
    <field name="name">Sale Order PDF</field>
    <field name="model">sale.order</field>
    <field name="report_type">qweb-pdf</field>
    <field name="report_name">my_module.report_saleorder_template</field>
    <field name="report_file">my_module.report_saleorder_template</field>
</record>
```

### 📄 QWeb Template:

```xml
<template id="report_saleorder_template">
   <t t-call="web.external_layout">
     <div class="page">
       <h2 t-esc="o.name"/>
       <t t-foreach="o.order_line" t-as="line">
         <p t-esc="line.product_id.display_name"/>
       </t>
     </div>
   </t>
</template>
```

### 🔧 Common `t-` usage in reports

| Directive   | Use case                                      |
| ----------- | --------------------------------------------- |
| `t-field`   | show field with formatting (e.g dates, money) |
| `t-out`     | raw expression                                |
| `t-esc`     | escaped text                                  |
| `t-call`    | inherit/layout                                |
| `t-foreach` | looping                                       |

## 📊 XLSX Reports (Excel)

Odoo generates XLSX in **Python**, not QWeb.

### 🧪 How to add action:

```xml
<record id="action_export_xlsx" model="ir.actions.server">
  <field name="name">Export to XLSX</field>
  <field name="model_name">sale.order</field>
  <field name="binding_model_id" ref="sale.model_sale_order"/>
  <field name="state">code</field>
  <field name="code">action = model.generate_xlsx()</field>
</record>
```

### 📄 Python sample generator:

```python
import io
from odoo import models
from xlsxwriter.workbook import Workbook

class SaleOrder(models.Model):
    _inherit = 'sale.order'

    def generate_xlsx(self):
        output = io.BytesIO()
        workbook = Workbook(output, {'in_memory': True})
        sheet = workbook.add_worksheet('Orders')
        sheet.write(0, 0, 'Order')
        sheet.write(0, 1, 'Customer')

        row = 1
        for order in self:
            sheet.write(row, 0, order.name)
            sheet.write(row, 1, order.partner_id.name)
            row += 1
        workbook.close()
        output.seek(0)
        return {
            'type': 'ir.actions.act_url',
            'url': 'data:application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;base64,' + b64encode(output.read()).decode(),
            'target': 'new',
        }
```

### 📌 Tips:

* Use **xlsxwriter / openpyxl**
* XLSX actions often triggered from **button** or **server action**
* Styling cells → `workbook.add_format()`
* Use `ir.actions.act_url` to return file

## ✨ Recommendation

| Report type | When to use                          |
| ----------- | ------------------------------------ |
| PDF (QWeb)  | invoices, formal reports, printable  |
| XLSX        | data export, analysis in spreadsheet |

---


