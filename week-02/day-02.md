# 📅 Week 2 — Day 2 Notes

## 🎯 Key Learnings

### 1. **Badge in XML**

* Used **badge** to display status in a **rounded box** for a more polished UI.

---

### 2. **Decorations in XML**

Decorations are used to style records in list/tree views based on conditions.

| Decoration Class     | Effect                                  |
| -------------------- | --------------------------------------- |
| `decoration-bf`      | Bold text                               |
| `decoration-it`      | Italics                                 |
| `decoration-danger`  | Light red (critical/problematic states) |
| `decoration-info`    | Light blue                              |
| `decoration-success` | Light green (positive/completed states) |
| `decoration-warning` | Light brown                             |
| `decoration-muted`   | Gray (low emphasis)                     |

💡 **Usage Example:**

```xml
<tree decoration-success="state == 'done'" decoration-danger="state == 'cancel'">
    <field name="name"/>
    <field name="state"/>
</tree>
```

---

### 3. **Restoring Database**

* Practiced restoring an Odoo database from a backup for testing/development.

---

### 4. **Placeholders**

* Added **placeholder** text in XML fields to guide users before input.

```xml
<field name="customer_name" placeholder="Enter Customer Name"/>
```

---

### 5. **Widget: Radio**

* Used `widget="radio"` to display selection fields as radio buttons.

```xml
<field name="priority" widget="radio"/>
```

---

### 6. **Invisible & Readonly by State Change**

* Controlled **field visibility** and **editability** based on record state.

💡 **Example:**

```xml
<field name="approved_by" invisible="state != 'approved'" readonly="state == 'approved'"/>
```

---

✅ **Progress:** Learned more advanced XML UI tweaks in Odoo to improve user experience and enforce business logic visually.

---
