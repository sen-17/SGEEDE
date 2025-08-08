# Day 02 - Odoo Fields, Relational DB, Wizards, Inheritance, Reports, Functions

## 📌 Odoo Field Types
### 🔹 Simple Fields
These fields store basic data types in the database:

* **Char**: Small strings

  ```python
  student_name = fields.Char(string="Name")
  ```
* **Text**: Large text blocks

  ```python
  address = fields.Text(string="Address")
  ```
* **Html**: Stores rich HTML content

  ```python
  description_html = fields.Html(string="HTML Description")
  ```
* **Boolean**: True/False values

  ```python
  is_paid = fields.Boolean(string="Paid?")
  ```
* **Selection**: Dropdown with options

  ```python
  gender = fields.Selection([('male', 'Male'), ('female', 'Female')], string="Gender")
  ```
* **Integer**: Whole numbers

  ```python
  age = fields.Integer(string="Age")
  ```
* **Float**: Decimal numbers

  ```python
  score = fields.Float(string="Score")
  ```
* **Date**: Date only (stored in UTC)

  ```python
  birth_date = fields.Date(string="Birth Date")
  ```
* **Datetime**: Date and time (stored in UTC)

  ```python
  appointment = fields.Datetime(string="Appointment")
  ```
* **Monetary**: Money values

  ```python
  price = fields.Monetary(string="Price", currency_field="currency_id")
  ```
* **Binary**: Store files

  ```python
  resume = fields.Binary(string="Resume")
  ```
* **Image**: Store image files

  ```python
  photo = fields.Image(string="Photo")
  ```
* **Json**: Store dictionary-like structure

  ```python
  extra_data = fields.Json(string="Extra Data")
  ```

### 🔹 Relational Fields

These fields link models together:

* **Many2one**: Foreign key stored in this model (many to one)

  ```python
  school_id = fields.Many2one('school.model', string="School")
  ```
* **One2many**: Reverse of Many2one, stores related records in another model

  ```python
  student_ids = fields.One2many('student.model', 'school_id', string="Students")
  ```
* **Many2many**: Stores relationship in a separate relation table

  ```python
  teacher_ids = fields.Many2many('teacher.model', string="Teachers")
  ```
* **Reference**: Can point to any model, stored as string

  ```python
  reference = fields.Reference(selection=[('res.partner', 'Partner'), ('res.users', 'User')], string="Reference")
  ```

### 🔹 Functional / Computed Fields

* **Compute**: Value is computed from another field

  ```python
  total = fields.Float(compute="_compute_total")

  @api.depends('value1', 'value2')
  def _compute_total(self):
      for rec in self:
          rec.total = rec.value1 + rec.value2
  ```
* **Related**: Points to another field on a related model

  ```python
  school_name = fields.Char(related="school_id.name", store=True)
  ```

## ⚙️ Useful Field Attributes

* `string` - Label
* `required=True` - Cannot be left empty
* `readonly=True` - Cannot be edited
* `default=value` - Sets a default value
* `copy=False` - Excludes field during duplication
* `tracking=True` - Tracks changes (for chatter)
* `invisible=True` - Hides the field
* `index=True` - Creates index for faster search
* `groups` - Limit field visibility by group

---

## 🧩 Wizard in Odoo

Wizards are transient models used for popups or temporary interactions (not stored permanently).

* Inherit from `models.TransientModel`
* Fields behave like normal models
* Used for actions like confirmations, reports, quick forms

```python
class ConfirmWizard(models.TransientModel):
    _name = 'example.confirm.wizard'
    _description = 'Confirm Wizard'

    confirm_text = fields.Text(string="Confirm Text")

    def action_confirm(self):
        # Do something
        pass
```

---

## 🧬 Inheritance in Odoo

### 1. **Classical Inheritance**

Extend logic of an existing model.

```python
class InheritStudent(models.Model):
    _inherit = 'student.model'

    new_field = fields.Boolean(string="New Field")
```

### 2. **Prototype Inheritance (\_inherits)**

Inherit fields from another model using foreign key. Used for compositions.

### 3. **View Inheritance**

Modify views using `xpath`.

```xml
<record id="view_form_inherit" model="ir.ui.view">
  <field name="inherit_id" ref="module.view_original"/>
  <field name="arch" type="xml">
    <xpath expr="//field[@name='name']" position="after">
      <field name="new_field"/>
    </xpath>
  </field>
</record>
```

---

## 🧾 Reports in Odoo

### Types of Reports:

* **QWeb PDF**: HTML/CSS to PDF
* **Excel (XLSX)**
* **Text-based reports**

### QWeb Report Example

```xml
<template id="report_student">
  <t t-call="web.html_container">
    <t t-foreach="docs" t-as="doc">
      <p>Name: <t t-esc="doc.name"/></p>
    </t>
  </t>
</template>
```

---

## ⚙️ Special Methods (Function Fields)

### 1. `@api.depends()` - For compute fields

```python
@api.depends('price', 'tax')
def _compute_total(self):
    self.total = self.price + self.tax
```

### 2. `@api.onchange()` - Triggered when field changes in form (not saved yet)

```python
@api.onchange('price')
def _onchange_price(self):
    if self.price > 1000:
        return {'warning': {'title': 'Warning', 'message': 'Price too high!'}}
```

### 3. `@api.constrains()` - Validates data before saving

```python
@api.constrains('age')
def _check_age(self):
    if self.age < 0:
        raise ValidationError("Age cannot be negative")
```

---

## 🧠 Understanding Relational Database in Odoo

In Odoo:

* **Each model = 1 table**
* **Each field = 1 column**
* **Each record = 1 row**

### Relational Types:

* **Many2one**: Stores a foreign key on current table
* **One2many**: Virtual, reverse of Many2one, uses a foreign key on another table
* **Many2many**: Intermediate table stores the link (ex: rel\_model\_id, student\_id, subject\_id)

These relationships allow joining and linking data just like standard PostgreSQL behavior.

---


