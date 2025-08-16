# Kanban View Notes (Odoo)

## ✅ Basic Attributes `<kanban>`

* `records_draggable="1"` – allow drag & drop of cards (default)
* `groups_draggable="1"` – allow drag & drop of columns (default)
* `default_group_by="state"` – default grouping
* `quick_create="1"` – inline create inside column (default True)
* `quick_create_view` – define view used when creating inline
* `on_create` – action/form opened when clicking add
* `group_create` – allow creation of new group/column
* `group_delete` – allow deleting a group/column
* `archiveable="1"` – allow archive/unarchive
* `sample="1"` – display sample cards in developer mode

```xml
<kanban class="o_res_partner_kanban"
        js_class="website_pages_kanban"
        type="object"
        sample="1">
    <field name="name"/>
    ...
</kanban>
```

## 🧩 Components inside `<kanban>`

* `<field name="..."/>` → output field normally (for form/readonly display)
* `<templates>` block → contains card definition (`kanban-box` template)

### Example structure:

```xml
<templates>
  <t t-name="kanban-box">
    <div class="o_kanban_card oe_kanban_global_click">
      <div class="o_kanban_record_top">
        <t t-out="record.name.value"/>
      </div>
      <div class="o_kanban_record_bottom">
        <field name="user_id"/>
      </div>
    </div>
  </t>
</templates>
```

* `.oe_kanban_global_click` → makes card clickable (open form view)
* `.o_kanban_card` → core wrapper of each kanban record (required)

## 🛠  Common `t-` Syntax in QWeb (used inside templates)

| Directive   | Description                                | Example                                  |
| ----------- | ------------------------------------------ | ---------------------------------------- |
| `t-out`     | print python expression **raw**            | `<t t-out="record.name"/>`               |
| `t-esc`     | print escaped value (HTML-safe)            | `<t t-esc="record.email"/>`              |
| `t-if`      | conditional output                         | `<t t-if="record.active">...</t>`        |
| `t-else`    | else block                                 | `<t t-else>...</t>`                      |
| `t-foreach` | loop through array / records               | `<t t-foreach="records" t-as="rec">`     |
| `t-set`     | define temp variable                       | `<t t-set="total" t-value="0"/>`         |
| `t-call`    | call another qweb template                 | `<t t-call="my_module.other_template"/>` |
| `t-att-*`   | dynamic HTML attributes, ex: `t-att-class` | `<div t-att-class="'bg-'+state"/>`       |

*Note:* You can manipulate data using python expressions inside `t-out` / `t-esc` (e.g., `.upper()`, `+`, `replace()`, etc).

## 🔍 Tips

* To use Bootstrap classes like `mb-5`, you **must include Bootstrap CSS** inside your asset bundle.
* Add to `web.assets_backend` or `web.assets_frontend`:

```xml
<template id="assets_backend" inherit_id="web.assets_backend">
  <xpath expr="." position="inside">
    <link rel="stylesheet" href="/your_module/static/src/css/bootstrap.min.css"/>
  </xpath>
</template>
```
