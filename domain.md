📖 راه‌اندازی Nginx برای سرو کردن JSON در مسیر /json

این راهنما توضیح می‌دهد که چطور روی یک سرور Ubuntu/Debian یک فایل JSON بسازیم و طوری تنظیم کنیم که فقط با آدرس:

👉 http://info.vbookapp.work/json

قابل دسترس باشد.

۱. نصب Nginx

ابتدا Nginx را نصب کنید:

sudo apt update
sudo apt install nginx -y

۲. ساخت پوشه و فایل JSON

یک پوشه برای سایت و فایل JSON ایجاد کنید:

sudo mkdir -p /var/www/info.vbookapp.work
sudo nano /var/www/info.vbookapp.work/db.json


محتوای JSON دلخواه را وارد کنید. مثلاً:

{
  "link": "https://example.com/api"
}

۳. تنظیمات Nginx

یک فایل کانفیگ برای دامنه بسازید:

sudo nano /etc/nginx/sites-available/info.vbookapp.work


و این محتوا را داخلش قرار دهید:

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

۴. فعال‌سازی کانفیگ

لینک سیمبولیک بسازید و Nginx را ری‌لود کنید:

sudo ln -s /etc/nginx/sites-available/info.vbookapp.work /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx

۵. تست نهایی

اکنون در مرورگر یا curl بزنید:

http://info.vbookapp.work/json


باید محتوای db.json نمایش داده شود 🎉

✅ با این روش، فقط مسیر /json فایل را برمی‌گرداند و نام فایل (db.json) در URL مخفی می‌ماند.
