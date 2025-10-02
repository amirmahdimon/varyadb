# 📖 راه‌اندازی Nginx برای سرو کردن JSON در مسیر `/json`

این راهنما نحوه‌ی ساخت و ارائه یک فایل JSON از طریق Nginx را روی سرور Ubuntu/Debian توضیح می‌دهد؛ به طوری که فقط با آدرس زیر قابل دسترسی باشد:

```
http://info.vbookapp.work/json
```

---

## ۱. نصب Nginx

ابتدا Nginx را نصب کنید:

```bash
sudo apt update
sudo apt install nginx -y
```

---

## ۲. ساخت پوشه و فایل JSON

یک پوشه برای سایت و فایل JSON ایجاد کنید:

```bash
sudo mkdir -p /var/www/info.vbookapp.work
sudo nano /var/www/info.vbookapp.work/db.json
```

محتوای دلخواه JSON را وارد کنید. مثلا:

```json
{
  "link": "https://example.com/api"
}
```

---

## ۳. تنظیمات Nginx

یک فایل کانفیگ برای دامنه بسازید:

```bash
sudo nano /etc/nginx/sites-available/info.vbookapp.work
```

و این محتوا را در آن قرار دهید:

```nginx
server {
    listen 80;
    server_name info.vbookapp.work;

    root /var/www/info.vbookapp.work;

    # مسیر /json → فایل db.json
    location = /json {
        default_type application/json;
        alias /var/www/info.vbookapp.work/db.json;
    }
}
```

---

## ۴. فعال‌سازی کانفیگ

لینک سیمبولیک بسازید و Nginx را ری‌لود کنید:

```bash
sudo ln -s /etc/nginx/sites-available/info.vbookapp.work /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

## ۵. تست نهایی

در مرورگر یا با دستور curl این آدرس را تست کنید:

```
http://info.vbookapp.work/json
```

باید محتوای `db.json` نمایش داده شود. 🎉

---

> ✅ با این روش، فقط مسیر `/json` فایل را برمی‌گرداند و نام فایل (`db.json`) در URL مخفی می‌ماند.
