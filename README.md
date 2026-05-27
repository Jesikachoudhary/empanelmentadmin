# IPA Center of Excellence — Empanelment Portal

This is a web application I built for the **Indian Ports Association (IPA) Center of Excellence** empanelment process. The portal handles the complete Expression of Interest (EOI) workflow — applicants register, fill their profile, and submit their expertise details; a super admin reviews all submissions from a central dashboard.

The project came out of a real requirement: IPA needed a structured digital system to collect and manage applications from domain experts across port management areas.

---

## What the system does

**For applicants:**
- Register with email and verify account before login
- Fill a single-page EOI form covering personal details, educational qualifications, and work experience
- Select expertise from 15 domain categories (each with subcategories) — Projects, Environment & Coastal Management, Finance, IT, Legal, HR, Disaster Management, and more
- Upload resume and supporting documents
- Edit submission anytime before the deadline
- Only one submission allowed per account

**For super admin:**
- View all submitted applications in a searchable, filterable dashboard
- See per-applicant breakdown including auto-calculated years of experience
- Export the full applicant list to Excel
- Manage admin accounts and registrations

---

## Some specific things I implemented

**Encrypted applicant IDs in URLs** — instead of exposing `/applicants/1`, `/applicants/2` in the browser, every applicant ID is AES-encrypted before it appears in any URL. This was done deliberately so applicant records can't be enumerated.

**Experience calculator** — the system calculates total years and months of experience automatically from multiple employment entries (each with from/to month and year). It handles overlapping periods and returns a decimal like 4.5 years.

**Category validation** — if an applicant checks a main category, the form enforces that at least one subcategory is also selected, both on the frontend and server-side. Selecting a parent without a child is rejected.

**Duplicate submission prevention** — if an applicant tries to submit a second EOI, the system silently redirects them to their existing form instead of creating a duplicate record.

**Email verification flow** — accounts cannot log in until the email is verified. Password reset also goes through a signed email link, not a security question.

**Role separation** — there are two roles in the system. Regular accounts can only see and edit their own application. Super admin accounts have full access to all data, exports, and user management. The separation is enforced at the middleware level, not just the view level.

---

## Tech used

- Laravel 11, PHP 8.2
- MySQL 8.0
- Redis (sessions and cache)
- Bootstrap + Blade templates
- OpenSpout for Excel export
- Docker + Nginx + PHP-FPM for deployment
- XAMPP for local development

---

## Setup

```bash
git clone https://github.com/Jesikachoudhary/empanelmentadmin.git
cd empanelmentadmin
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
php artisan serve
```

Configure your database and mail credentials in `.env` before running migrations.

For Docker setup see [DOCKER_SETUP.md](DOCKER_SETUP.md).

---

## Expertise categories

The portal covers 15 domain areas relevant to port operations and management:

Projects · Environment & Coastal Management · Finance & Taxation · Information Technology · Navigational Audit · Estate & Town Planning · Legal · Disaster Management · Business Development · Human Resources & Welfare · Capacity Building · NDT & SDT · Independent Engineers · Transaction Advisors · Cruise Development

Categories and subcategories are configured in `config/coe_categories.php` — no database table needed, easy to update.

---

## Developer

Jesika Choudhary  
B.Tech Information Technology, Manipal University Jaipur  
jesika.23fe10ite00069@muj.manipal.edu
