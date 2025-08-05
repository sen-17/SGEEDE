# ⚙️ Odoo Actions
In Odoo, actions define what should happen when a user clicks a menu item, a button, or performs a specific operation.

## 🔹 1. Window Actions (ir.actions.act_window)
Most commonly used. Opens a view of a model (form, tree, kanban, etc.).
✅ Use case:
- When opening a model from a menu.
- When clicking a smart button.
<img width="800" height="600" alt="window_functions" src="https://github.com/user-attachments/assets/002ee289-2617-47b1-838c-471cce06f6f0" />

## 🔹 2. Server Actions (ir.actions.server)
Used to run server-side Python code.
✅ Use case:
- Apply Python code to multiple records.
- Call methods from UI buttons.
- Scheduled actions (automations).
<img width="1322" height="758" alt="server_actions" src="https://github.com/user-attachments/assets/5871176e-82dd-497f-9490-43c54b6a8e3b" />

## 🔹 3. Client Actions (ir.actions.client)
Used to open custom web clients or dashboards written in JavaScript.
✅ Use case:
- Custom JS views.
- Web-based dashboards or wizards.
<img width="1282" height="368" alt="client_actions" src="https://github.com/user-attachments/assets/07d26391-8778-401f-9569-2a66d5b8aa42" />

## 🔹 4. URL Actions (ir.actions.act_url)
Redirects to an external or internal URL.
✅ Use case:
- Open a website link.
- Link to an external app or document.
<img width="1482" height="258" alt="url_actions" src="https://github.com/user-attachments/assets/92151225-4366-4e27-84aa-f0cc72079d06" />


## 🔹 5. Report Actions (ir.actions.report)
Used to generate and print reports (PDF, QWeb, etc.).
✅ Use case:
- Print invoice, report cards, salary slip, etc.
<img width="1583" height="257" alt="report_actions" src="https://github.com/user-attachments/assets/097c79a8-7823-4694-9664-91a939249aaf" />