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


🚀 Getting StartedPrerequisitesNode.js (v18 or higher)A running MySQL or MariaDB server1. Clone and Install DependenciesBashgit clone [https://github.com/niyabraham/Helply.git](https://github.com/niyabraham/Helply.git)
cd Helply
npm install
2. Configure Environment VariablesCreate a .env file in the project root with the following configuration:Code snippetDB_HOST=localhost
DB_USER=your_mysql_user
DB_PASSWORD=your_mysql_password
DB_NAME=helply_db
PORT=3000
3. Create the DatabaseKnex migrations manage table creation, but you must create the database instance first:SQLCREATE DATABASE helply_db;
4. Run Migrations and SeedsBashnpx knex migrate:latest
npx knex seed:run
(Note: config/schema.sql is provided as a human-readable reference, but the Knex migrations in db/migrations/ serve as the absolute source of truth.)5. Start the ServerBashnpm start
Open your browser and navigate to http://localhost:3000.🗺️ Routes & API EndpointsPages (Frontend Views)RouteDescriptionAuth RequiredGET /, GET /indexHome pageNoGET /signupCreate-account formNoGET /signinSign-in formNoGET /hirePost-a-job formNoGET /job-detailsBrowse open jobsNoGET /job-request/:jobIdApply to a specific jobYesGET /profileView/edit profileYesGET /employer_dashEmployer dashboardYesGET /worker_dashboardWorker dashboardYesGET /logoutEnd sessionNoAPI EndpointsMethodRouteDescriptionAuth RequiredPOST/signupCreate an accountNoPOST/signinSign in and start a sessionNoGET/api/jobsList all open jobsNoPOST/api/jobsCreate a job postingNo ⚠️DELETE/job/:idDelete a job postingNo ⚠️POST/api/applicationsApply to a jobYesGET/api/worker/applicationsGet current worker's applicationsYesGET/api/employer/applicationsGet applications on employer's jobsYesPUT/api/applications/:id/statusAccept or reject an applicationYesPOST/api/profileUpdate name, phone, location, skillsYes
