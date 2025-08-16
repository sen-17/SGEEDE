# 🔍 Odoo 17 — Search View & Search Panel

## 📌 What is a Search View?

* Responsible for **searching values** in **tree views**.
* Allows users to filter, group, and navigate records efficiently.

---

## 1️⃣ Defining a Search View

Example structure:

```xml
<record id="view_something_search" model="ir.ui.view">
    <field name="name">something</field>
    <field name="model">something.something</field>
    <field name="arch" type="xml">
        <search string="something">
            <!-- Fields used for searching -->
            <field name="field_1"/>
            <field name="field_2"/>
            <field name="field_3"/>

            <!-- Filters -->
            <filter string="Filter Name" name="filter_id" domain="[()]"/>
            <!-- Optional separator -->
            <!-- <separator/> -->

            <!-- Group By options -->
            <group expand="1" string="Group By">
                <filter name="group_filter" string="Group Name" context="{... : ...}"/>
            </group>

            <!-- Search Panel -->
            <searchpanel>
                <field name="field_for_panel" string="Panel Name" enable_counters="1"/>
            </searchpanel>
        </search>
    </field>
</record>
```

### ✅ Key Points:

* `<field>` inside `<search>` → fields searchable in tree view.
* `<filter>` → create filters using `domain`.
* `<group>` → define group-by options.
* `<searchpanel>` → add panels for quick filtering with counters.

---

## 2️⃣ Joined Filters

* Combine multiple fields in a single filter.
* Example using reference and name:

```xml
<field name="reference" 
       filter_domain="['|', ('reference', 'ilike', self), ('name', 'ilike', self)]" 
       string="Joined Filter"/>
```

* `|` → logical OR between conditions.
* `ilike` → case-insensitive search.

---

## 📌 Summary

* **Search View**: fields to search, filters, group by, search panels.
* **Filters**: standard or joined filters for multiple criteria.
* **Group By**: categorize results easily.
* **Search Panel**: interactive sidebar filters with counters.

---

