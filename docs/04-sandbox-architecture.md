# Sandbox Architecture


هر Sandbox یک Namespace جدا دارد.


Example:

user-123
App
IDE
Terminal
Monitoring
---

# Security


هر Sandbox:


- ResourceQuota
- LimitRange
- NetworkPolicy
- RBAC


دارد.


---

# Lifecycle

Create - Provision - Running - Finish - Delete

---

# Cleanup


Sandboxهای Expired باید خودکار حذف شوند.
