# Sandbox Architecture

هر Sandbox یک محیط ایزوله و موقت است که برای یک کاربر و یک Template ساخته می‌شود و در یک Namespace جداگانه در K3s اجرا می‌شود.

---

## 1. ساختار Namespace

هر Sandbox Namespace اختصاصی خودش را دارد.

الگوی نام‌گذاری پیشنهادی:

```
sandbox-<template>-<user-id>-<random>
مثال: sandbox-nginx-basic-u123-a7f2
```

داخل هر Namespace:

- **Application** — سرویس‌هایی که Template تعریف کرده (مثلاً یک Nginx، یک App ساده، یک Database سبک)
- **IDE** — محیط کدنویسی کاربر
- **Terminal** — محیط اجرای دستورات
- **Monitoring Agent** — جمع‌آوری Metrics و Logs

نکته: اینکه Monitoring Stack به‌صورت مشترک اجرا شود و فقط Agent داخل Sandbox باشد، یا کامل داخل هر Sandbox، جزو [تصمیم‌های باز](08-open-decisions.md) است (D-013).

---

## 2. Security Layer

هر Sandbox با این محدودیت‌ها ساخته می‌شود:

- **ResourceQuota** — سقف CPU، Memory و تعداد Pod برای کل Namespace
- **LimitRange** — سقف منابع برای هر Pod و Container
- **NetworkPolicy** — Sandbox فقط به سرویس‌های داخل خودش دسترسی دارد؛ دسترسی به Namespaceهای دیگر مسدود است
- **RBAC** — Service Account هر Sandbox فقط به Namespace خودش دسترسی دارد

سقف‌های عددی منابع به‌ازای هر Template در template.yaml تعریف می‌شود (ببینید: [05-template-specification.md](05-template-specification.md)).

---

## 3. دسترسی کاربر

کاربر فقط از طریق مرورگر وارد Workspace می‌شود و به IDE، Terminal و Monitoring دسترسی دارد.

- هر Sandbox یک آدرس اختصاصی دریافت می‌کند (الگوی پیشنهادی: `sandbox-<id>.<domain>`)
- دسترسی به Terminal باید از طریق HTTPS/WebSocket امن انجام شود
- جزئیات فنی (Ingress، سرتیفیکیت، نحوه Proxy ترمینال) جزو [تصمیم‌های باز](08-open-decisions.md) است (D-008)

---

## 4. Lifecycle

```
Created → Provisioning → Running → Validating → Completed
                                  ↘ Expired ↗
                        (هر دو) → Terminating → Deleted
```

| حالت | توضیح |
|------|-------|
| Created | درخواست ثبت شده و در صف ساخت است |
| Provisioning | Namespace و منابع در حال ساخته شدن هستند |
| Running | Sandbox آماده است و کاربر مشغول حل تمرین است |
| Validating | کاربر درخواست بررسی داده و چک‌ها در حال اجرا هستند؛ بعد از پایان دوباره به Running برمی‌گردد |
| Completed | تمرین تمام شده و نتیجه ثبت شده است |
| Expired | TTL تمام شده است |
| Terminating | منابع در حال حذف هستند |
| Deleted | همه‌چیز پاک شده و فقط رکورد نتیجه باقی می‌ماند |

---

## 5. TTL و Cleanup

- هر Template یک TTL پیش‌فرض دارد (مثلاً 30m یا 60m).
- تمام شدن TTL یعنی Sandbox باید حذف شود؛ حتی اگر کاربر وسط کار باشد.
- Sandboxهای Expired به‌صورت خودکار حذف می‌شوند.
- مکانیزم اجرای Cleanup (Scheduler در Backend یا Controller در Kubernetes) جزو [تصمیم‌های باز](08-open-decisions.md) است (D-012).

قواعد Cleanup:

- Namespace و همه منابع آن کامل حذف می‌شود.
- هیچ داده‌ای از داخل Sandbox نگهداری نمی‌شود.
- Score و Progress کاربر بیرون از Sandbox ذخیره شده‌اند و باقی می‌مانند.

---

## 6. سلامت Sandboxها

برای عملکرد کل سیستم:

- وضعیت و منابع مصرفی هر Sandbox قابل مشاهده است.
- Sandboxهای خراب یا گیرکرده در Provisioning باید شناسایی و پاک شوند تا منابع آزاد بماند.
