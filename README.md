# Kaltim Smart Platform

LKS Cloud Computing 2026 - Provinsi Kalimantan Timur

**Peserta:** Gabriel Ado Ramos Tukan
**Kode Peserta:** GAR-2842AEED

[ALB URL: http://kaltim-smart-platform-alb-1585642066.ap-southeast-1.elb.amazonaws.com/](http://kaltim-smart-platform-alb-1585642066.ap-southeast-1.elb.amazonaws.com/)

![Route 53 URL: http://kaltimsmart.gabriel-smkn7.click](http://kaltimsmart.gabriel-smkn7smr.click/)

![Postman Collection](docs/Kaltim-Smart-Platform.postman_collection.json)

![Screenshot CloudWatch Dashboard](docs/screenshot_cloudwatch_dashboard.png)

![Cloudtrail Presigned URL](https://aws-cloudtrail-logs-492976517400-aaddee1c.s3.ap-southeast-1.amazonaws.com/AWSLogs/492976517400/CloudTrail/ap-southeast-1/2026/05/25/?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAXFR5QWEMPYCNHN66%2F20260625%2Fap-southeast-1%2Fs3%2Faws4_request&X-Amz-Date=20260625T065000Z&X-Amz-Expires=3600&X-Amz-SignedHeaders=host&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aDmFwLXNvdXRoZWFzdC0xIkgwRgIhAOn1sJ6NBRXd3OL47SydBuVj6TgiFuLD7o27Z%2BLWMpIOAiEA%2Bz6eMZY8DhMxV2EEE1Wg%2FS6PksOEdAEACpCcG9Qups0q5AIISBACGgw0OTI5NzY1MTc0MDAiDBYdwM%2BXM4lS4jyJhSrBAtTVMnzMhZH1MQtuAeyEuA4atbZiaBTjprXfgoxrpLOy4N5J9YcNKqlzB5%2FHe4%2FNXdRYVihdaW%2FdcHd8a03h8xpVAVQMbu5MmdoatuAYYcCaRxBxl%2BHr2qNSzIo3KvVrvyhHltjY4KotigFEREfLS2MMRkO8CskzkMcFM215E5f4%2BBofKWBhQXjF93GKqix6LKEwM6Dw1ZUpNCrB0HPuK6yQp3H9tmFoBbpa1XGqA7ga4wzSCDVvSyXSWus8%2BXg5e4MLDDcwGPLzggbIyp4UWrLtpIaJmpOs9O2xpbhg09fb5jzvPhX4m5ncpqLntsn9LI%2BOGSMq2jM2JO82B7DZ%2FLbrgobdawIpQCXOQpNZx8QfDZVrsaxkzoDtiYEV5Qy2c2fcSDpsHlxLDCQ4%2BpfAPJ6pE4dQcVQKqdwHoO%2FfHaOe0TDv5%2FHRBjqsAgUJ0r21BYa02JrTa5ncnrVlHn%2F9IPQ61q%2BMo84m4iViIuVNMvWhkz1Y3T68WZl3EgeDo2Ffuui%2BreG7Yqt4F9H5ncTSqiMX9LzMO1TSr31Vxm1wG9YkLSMXIDrluvKvuBnlztoUWuke3GKjnn3gG8nS0Inezo8oOYObhJ8AWf5cdkqnTydUkPe%2BeD8UPuviFeN3BdFh7Wzp9YXlQzqX9XbhMPYCtkmyASkPnYVd4lyj%2F6V3HSz%2BjjXv0lKFYYasWofIA9PLO0PGcnCbV7BQtJ4sMHOyLYgu068LdByGnZSc8KIecMeTZD2C1Lx8QaXgxvY41FCP%2B3rXa3Oj%2FL1B9%2BpPMjsbdkdmAzHhh2Z2xGguibvjlxlvo9Qg1LFbQ96GGnPamCDo%2BmacsTUBUQ%3D%3D&X-Amz-Signature=67eb7e34c1b8348ee50f70f9d327ff65eff03f8102d8e85142fd8e5b40ee5e23)

## Deskripsi

Platform layanan publik digital berbasis cloud untuk Pemerintah Provinsi Kalimantan Timur. Aplikasi ini menyediakan REST API dan web interface untuk:

- Autentikasi pengguna (warga & admin) dengan JWT
- Pengajuan layanan publik secara digital (KTP, KK, Akta, Izin Usaha)
- Pelaporan masalah warga (infrastruktur, lingkungan, sosial)
- Notifikasi real-time saat status layanan berubah
- Dashboard analitik untuk pejabat daerah
- Chatbot AI (Amazon Lex) untuk tanya-jawab layanan publik
- Health check menyeluruh (database, Redis, storage)

## Teknologi

| Kategori | Teknologi |
|---|---|
| Backend | Laravel 13 (PHP 8.4) |
| Autentikasi | JWT (JSON Web Token) + RBAC |
| Database | MySQL 8.0 (5 tabel dengan foreign key) |
| Cache | Redis |
| Container | Docker + Docker Compose (multi-stage build, non-root user) |
| Infrastruktur | AWS (EC2 ASG, RDS, S3, ElastiCache, ALB, Lex) |
| IaC | Terraform (VPC, subnets, SG, IAM) |
| AI Chatbot | Amazon Lex (Bahasa Indonesia) |

## Arsitektur

![Architecture Diagram](docs/architecture-diagram.png)

```
Internet → ALB (Public Subnet) → EC2 App (Private Subnet) → RDS + ElastiCache
           S3 (File Uploads)      Lex (Chatbot AI)
```

## Menjalankan Secara Lokal

### Dengan Docker (Recommended)

```bash
git clone https://github.com/mrgart64/lks-kaltim-2026-GAR-2842AEED.git
cd lks-kaltim-2026-GAR-2842AEED

# Generate key
php artisan key:generate --show   # copy output
php artisan jwt:secret --show     # copy output (jika sudah ada JWT_SECRET di .env, skip)

# Setup environment
cp docker/.env.example docker/.env
# Edit docker/.env - isi APP_KEY, JWT_SECRET, DB_PASSWORD

# Jalankan
cd docker
docker compose up -d --build
```

Buka `http://localhost:8080`

### Development (Tanpa Docker)

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan jwt:secret
php artisan migrate --seed
php artisan serve --port=8080
```

## Akun Demo

| Role | Email | Password |
|---|---|---|
| Admin | admin@kaltim.go.id | password |
| Warga | budi@email.com | password |
| Warga | siti@email.com | password |

## Halaman Web

| URL | Keterangan |
|---|---|
| `/` | Landing page |
| `/login` | Login warga/admin |
| `/register` | Registrasi warga |
| `/citizen/dashboard` | Dashboard warga |
| `/citizen/services` | Ajukan & lihat layanan |
| `/citizen/reports` | Buat & lihat laporan |
| `/admin/dashboard` | Dashboard admin (statistik) |
| `/admin/services` | Kelola permintaan layanan |
| `/admin/reports` | Kelola laporan warga |
| `/chatbot` | Asisten virtual (full page) |
| `/health` | Health check (HTML) |
| `/api-info` | Dokumentasi API |

## Endpoint API

### Autentikasi
- `POST /api/auth/register` - Registrasi warga
- `POST /api/auth/login` - Login, dapat JWT token
- `POST /api/auth/logout` - Logout (Auth)
- `GET /api/auth/profile` - Profil (Auth)

### Layanan Publik
- `GET /api/services` - Daftar jenis layanan
- `POST /api/services/request` - Ajukan permintaan (Auth)
- `GET /api/services/request/{id}` - Detail permintaan (Auth)
- `PUT /api/services/request/{id}/status` - Update status (Admin)
- `GET /api/services/requests` - Daftar permintaan (Auth)

### Laporan Warga
- `POST /api/reports` - Kirim laporan (Auth)
- `GET /api/reports` - Daftar laporan (Auth)
- `GET /api/reports/{id}` - Detail laporan (Auth)
- `PUT /api/reports/{id}` - Update laporan (Auth)

### Notifikasi
- `GET /api/notifications` - Daftar notifikasi (Auth)

### Dashboard Admin
- `GET /api/dashboard/stats` - Statistik ringkasan (Admin)
- `GET /api/dashboard/reports/summary` - Rekapitulasi per kategori (Admin)

### Lainnya
- `GET /api/health` - Health check (JSON)
- `POST /api/chatbot` - Chatbot AI (Amazon Lex / rule-based)

## Environment Variables

| Variable | Deskripsi | Default |
|---|---|---|
| `APP_KEY` | Laravel application key | (wajib) |
| `JWT_SECRET` | JWT signing secret | (wajib) |
| `DB_CONNECTION` | Database driver | mysql |
| `DB_HOST` | Database host | db |
| `DB_PORT` | Database port | 3306 |
| `DB_DATABASE` | Database name | kaltim_smart_platform |
| `DB_USERNAME` | Database user | kaltim_user |
| `DB_PASSWORD` | Database password | (wajib) |
| `CACHE_STORE` | Cache driver | redis |
| `SESSION_DRIVER` | Session driver | redis |
| `REDIS_HOST` | Redis host | cache |
| `REDIS_PORT` | Redis port | 6379 |
| `FILESYSTEM_DISK` | Upload storage driver | public (s3 untuk AWS) |
| `AWS_ACCESS_KEY_ID` | AWS access key | (untuk S3 & Lex) |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key | (untuk S3 & Lex) |
| `AWS_DEFAULT_REGION` | AWS region | ap-southeast-1 |
| `AWS_BUCKET` | S3 bucket name | (untuk S3) |
| `AWS_LEX_BOT_ID` | Lex bot ID | (opsional, AI chatbot) |
| `AWS_LEX_BOT_ALIAS_ID` | Lex bot alias ID | (opsional, AI chatbot) |

## Deploy ke AWS

Lihat [deployment-guide.md](docs/deployment-guide.md) — panduan step-by-step lengkap termasuk:
- Setup AWS Account & IAM
- Deploy infrastruktur dengan Terraform
- Konfigurasi Amazon Lex chatbot
- Verifikasi & monitoring

## Struktur Project

```
├── app/
│   ├── Http/
│   │   ├── Controllers/        # API + Web controllers + Chatbot + Health
│   │   │   └── Web/            # Web interface controllers
│   │   └── Middleware/          # RoleMiddleware (RBAC)
│   └── Models/                  # User, ServiceType, ServiceRequest, Report, Notification
├── database/
│   ├── migrations/              # 5 tabel migrasi
│   └── seeders/                 # DatabaseSeeder (admin + citizen + sample data)
├── routes/
│   ├── api.php                  # API routes (JWT auth)
│   └── web.php                  # Web UI routes (session auth)
├── resources/views/
│   ├── layouts/app.blade.php    # Layout web utama
│   ├── welcome.blade.php        # Landing page
│   ├── auth/                    # Login & register
│   ├── citizen/                 # Dashboard warga, services, reports
│   ├── admin/                   # Dashboard admin, kelola layanan & laporan
│   ├── chatbot.blade.php        # Halaman chatbot
│   ├── health.blade.php         # Health check UI
│   └── api-info.blade.php       # Dokumentasi API
├── docker/
│   ├── Dockerfile               # Multi-stage build (non-root user)
│   ├── docker-compose.yml       # App + Nginx + MySQL + Redis
│   ├── nginx.conf               # Nginx reverse proxy ke PHP-FPM
│   ├── entrypoint.sh            # Auto migrate, seed, dan optimize
│   └── .env.example
├── terraform/
│   ├── main.tf                  # Provider config
│   ├── variables.tf             # Semua variabel
│   ├── outputs.tf               # Output values
│   ├── vpc.tf                   # VPC, subnets, IGW, NAT, route tables
│   ├── ec2.tf                   # ALB, ASG, launch template
│   ├── rds.tf                   # RDS MySQL 8.0
│   ├── s3.tf                    # S3 bucket (versioned, encrypted)
│   ├── elasticache.tf           # Redis cluster
│   ├── security.tf              # Security Groups (least privilege)
│   └── lex.tf                   # Amazon Lex chatbot bot
├── docs/
│   ├── architecture-diagram.png
│   ├── api-documentation.md
│   └── deployment-guide.md
├── .env.example
└── README.md
```
