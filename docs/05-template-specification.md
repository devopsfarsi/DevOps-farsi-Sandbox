# Template Specification

Template یک Lab آموزشی قابل اجرا است. همه‌چیز درباره یک تمرین — از سناریو و منابع گرفته تا Taskها و روش سنجش — داخل Template تعریف می‌شود.

---

## 1. ساختار پوشه

```
templates/nginx-basic/
├── template.yaml      ← تعریف اصلی Template
├── manifests/         ← Manifestهای Kubernetes که در Sandbox اجرا می‌شوند
├── tasks/             ← شرح Taskها و راهنمای هر مرحله
├── tests/             ← چک‌های Validation
└── README.md          ← مستندات آموزشی Template برای کاربر
```

---

## 2. template.yaml

فایل اصلی هر Template است و باید شامل این بخش‌ها باشد:

| بخش | توضیح |
|-----|-------|
| Metadata | نام، عنوان، توضیح، نسخه، نویسنده، تگ‌ها |
| Difficulty | سطح دشواری: easy / medium / hard |
| Duration | مدت زمان تخمینی تمرین |
| Skills | مهارت‌هایی که کاربر تمرین می‌کند |
| Resources | سقف CPU و Memory لازم برای اجرا |
| TTL | مدت زمان مجاز بودن Sandbox |
| Tasks | لیست مراحل که کاربر باید انجام دهد |
| Validation | چک‌هایی که موفقیت هر Task را می‌سنجند |
| Documentation | README آموزشی داخل پوشه Template |

---

## 3. نمونه

```yaml
# template.yaml
metadata:
  name: nginx-basic
  title: Nginx Reverse Proxy
  description: یک سرویس وب پشت Reverse Proxy راه بیاندازید و مشکل Routing را حل کنید.
  version: 0.1.0
  author: github-username
  difficulty: easy
  duration: 30m
  tags: [nginx, networking, proxy]
  skills: [nginx, reverse-proxy, debugging]

spec:
  ttl: 45m
  resources:
    maxCpu: "500m"
    maxMemory: "512Mi"
  monitoring:
    enabled: true

  tasks:
    - id: task-1
      title: سرویس Nginx را بالا بیاورید
      description: سرویسی که پشت Proxy قرار می‌گیرد فعال نیست؛ آن را راه‌اندازی کنید.
      hints:
        - وضعیت Podها و لاگ‌ها را بررسی کنید.

    - id: task-2
      title: Routing را درست کنید
      description: درخواست به مسیر /app باید از Proxy به سرویس پشتی برسد.
      hints:
        - به upstream تعریف‌شده در configuration دقت کنید.

  validation:
    - task: task-1
      checks:
        - type: service-state
          expect: running

    - task: task-2
      checks:
        - type: http-check
          path: /app
          expect: 200
```

انواع Check در سطح مفهومی:

- **http-check** — یک درخواست HTTP به داخل Sandbox می‌زند و پاسخ را می‌سنجد
- **command-check** — یک دستور داخل Sandbox اجرا می‌کند و خروجی یا کد خروج را می‌سنجد
- **service-state** — وضعیت منابع (Running، تعداد Replica و ...) را می‌سنجد

موتور اجرای Validation هنوز انتخاب نشده و جزو [تصمیم‌های باز](08-open-decisions.md) است (D-014).

---

## 4. قواعد نگارش Template

- نام پوشه و name باید یکسان و kebab-case باشد؛ مثل `nginx-basic`
- هر Template یک نسخه (Semantic Version) دارد.
- محتوای آموزشی فارسی و نام‌های فنی انگلیسی نوشته می‌شود.
- Sandbox نباید به اینترنت یا منابع بیرونی وابسته باشد.
- منابع هر Template باید سبک باشد تا تعداد Sandboxهای همزمان قابل بالا بردن باشد.
- راه‌حل هرگز در جایی که کاربر به آن دسترسی دارد قرار نمی‌گیرد.

راهنمای کامل قدم‌به‌قدم ساخت Template: [07-template-authoring-guide.md](07-template-authoring-guide.md)

---

## 5. چک‌لیست ریویو Template

- [ ] template.yaml با اسکیمای بالا هم‌خوان است
- [ ] Validation واقعاً رفتار درست را می‌سنجد (نه فقط وجود یک فایل)
- [ ] Template بدون اینترنت کار می‌کند
- [ ] منابع مصرفی معقول و سبک است
- [ ] README آموزشی کامل است
- [ ] در یک کلاستر محلی اجرا و تأیید شده است (ابزار تست محلی: D-016)
