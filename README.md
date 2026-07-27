# Helply 🤝

> Connecting people who need a hand with everyday tasks to workers ready to lend one.

**Helply** is a full-stack web platform that bridges job seekers and job providers for everyday local services — including babysitting, tutoring, cleaning, electrical work, cooking, and gardening. Employers post what they need and review applicants, while workers browse open listings and apply directly. Contact details are shared automatically once an application is accepted.

---

## 📑 Contents

- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Routes & API Endpoints](#-routes--api-endpoints)
- [Database Schema](#-database-schema)
- [Notes & Known Issues](#-notes--known-issues)
- [Roadmap](#-roadmap)
- [License](#-license)

---

## 🌟 Features

### For Employers
* **Job Posting:** Post jobs specifying category, location, pay, schedule, description, and requirements.
* **Dashboard:** Track jobs posted and applications received via a stats overview.
* **Job Management:** View, manage, and delete existing job postings.
* **Applicant Review:** Review applicant profiles and cover letters, then accept or reject applications.

### For Workers
* **Job Discovery:** Browse and search all open job listings.
* **Detailed View:** View full job details before applying.
* **Direct Applications:** Apply with a custom cover letter and an optional resume upload.
* **Application Tracker:** Track every application's status (`Pending` / `Accepted` / `Rejected`) from a personal dashboard.

### Shared Features
* **Authentication:** Signup and signin powered by `bcrypt`-hashed passwords and database-backed sessions.
* **Profiles:** Editable user profiles (Name, phone, location, skills).
* **Categories:** Six built-in service categories (Babysitting, Tuition, Cleaning, Electrical, Cooking, Gardening).
* **Location Support:** Configured for Kerala (Kochi, Thiruvananthapuram, Kozhikode, Thrissur) with easy extensibility.

---

## 💻 Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Runtime** | Node.js (v18+) |
| **Web Framework** | Express 5 |
| **Template Engine** | EJS |
| **Database** | MySQL / MariaDB (via `mysql2`) |
| **Migrations & Seeds** | Knex.js |
| **Sessions** | `express-session` + `express-mysql-session` (DB-backed) |
| **Authentication** | `bcrypt` password hashing |
| **Frontend** | HTML, CSS, Vanilla JavaScript |

---

## 📂 Project Structure

```text
Helply/
├── app.js                      # Express app: routes, auth, and API endpoints
├── knexfile.js                 # Knex database configuration
├── config/
│   ├── db.js                   # MySQL connection pool
│   └── schema.sql              # Reference SQL schema
├── db/migrations/              # Knex migrations (tables & columns)
├── seeds/                      # Knex seed data (service categories)
├── public/
│   ├── css/                    # Stylesheets
│   └── js/                     # Dashboard client-side logic
├── views/                      # EJS templates
└── Documentations/             # Project write-up



