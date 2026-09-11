# System Architecture

معماری DevOps Farsi Sandbox شامل چند لایه اصلی است.

این سند عمداً **فناوری‌بی‌طرف** نوشته شده است: انتخاب زبان، فریم‌ورک و ابزارهای مشخص هنوز به‌صورت رسمی قطعی نشده و جزو [تصمیم‌های باز](08-open-decisions.md) است. دیاگرام موجود در README یک **پیشنهاد اولیه** برای شروع بحث است، نه معماری نهایی.

---

## نمای کلی

```
Browser
  └── Frontend (Web App)
        └── Backend API
              ├── Sandbox Manager
              │     └── Kubernetes (K3s)
              ├── Database
              └── Queue / Cache (برای کارهای async)
```

---

## 1. Frontend

مسئول رابط کاربری.

وظایف:

- صفحه ورود (Login)
- Dashboard و لیست Templateها به‌همراه Filterها
- صفحه جزئیات Template
- Workspace UI — نمایش IDE، Terminal و Monitoring
- نمایش نتیجه Validation و Score

---

## 2. Backend (API)

مرکز کنترل سیستم و محل اجرای منطق تجاری.

وظایف:

- User — ثبت و شناسایی کاربر
- Template — نگهداری و ارائه لیست Templateها
- Sandbox — چرخه حیات Sandbox (ساخت، وضعیت، حذف)
- Validation — اجرای چک‌های هر Task
- Score — محاسبه و ذخیره نتیجه

اصول:

- Backend مستقیماً با Kubernetes صحبت نمی‌کند؛ این کار از طریق Sandbox Manager انجام می‌شود.
- همه عملیات باید Idempotent باشند؛ چون ساخت Sandbox ممکن است وسط راه قطع و دوباره ادامه پیدا کند.
- Backend Stateless طراحی می‌شود تا در آینده قابل Scale باشد؛ State در Database نگهداری می‌شود.

---

## 3. Sandbox Manager

لایه ارتباط با Kubernetes. Backend از طریق این لایه زیرساخت را می‌سازد و پاک می‌کند.

وظایف:

- ساخت Namespace اختصاصی برای هر Sandbox
- اعمال Security Layer (ResourceQuota، LimitRange، NetworkPolicy، RBAC)
- Deploy کردن Manifestهای Template
- بررسی سلامت (Health Check) و وضعیت Sandbox
- اجرای TTL و حذف خودکار Sandboxهای منقضی‌شده

---

## 4. Kubernetes Layer

K3s مسئول اجرای Sandboxها است.

هر Sandbox در یک Namespace جدا اجرا می‌شود:

- Application — سرویس‌های Template
- IDE
- Terminal
- Monitoring

جزئیات داخل Sandbox در [04-sandbox-architecture.md](04-sandbox-architecture.md) آمده است.

---

## 5. State و داده‌ها

در سطح مفهومی، سیستم به این ذخیره‌سازی‌ها نیاز دارد:

- Database — کاربران، Templateها، Score و وضعیت Sandboxها
- Queue / Cache — کارهای async مثل ساخت و حذف Sandbox و صف ظرفیت

انتخاب محصول مشخص برای هرکدام جزو [تصمیم‌های باز](08-open-decisions.md) است (D-005 و D-006). اینکه Object Storage در MVP لازم باشد یا نه (D-007) هم در همین فهرست است.

---

## 6. اصول طراحی

- **Isolation** — Sandboxها هیچ دسترسی به هم یا به سیستم اصلی ندارند.
- **Least Privilege** — هر جزء فقط حداقل دسترسی لازم را دارد.
- **Everything as Code** — همه Sandboxها فقط از Template و Manifest ساخته می‌شوند؛ هیچ‌چیز دستی راه نمی‌افتد.
- **Observability** — Backend و Sandboxها باید قابل مانیتور و لاگ باشند.
- **Recoverability** — خرابی یک جزء نباید کل سیستم را از کار بیندازد؛ عملیات ناتمام باید قابل ادامه یا قابل پاک‌سازی باشند.
