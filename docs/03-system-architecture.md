# System Architecture


معماری DevOps Farsi Sandbox شامل چند بخش اصلی است.


## Frontend

مسئول UI.


وظایف:

- Login
- Dashboard
- Template View
- Workspace


---

## Backend

مرکز کنترل سیستم.


وظایف:

- User
- Template
- Sandbox
- Score


---

## Sandbox Manager

لایه ارتباط با Kubernetes.


وظایف:

- Create Namespace
- Deploy Template
- Delete Sandbox


---

## Kubernetes Layer


K3s مسئول اجرای Sandboxها است.

K3s:
-Namespace
--Application
--IDE
--Terminal
--Monitoring
