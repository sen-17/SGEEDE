# 📚 Day 1: Odoo Custom Module & Linux Essentials

## 🔧 Linux & System Setup

### 🛠 Key Commands
* sudo = Run as administrator
* sudo su = Switch to root user
* chmod -R 777 /dir = Give full access to a directory
* sudo apt update = Update package list
* nano, touch, mkdir, ls, cd, clear, rm -rf, rm = Basic Linux commands

### 🐘 PostgreSQL Setup
* Access terminal:
  bash
  sudo -u postgres psql
  
* Create a new database user:
  sql
  CREATE USER name WITH PASSWORD 'password';
---

## 🧩 Odoo Custom Module: library

### 1. __manifest__.py
python
{
    'name': 'Library Management',
    'version': '1.0',
    'summary': 'A simple library management module',
    'sequence': 1,
    'description': """This module manages books in a library.""",
    'category': 'Library',
    'author': 'Your Name',
    'depends': ['base'],
    'data': [
        'security/ir.model.access.csv',
        'views/library_book_views.xml',
    ],
    'installable': True,
    'application': True,
}

🧠 *Purpose*: Declares module info, dependencies, views, and access controls.
---

### 2. Root __init__.py
python
from . import models

📌 *Logic*: Load all Python files inside the models/ directory.

---

### 3. models/__init__.py
python
from . import library_book

📌 *Logic*: Register your model file library_book.py.

---

### 4. models/library_book.py
python
from odoo import models, fields

class LibraryBook(models.Model):
    _name = 'library.book'
    _description = 'Library Book'

    name = fields.Char(string='Title', required=True)
    author = fields.Char(string='Author')
    published_date = fields.Date(string='Published Date')
    available = fields.Boolean(string='Available', default=True)

🧠 *Explanation*:
* Creates a table: library_book
* Fields: title, author, date, availability

---

## 🖼 Views

### 🌳 Tree (List) View

xml
<record id="view_library_book_tree" model="ir.ui.view">
    <field name="name">library.book.tree</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">
        <tree>
            <field name="name"/>
            <field name="author"/>
            <field name="published_date"/>
            <field name="available"/>
        </tree>
    </field>
</record>


📌 *Purpose*: Displays records as rows in a list (grid).

---

### 📝 Form View

xml
<record id="view_library_book_form" model="ir.ui.view">
    <field name="name">library.book.form</field>
    <field name="model">library.book</field>
    <field name="arch" type="xml">
        <form>
            <sheet>
                <group>
                    <field name="name"/>
                    <field name="author"/>
                    <field name="published_date"/>
                    <field name="isbn"/>
                    <field name="available"/>
                </group>
            </sheet>
        </form>
    </field>
</record>


📌 *Purpose*: Provides a user-friendly form to edit book details.

---

## 📁 Menu Items

xml
<menuitem id="menu_library_root" name="Library"/>
<menuitem id="menu_library_books" name="Books" parent="menu_library_root"/>


📌 Adds a main menu and a submenu under “Library”.

---

## ⚙ Action Window

xml
<record id="action_library_books" model="ir.actions.act_window">
    <field name="name">Books</field>
    <field name="res_model">library.book</field>
    <field name="view_mode">tree,form</field>
</record>


📌 Tells Odoo to open the library.book model in tree → form view.

---

## 🔗 Link Action to Menu

xml
<menuitem id="menu_library_books_list"
          name="Manage Books"
          parent="menu_library_books"
          action="action_library_books"/>


📌 When users click “Manage Books”, it opens the list view of books.

---





