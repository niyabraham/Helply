# Helply 🤝

> Connecting people who need a hand with everyday tasks to workers ready to lend one.

**Helply** is a full-stack web platform that bridges job seekers and job providers for everyday local services — including babysitting, tutoring, cleaning, electrical work, cooking, and gardening. Employers post what they need and review applicants, while workers browse open listings and apply directly. Contact details are shared automatically once an application is accepted.

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
🚀 Getting Started
Prerequisites
Node.js (v18 or higher)

A running MySQL or MariaDB server

1. Clone and Install Dependencies
Bash
git clone [https://github.com/niyabraham/Helply.git](https://github.com/niyabraham/Helply.git)
cd Helply
npm install
2. Configure Environment Variables
Create a .env file in the project root with the following configuration:

Code snippet
DB_HOST=localhost
DB_USER=your_mysql_user
DB_PASSWORD=your_mysql_password
DB_NAME=helply_db
PORT=3000
3. Create the Database
Knex migrations manage table creation, but you must create the database instance first:

SQL
CREATE DATABASE helply_db;
4. Run Migrations and Seeds
Bash
npx knex migrate:latest
npx knex seed:run
(Note: config/schema.sql is provided as a human-readable reference, but the Knex migrations in db/migrations/ serve as the absolute source of truth.)

5. Start the Server
Bash
npm start
Open your browser and navigate to http://localhost:3000.

🗺️ Routes & API Endpoints
Pages (Frontend Views)
GET /, GET /index — Home page (No auth required)

GET /signup — Create-account form (No auth required)

GET /signin — Sign-in form (No auth required)

GET /hire — Post-a-job form (No auth required)

GET /job-details — Browse open jobs (No auth required)

GET /job-request/:jobId — Apply to a specific job (Auth Required)

GET /profile — View/edit profile (Auth Required)

GET /employer_dash — Employer dashboard (Auth Required)

GET /worker_dashboard — Worker dashboard (Auth Required)

GET /logout — End session (No auth required)

API Endpoints
POST /signup — Create an account (No auth required)

POST /signin — Sign in and start a session (No auth required)

GET /api/jobs — List all open jobs (No auth required)

POST /api/jobs — Create a job posting (No auth required ⚠️)

DELETE /job/:id — Delete a job posting (No auth required ⚠️)

POST /api/applications — Apply to a job (Auth Required)

GET /api/worker/applications — Get current worker's applications (Auth Required)

GET /api/employer/applications — Get applications on employer's jobs (Auth Required)

PUT /api/applications/:id/status — Accept or reject an application (Auth Required)

POST /api/profile — Update name, phone, location, skills (Auth Required)

🗄️ Database Schema
user: Stores user accounts (Name, email, username, phone, location, hashed password, skills).

service_category: Stores the six preset job categories.

job: Job postings linked to the posting user and category.

application: Worker applications linked to a specific job and user.

review: Reserved for future ratings/reviews functionality.

payment: Reserved for future payme
