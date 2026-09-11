# DevOps Farsi Sandbox

## معرفی

DevOps Farsi Sandbox یک پلتفرم آزمایشگاهی برای یادگیری عملی DevOps است.

کاربر وارد سایت می‌شود، یک تمرین (Template) انتخاب می‌کند و یک Sandbox ایزوله اختصاصی دریافت می‌کند. داخل Sandbox به IDE، Terminal و Monitoring دسترسی دارد و باید یک مسئله واقعی DevOps را حل کند — بدون نیاز به راه‌اندازی هیچ زیرساختی.

هدف: مهندس‌های DevOps بتوانند سناریوهای نزدیک به Production را تمرین کنند، مشکلات را Debug کنند و مهارت‌هایشان را در شرایط واقعی ارتقا دهند.

---

## داکیومنت‌ها

| سند | محتوا |
|-----|-------|
| [معرفی محصول](docs/01-product-overview.md) | چیستی پروژه، مخاطب هدف، ارزش پیشنهادی |
| [جریان کاربر](docs/02-user-flow.md) | مسیر کاربر از ورود تا Cleanup |
| [معماری سیستم](docs/03-system-architecture.md) | لایه‌ها، مسئولیت‌ها، اصول طراحی |
| [معماری Sandbox](docs/04-sandbox-architecture.md) | ایزوله‌سازی، امنیت، Lifecycle، Cleanup |
| [مشخصات Template](docs/05-template-specification.md) | ساختار و اسکیمای Labهای آموزشی |
| [راهنمای مشارکت](docs/06-contributor-guide.md) | شروع مشارکت، فرآیند Issue و PR |
| [راهنمای نگارش Template](docs/07-template-authoring-guide.md) | قدم‌به‌قدم ساخت Lab آموزشی |
| [تصمیم‌های باز](docs/08-open-decisions.md) | فهرست تصمیم‌های فنی و مکانیزم تصمیم‌گیری |

---

## معماری پیشنهادی

![معماری پیشنهادی DevOps Farsi Sandbox](docs/images/DevopsFarsi-Sandbox-Architecture-diagram.png)

> این دیاگرام یک **پیشنهاد اولیه** است، نه معماری قطعی. انتخاب استک فناوری (Backend، Frontend، Database و ...) هنوز انجام نشده و به موج اول کانتربیوترها سپرده می‌شود؛ فهرست کامل: [تصمیم‌های باز](docs/08-open-decisions.md). در نسخه فعلی تصویر چند ایراد تایپی جزئی وجود دارد که در بازسازی بعدی اصلاح می‌شود.

معماری در سطح لایه‌ها ساده است: مرورگر → Frontend → Backend API → Sandbox Manager → Kubernetes (K3s). جزئیات: [معماری سیستم](docs/03-system-architecture.md)

---

## Roadmap

| فاز | هدف | امکانات کلیدی |
|-----|-----|---------------|
| 1 — MVP | اولین Sandbox قابل استفاده | ورود ساده، انتخاب Template، ساخت Namespace، Workspace، Terminal، Monitoring، Validation پایه |
| 2 — Learning Engine | سیستم یادگیری | Taskها، ارزیابی خودکار، Score، Progress، Leaderboard |
| 3 — Incident Training | تمرین Incident | Fault Injection، سناریوهای شبیه Production، امتیازدهی Incident |
| 4 — AI Assistant | هوشمندسازی | Incident تولیدشده با AI، Hint، پیشنهاد یادگیری، تحلیل کاربر |

---

## مشارکت

این پروژه توسط کامیونیتی ساخته می‌شود — و ساختنش خودش یک تجربه یادگیری است. کانتربیوترها با تسک‌های کوچک و مشخص زیرساخت را طراحی می‌کنند و در نهایت تجربه و رزومه واقعی می‌سازند.

زمینه‌های مشارکت:

- **Backend** — API، Sandbox Lifecycle، Validation Engine
- **Frontend** — Dashboard، Template UI، Workspace UI
- **DevOps / Kubernetes** — K3s، Helm، Namespace Isolation، Security
- **Monitoring** — Prometheus، Grafana، Loki
- **Template** — طراحی و نگارش Labهای آموزشی
- **Docs** — داکیومنت و راهنماها

شروع کار: [راهنمای مشارکت](docs/06-contributor-guide.md)

---

## وضعیت پروژه

داکیومنت‌های پایه آماده شده‌اند. تسک‌های فاز 1 به‌زودی به‌صورت Issue منتشر می‌شوند. لینک‌های کامیونیتی بعد از اطلاع‌رسانی رسمی در همین بخش اضافه می‌شود.
