## 📌 Day 5 Odoo Development Notes – Extending Models (Inherit), Views & Reports

### *1. Extending Existing Models*

Using _inherit to *add new fields* to built-in models:

python
from odoo import models, fields

class PurchaseOrder(models.Model):
    _inherit = 'purchase.order'
    customer_reference = fields.Char(string="Customer Reference")


class PurchaseOrderLine(models.Model):
    _inherit = 'purchase.order.line'
    number = fields.Char(string="No")


* _inherit modifies an existing model instead of creating a new one.
* New fields:

  * customer_reference → on purchase orders.
  * number → on purchase order lines.

---

### *2. Modifying Views with Inheritance*

View inheritance lets you insert, hide, or modify fields in an existing form or tree.

xml
<record id="library_purchase_order_form_inherit" model="ir.ui.view">
    <field name="name">library.purchase.order.form.inherit</field>
    <field name="model">purchase.order</field>
    <field name="inherit_id" ref="purchase.purchase_order_form"/>
    <field name="arch" type="xml">

        <!-- Add 'number' before product_id in order line tree -->
        <xpath expr="//field[@name='order_line']/tree/field[@name='product_id']" position="before">
            <field name="number"/>
        </xpath>

        <!-- Add customer_reference after partner_ref -->
        <field name="partner_ref" position="after">
            <field name="customer_reference"/>
        </field>

        <!-- Hide partner_ref -->
        <field name="partner_ref" position="attributes">
            <attribute name="invisible">True</attribute>
        </field>
    </field>
</record>


*Key Points:*

* inherit_id → references the base view.
* xpath → finds the target element.
* position:

  * before / after → insert fields.
  * attributes → modify field attributes (e.g., hide).

---

### *3. Creating QWeb PDF Reports*

Odoo reports use *QWeb templates* to generate HTML or PDF.

#### **Template (QWeb)**

xml
<template id="report_books_borrowed_list">
    <t t-call="web.html_container">
        <t t-foreach="docs" t-as="doc">
            <t t-call="web.basic_layout">
                <h2>Transaction Report</h2>

                <p><strong>REFERENCE: </strong><span t-field="doc.reference_id"/></p>
                <p><strong>NAME: </strong><span t-field="doc.borrower_name.name"/></p>
                <p><strong>Borrow Date: </strong><span t-field="doc.borrow_date"/></p>
                <p><strong>Total Quantity: </strong><span t-field="doc.total_qty"/></p>

                <t t-if="doc.line_ids">
                    <h3>Books Borrowed:</h3>
                    <table border="1">
                        <tr>
                            <th>Book Title</th>
                            <th>Quantity</th>
                        </tr>
                        <t t-foreach="doc.line_ids" t-as="line">
                            <tr>
                                <td><span t-field="line.book_id.name"/></td>
                                <td><span t-field="line.quantity"/></td>
                            </tr>
                        </t>
                    </table>
                </t>
            </t>
        </t>
    </t>
</template>


#### *Report Action*

xml
<record id="report_books_borrowed" model="ir.actions.report">
    <field name="name">Transaction</field>
    <field name="model">library.transaction</field>
    <field name="report_type">qweb-pdf</field>
    <field name="report_name">library_management.report_books_borrowed_list</field>
    <field name="report_file">library_management.report_books_borrowed_list</field>
</record>


*Key Points:*

* t-foreach="docs" → Loops through the records.
* t-field="doc.field_name" → Displays a model field with proper formatting.
* t-call="web.basic_layout" → Uses Odoo’s default PDF layout.
* In ir.actions.report:

  * report_type="qweb-pdf" → PDF output.
  * report_name → Must match <template id> with module name prefix.

---

### *4. What I Learned Today*

1. *Extending models* with _inherit and adding new fields without rewriting the core model.
2. *Modifying views* using XML view inheritance and xpath.
3. *Creating PDF reports* using QWeb templates (t-foreach, t-field, t-call).
4. *Linking a template to a report action* via ir.actions.report.

---

### *5. Best Practices*

* Keep model, view, and report code organized in separate files.
* Use module_name.template_id in report_name to avoid conflicts.
* Test the report in both HTML and PDF modes for formatting issues.

---

