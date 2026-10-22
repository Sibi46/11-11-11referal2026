# Production deployment

- Site: https://www.111111perfume.com (also serves https://111111perfume.com)
- Server: 64.227.168.99
- Source: /var/www/111111perfume
- Public document root: /var/www/111111perfume/public
- Backend: django_backend/evoxu, Gunicorn on 127.0.0.1:8011
- Service: 111111perfume.service (enabled at boot)
- Nginx: /etc/nginx/sites-available/111111perfume
- Database: perfume111111, dedicated MySQL user with access only to that database
- Environment: django_backend/.env on server; never commit it
- Admin credentials: /root/111111perfume-admin.txt on server (root only)
- TLS: existing Certbot certificate for both domains, nginx renewal authenticator

## Update

Back up the database and media before updates. Pull main from https://github.com/Sibi46/11-11-11referal2026.git, install django_backend/requirements.txt in .venv, run migrations and collectstatic from django_backend, copy the frontend HTML and assets into public, and restart 111111perfume.service. Keep backend source, environment files, SQL, and Git metadata outside public.

## Initial database migration issue

Two existing resellers 0004 migrations add the same fields. For this new database only, 0004_payout_payment_fields was applied normally, then 0004_resellerpayout_notes_resellerpayout_payment_mode_and_more was marked applied with --fake because its schema operations are identical. The remaining migrations, including the merge, were then applied normally. Do not repeat this procedure on an existing database without checking its migration history and schema.

## Operations

```sh
systemctl status 111111perfume
journalctl -u 111111perfume -n 100 --no-pager
nginx -t
certbot certificates
```

No demonstration resellers or orders were seeded. The product migration supplies the initial catalog. Payment and OTP integrations require separate verification before accepting paid orders.
