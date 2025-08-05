# Models
A model in Odoo is a Python class that defines the structure and behavior of data (like a table in a database). It's where you define fields, logic, and database interaction.
All models inherit from models.Model or its variants

## Types of Models in Odoo
1. Regular Model (models.Model)
<img width="1178" height="535" alt="Regular Model" src="https://github.com/user-attachments/assets/8f647355-bd17-4bca-bfaf-a67e0d10906e" />
* Stored permanently in the database.
* Each reord is saved and retrievable
* Used for standard data like products, customers, students, etc.

2. Transient Model (model.TransientModel)
<img width="1101" height="476" alt="Transcient Model" src="https://github.com/user-attachments/assets/da30fa2a-e01a-47d5-8d7a-8bc97e27859c" />
* Temporary Storage
* Data is auto-deleted after some time
* Used for wizards, temporary forms, or popups