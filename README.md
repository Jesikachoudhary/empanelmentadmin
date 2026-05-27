# IPA Center of Excellence — Empanelment Admin Portal

A production-grade Expression of Interest (EOI) portal built for the **Indian Ports Association (IPA) Center of Excellence**. The system allows domain experts and consultants to submit empanelment applications across 15 specialized port management categories, and provides administrators with a full application management dashboard.

Built with Laravel 11, MySQL, and Docker — deployed with Nginx + PHP-FPM.

---

## What This Does

Organizations applying to be empanelled as advisors to the IPA Center of Excellence use this portal to:
- Register an account with email verification
- Submit a structured EOI with personal details, education history, work experience, and expertise categories
- Upload supporting documents (resume, additional certification)
- Edit their submission before the deadline

Super admins and regional admins use the dashboard to:
- Review, filter, and search all submitted applications
- View per-applicant detail including calculated years of experience
- Export the full applicant list to Excel (XLSX)
- Manage admin accounts with role-based access control

---

## Key Technical Features

**Security**
- Encrypted route model binding — applicant IDs in URLs are AES-encrypted, never exposed as plain integers
- Email verification required before login
- Role-based middleware: Super Admin vs. regular Admin separation
- Password reset via signed email notifications
- CSRF protection on all forms

**Business Logic**
- 15 expertise categories (Projects, Environment & Coastal, Finance, IT, Legal, Disaster Management, HR, and more), each with configurable subcategories — loaded from a central config file, not hardcoded in views
- Server-side enforcement: selecting a main category requires at least one subcategory
- Experience calculator: computes total years and months from multiple employment entries with month-level precision
- Duplicate submission prevention: one EOI per registered account, with redirect to edit on re-attempt
- Document upload validation: PDF/DOC/DOCX only, 2MB resume limit, 5MB additional document limit

**Infrastructure**
- Full Docker setup: PHP-FPM 8.2 + Nginx + MySQL 8.0 + Redis, orchestrated via Docker Compose
- Redis used for session and cache storage
- OpenSpout-based XLSX export for applicant data

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Laravel 11, PHP 8.2 |
| Database | MySQL 8.0 |
| Cache / Sessions | Redis |
| Frontend | Blade templates, Bootstrap, JavaScript |
| File Storage | Laravel Storage (local/S3-compatible) |
| Export | OpenSpout (XLSX) |
| Web Server | Nginx |
| Containerization | Docker, Docker Compose |
| Local Dev (alt) | XAMPP |

---

## Expertise Categories Covered

The portal covers 15 domain areas relevant to port management:

Projects · Environment & Coastal Management · Finance & Taxation · Information Technology · Navigational Audit · Estate & Town Planning · Legal · Disaster Management · Business Development · Human Resources & Welfare · Capacity Building · NDT & SDT · Independent Engineers · Transaction Advisors · Cruise Development

Each category contains 4–6 subcategories configurable in `config/coe_categories.php`.

---

## Installation

### Option A — Docker (Recommended)

```bash
git clone https://github.com/Jesikachoudhary/empanelmentadmin.git
cd empanelmentadmin
cp .env.docker.example .env
docker-compose up -d
docker-compose exec app php artisan key:generate
docker-compose exec app php artisan migrate
```

Access at `http://localhost`

See [DOCKER_SETUP.md](DOCKER_SETUP.md) for full configuration details including SSL and Redis setup.

### Option B — Local (XAMPP)

```bash
git clone https://github.com/Jesikachoudhary/empanelmentadmin.git
cd empanelmentadmin
composer install
cp .env.example .env
php artisan key:generate
# Configure DB credentials in .env
php artisan migrate
php artisan serve
```

Access at `http://localhost:8000`

---

## Environment Variables

Key variables to configure in `.env`:

```env
APP_NAME="IPA CoE Empanelment Portal"
APP_URL=http://localhost

DB_HOST=127.0.0.1
DB_DATABASE=empanelment
DB_USERNAME=root
DB_PASSWORD=

MAIL_MAILER=smtp
MAIL_HOST=your-smtp-host
MAIL_PORT=587
MAIL_USERNAME=your-email
MAIL_PASSWORD=your-password
MAIL_FROM_ADDRESS=noreply@yourdomain.com

CACHE_DRIVER=redis
SESSION_DRIVER=redis
```

---

## Project Structure

```
app/
├── Http/
│   ├── Controllers/
│   │   ├── AdminApplicantController.php   # Core EOI CRUD + export (795 lines)
│   │   ├── AdminAuthController.php        # Registration, login, verification
│   │   ├── AdminForgotPasswordController.php
│   │   └── AdminResetPasswordController.php
│   └── Middleware/
│       └── AdminSuper.php                 # Super admin role guard
├── Models/
│   ├── Applicant.php                      # Encrypted routing, experience calc
│   ├── ApplicantEducation.php
│   ├── ApplicantExperience.php
│   └── Admin.php
config/
└── coe_categories.php                     # All 15 categories + subcategories
```

---

## Admin Roles

| Role | Can Do |
|---|---|
| Super Admin | Manage all admins, view all applications, export data, delete records |
| Admin | Submit own EOI, view own application, edit before deadline |

Super admin accounts are created via registration code — not open to public registration.

---

## Applicant Guide

A complete step-by-step guide for applicants is available in [APPLICANT_GUIDE.md](APPLICANT_GUIDE.md), covering account setup, form completion, document upload, and submission confirmation.

---

## Developer

**Jesika Choudhary**  
B.Tech Information Technology, Manipal University Jaipur  
jesika.23fe10ite00069@muj.manipal.edu

---

## License

MIT License — see [LICENSE](LICENSE) for details.
