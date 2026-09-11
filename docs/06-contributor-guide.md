# راهنمای مشارکت (Contributor Guide)

این سند توضیح می‌دهد چطور در DevOps Farsi Sandbox مشارکت کنید.

---

## 1. چرا مشارکت ارزشمند است؟

این پروژه توسط کامیونیتی ساخته می‌شود. کانتربیوترها:

- زیرساخت یک سرویس واقعی را طراحی و پیاده‌سازی می‌کنند
- با Git، Code Review و همکاری تیمی در یک پروژه Open Source تمرین می‌کنند
- برای رزومه‌شان پروژه‌ای می‌سازند که نشان می‌دهد زیرساخت واقعی ساخته‌اند، نه فقط تمرین شخصی

---

## 2. ساختار ریپو

```
README.md                    ← معرفی پروژه و لینک‌ها
docs/                        ← داکیومنت‌ها (شماره‌گذاری‌شده)
  01-product-overview.md
  02-user-flow.md
  03-system-architecture.md
  04-sandbox-architecture.md
  05-template-specification.md
  06-contributor-guide.md    ← همین سند
  07-template-authoring-guide.md
  08-open-decisions.md
templates/                   ← Templateهای آموزشی (از فاز 1)
src/                         ← کد اصلی (با شروع MVP)
```

---

## 3. برنچ‌ها

- **main** — نسخه پایدار؛ فقط با Merge از develop به‌روز می‌شود.
- **develop** — کارهای جاری؛ همه PRها به develop فرستاده می‌شوند.

نام‌گذاری برنچ:

```
feat/<topic>       مثال: feat/sandbox-manager
fix/<topic>        مثال: fix/ttl-cleanup
docs/<topic>       مثال: docs/contributor-guide
```

---

## 4. فرآیند کار

1. یک Issue از لیست انتخاب کنید (ترجیحاً `good first issue` برای شروع).
2. زیر ایشو کامنت بگذارید و اعلام کنید کار برعهده شماست؛ لیبل `claimed` اضافه می‌شود.
3. برنچ بسازید، کار کنید و Commitهای تمیز بزنید.
4. PR به برنچ `develop` بفرستید و ایشو را لینک کنید.
5. بعد از تأیید Review، PR Merge می‌شود و ایشو بسته می‌شود.

قواعد:

- قبل از شروع حتماً ایشو را Claim کنید تا دو نفر روی یک کار وقت نگذارند.
- هر PR کوچک و متمرکز باشد؛ یک PR = یک هدف مشخص.
- اگر کاری بیش از چند روز طول کشید، در ایشو گزارش پیشرفت بدهید.

---

## 5. سایز تسک‌ها

- `size/S` — چند ساعت کار؛ مناسب شروع
- `size/M` — یک تا چند روز
- `size/L` — بیش از یک هفته؛ معمولاً باید به تسک‌های کوچک‌تر شکسته شود

مدل ما «تسک‌های کوچک» است. اگر ایشویی خیلی بزرگ است، در ایشو پیشنهاد دهید شکسته شود.

---

## 6. برچسب‌ها (Labels)

| لیبل | معنی |
|------|------|
| `area/backend` / `area/frontend` / `area/devops` / `area/monitoring` / `area/docs` / `area/template` | حوزه کاری |
| `size/S` / `size/M` / `size/L` | حجم کار |
| `good first issue` | مناسب شروع |
| `help wanted` | نیاز به کانتربیوتر |
| `decision` | مربوط به یک تصمیم باز (ADR) |
| `claimed` | کسی برعهده گرفته است |
| `in-review` | در حال ریویو |

---

## 7. تصمیم‌های فنی (ADR)

تصمیم‌های معماری از طریق [08-open-decisions.md](08-open-decisions.md) مدیریت می‌شوند:

1. برای هر تصمیم یک Issue با لیبل `decision` باز می‌شود.
2. گزینه‌ها با مزایا و معایب در ایشو بحث می‌شوند.
3. تصمیم نهایی با یک PR در فایل 08 ثبت می‌شود.

---

## 8. استانداردها

- داکیومنت‌ها **فارسی** نوشته می‌شوند؛ نام‌های فنی و کد انگلیسی می‌مانند.
- نام‌گذاری در کد و Commentها انگلیسی است.
- هر تغییر کد که رفتار سیستم را عوض می‌کند باید داکیومنت مرتبط را هم به‌روز کند.
- استاندارد Commit و برنچ (D-017) پس از تصمیم‌گیری ثبت می‌شود.

---

## 9. زمینه‌های مشارکت

- **Backend** — API، Sandbox Lifecycle، Validation Engine
- **Frontend** — Dashboard، Template UI، Workspace UI
- **DevOps / Kubernetes** — K3s، Helm، Isolation، Security، CI/CD
- **Monitoring** — Prometheus، Grafana، Loki
- **Template** — طراحی و نگارش Labهای آموزشی (راهنما: سند 07)
- **Docs** — بهبود داکیومنت‌ها و راهنماها

---

## 10. ارتباط با تیم

https://t.me/DevopsFarsi_ir
https://www.linkedin.com/company/devops-farsi

جهت مدیریت بهینه پروژه، پس از همراهی مشارکت‌کنندگان گروهی در تلگرام ایجاد خواهد شد.