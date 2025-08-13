# 📓 Day 1 – Odoo Flows

## 🛒 Purchase Flow

1. **RFQ (Request for Quotation)** → Print Quotation
2. **Confirm Order?**

   * Yes → Send PO to Vendor
   * No → Cancel / Review RFQ
3. Receive Product → Check if **all products received**

   * Yes → Validate → Update stock quant → Create Bill → Validate
   * No → Create backorder
4. Update stock quant → Pick product → Receipt slip
5. Update stock journal → Accounting flow

---

## 📦 Inventory Flow

* Can come from **Sales (outgoing)** or **Purchase (incoming)**
* **Chaining** = menghubungkan beberapa picking menjadi rangkaian otomatis

**Steps:**

1. Receive Product
2. Validate or Create Backorder if incomplete
3. Update stock quant → Pick product → Receipt slip
4. Update stock journal → Purchase done → Reconcile payment → Update purchase journal → Register payment

---

## 📊 Accounting Flow

1. Purchase done
2. Reconcile payment
3. Update purchase journal
4. Register payment

---

## 🛍 Sales Flow

1. Quotation → Client confirms or rejects

   * Confirm → Draft sales order → Create invoice
   * Reject → Cancel
2. Send product from inventory → Decrease stock on hand
3. Invoice done → Accounting flow

---

## 🛠 SGEEDE Project Startup Guide

### 1. Copy Project to Local

* Clone/copy the project files from SGEEDE source to your local machine.

### 2. Environment Setup

* **If using Odoo 17**:

  * Use the pre-created `odoo17-venv` folder.
  * Copy it into your project directory.
* **If downgrading (older Odoo)**:

  * Make sure Python (required version) is installed.
  * Create a new virtual environment:

    ```bash
    python -m venv venv
    ```
  * Activate the environment and install requirements:

    ```bash
    pip install -r requirements.txt
    ```

### 3. Configuration

* Update Odoo config file to **target all addons**, including:

  * Core Odoo addons
  * Custom modules from SGEEDE
  * Third-party addons if any

---

✅ **Key Takeaways:**

* Purchase, Sales, Inventory, and Accounting flows in Odoo are tightly linked.
* Always confirm the product reception before moving to validation.
* Stock quant & stock journal updates are essential for accurate inventory management.
* For SGEEDE projects, environment preparation depends on Odoo version compatibility.

---

