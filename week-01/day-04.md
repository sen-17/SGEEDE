# Day 4 — Odoo Views, Tracking, Status Bars, Sequences, Widgets, Computed Fields, and Smart Buttons
---

## 1. Views & Tracking

### **Views**
* **Record → fields → tree/form/kanban**
* In **form views**:

  * Use `<group>` for grouping fields.
  * Use `<sheet>` so the form doesn’t take up the entire screen.

### **Tracking**

1. Add `mail.thread` to inherit in your model:

   ```python
   _inherit = ['mail.thread']
   ```
2. Add `mail` to dependencies in `manifest.py`:

   ```python
   'depends': ['mail']
   ```
3. In view XML (after `<sheet>` tag):

   ```xml
   <div class="oe_chatter">
       <field name="message_follower_ids" groups="base.group_user"/>
       <field name="message_ids"/>
   </div>
   ```
4. Enable field tracking:

   ```python
   field_name = fields.Char(tracking=True)
   ```

---

## 2. Removing the "New" Button

* Add inside `<tree>` tag:

  ```xml
  create="0"
  ```

---

## 3. Status Bar, Buttons, Conditional Invisible & `self`

### **Selection field for status**

```python
state = fields.Selection([
    ('draft', 'Draft'),
    ('ongoing', 'Ongoing'),
    ('done', 'Done'),
    ('cancel', 'Cancelled')
], default='draft')
```

### **Header example**

```xml
<header>
    <button name="action_confirm" type="object" string="Confirm"
            invisible="state != 'draft'" class="oe_highlight"/>
    <field name="state" widget="statusbar" statusbar_visible="draft,ongoing,done"
           options="{'clickable': '1'}"/>
</header>
```

### **Python function**

```python
def action_confirm(self):
    self.state = 'confirm'
```

---

## 4. Sequence, `_rec_name`, `noupdate`, and `create()` method

* **Many2one technical name**: `name_id`
* **Custom record display**:

  ```python
  _rec_name = 'name_id'
  ```

### **Create sequence**

**sequence.xml**:

```xml
<record id="seq_purchase_order" model="ir.sequence">
    <field name="name">Purchase Order</field>
    <field name="code">purchase.order</field>
    <field name="prefix">PO</field>
    <field name="padding">5</field>
</record>
```

**Model**:

```python
reference = fields.Char(string="Reference", default="New")

@api.model_create_multi
def create(self, vals_list):
    for vals in vals_list:
        if not vals.get('reference') or vals['reference'] == 'New':
            vals['reference'] = self.env['ir.sequence'].next_by_code('purchase.order')
    return super().create(vals_list)
```

* Wrap sequence in:

  ```xml
  <data noupdate="1"> ... </data>
  ```

---

## 5. Icons & Widgets

* Place icon: `static/description/icon.png`
* Use:

  ```xml
  widget="many2many_tags"
  ```
* **Handle widget**:

  ```python
  sequence = fields.Integer(default=10)
  _order = 'sequence, id'
  ```

  ```xml
  <field name="sequence" widget="handle"/>
  ```

---

## 6. Notebook & Pages

```xml
<notebook>
    <page string="Details">
        ...
    </page>
</notebook>
```

---

## 7. Searching for Reference

```python
_res_name_search = ['reference', 'name']
```

---

## 8. Compute Display Name

```python
def _compute_display_name(self):
    for rec in self:
        rec.display_name = f"[{rec.reference}] {rec.name}"
```

---

## 9. Compute Fields

```python
total_qty = fields.Float(string='Total Qty', compute='_compute_total_qty')

@api.depends('line_ids.qty')
def _compute_total_qty(self):
    for rec in self:
        rec.total_qty = sum(rec.line_ids.mapped('qty'))
```

---

## 10. Smart Buttons

**Basic example**:

```xml
<div class="oe_button_box" name="button_box">
    <button name="action_function" type="object" class="oe_stat_button">
        <div class="o_stat_info">
            <field name="computed_field" class="o_stat_value"/>
            <span class="o_stat_text">Text</span>
        </div>
    </button>
</div>
```

**With icon & condition**:

```xml
<div class="oe_button_box" name="button_box">
    <button type="object" name="action_view_picking" class="oe_stat_button"
            icon="fa-truck" invisible="incoming_picking_count == 0">
        <field name="incoming_picking_count" widget="statinfo" string="Receipt"
               help="Incoming Shipments"/>
    </button>
</div>
```

---

**✅ Summary:**
Today’s notes covered:
* Structuring Odoo views (form/tree/kanban)
* Enabling chatter tracking
* Controlling form buttons and state with status bars
* Creating sequences and custom display names
* Using widgets, notebooks, and smart buttons
* Writing compute functions for automatic field updates
---

