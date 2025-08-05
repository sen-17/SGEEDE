# 🧠 Odoo MVC 
🔧 What is MVC?
Odoo uses the Model-View-Controller (MVC) pattern:
- Model: Defines business logic & database structure (models/)
- View: UI layer using XML to render forms, lists, kanban, etc. (views/)
- Controller: Handles HTTP routes, often used in website modules (controllers/)

## 📁 Odoo Module Structure
Assume your custom module is called college_erp.

college_erp/
├── __init__.py
├── __manifest__.py
├── models/
│   ├── __init__.py
│   └── student.py
├── views/
│   └── student_views.xml
├── security/
│   ├── ir.model.access.csv
│   └── security.xml
└── data/
    └── student_data.xml (optional)


### 📝 __manifest__.py example
{
    'name': 'College ERP',
    'version': '1.0',
    'summary': 'Module for managing students in a college',
    'description': 'Custom Odoo module to manage student records, classes, etc.',
    'category': 'Education',
    'author': 'Jasson',
    'website': 'https://yourwebsite.com',
    'depends': ['base'],
    'data': [
        'security/security.xml',
        'security/ir.model.access.csv',
        'views/student_views.xml',
        # 'data/student_data.xml',
    ],
    'installable': True,
    'application': True,
    'auto_install': False,
}
