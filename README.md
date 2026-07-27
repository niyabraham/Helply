# 🤝 Helply

> Connecting people who need a hand with everyday tasks to workers ready to lend one.

Helply is a full-stack web platform that bridges job seekers and job providers for everyday services — babysitting, tutoring, cleaning, electrical work, cooking, and gardening. Employers post what they need and review applicants; workers browse open listings and apply directly, with contact details shared automatically once an application is accepted.

![Node.js](https://img.shields.io/badge/Node.js-43853D?style=flat&logo=node.js&logoColor=white)
![Express 5](https://img.shields.io/badge/Express%205-000000?style=flat&logo=express&logoColor=white)
![EJS](https://img.shields.io/badge/EJS-B4CA65?style=flat&logo=ejs&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
![License: ISC](https://img.shields.io/badge/license-ISC-blue)

---

## Contents
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Routes](#routes)
- [Database Schema](#database-schema)
- [Notes & Known Issues](#notes--known-issues)
- [Roadmap](#roadmap)
- [License](#license)

## Features

**For employers**
- Post jobs with category, location, pay, schedule, description, and requirements
- Dashboard with jobs-posted / applications-received stats
- View, manage, and delete job postings
- Review applicants and accept or reject applications

**For workers**
- Browse and search all open job listings
- View full job details before applying
- Apply with a cover letter and an optional resume upload
- Track every application's status (Pending / Accepted / Rejected) from a personal dashboard

**Shared**
- Signup/signin with bcrypt-hashed passwords and DB-backed sessions
- Editable profile — name, phone, location, skills
- Six built-in service categories: Babysitting, Tuition, Cleaning, Electrical, Cooking, Gardening
- Locations currently set up for Kerala (Kochi, Thiruvananthapuram, Kozhikode, Thrissur) — easy to extend

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Web framework | Express 5 |
| Views | EJS |
| Database | MySQL / MariaDB (via `mysql2`) |
| Migrations & seeds | Knex.js |
| Sessions | `express-session` + `express-mysql-session` (DB-backed) |
| Auth | `bcrypt` password hashing |
| Frontend | HTML, CSS, vanilla JavaScript |

## Project Structure

```
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
```

## Getting Started

### Prerequisites
- Node.js 18+
- A running MySQL or MariaDB server

### 1. Clone and install
```bash
git clone https://github.com/niyabraham/Helply.git
cd Helply
npm install
```

### 2. Configure environment variables
Create a `.env` file in the project root:
```env
DB_HOST=localhost
DB_USER=your_mysql_user
DB_PASSWORD=your_mysql_password
DB_NAME=helply_db
PORT=3000
```

### 3. Create the database
Knex's migrations build the tables, but not the database itself:
```sql
CREATE DATABASE helply_db;
```

### 4. Run migrations and seeds
```bash
npx knex migrate:latest
npx knex seed:run
```

### 5. Start the server
```bash
npm start
```
Then open `http://localhost:3000`.

> `config/schema.sql` is kept as a human-readable reference for the schema — the Knex migrations in `db/migrations/` are the source of truth, so run those rather than executing `schema.sql` directly (it has a small syntax slip; see [Notes & Known Issues](#notes--known-issues)).

## Routes

### Pages
| Route | Description | Auth required |
|---|---|---|
| `GET /`, `GET /index` | Home page | No |
| `GET /signup` | Create-account form | No |
| `GET /signin` | Sign-in form | No |
| `GET /hire` | Post-a-job form | No |
| `GET /job-details` | Browse open jobs | No |
| `GET /job-request/:jobId` | Apply to a specific job | Yes |
| `GET /profile` | View/edit profile | Yes |
| `GET /employer_dash` | Employer dashboard | Yes |
| `GET /worker_dashboard` | Worker dashboard | Yes |
| `GET /logout` | End session | No |

### API
| Method | Route | Description | Auth required |
|---|---|---|---|
| POST | `/signup` | Create an account | No |
| POST | `/signin` | Sign in, start a session | No |
| GET | `/api/jobs` | List all open jobs | No |
| POST | `/api/jobs` | Create a job posting | No ⚠️ |
| DELETE | `/job/:id` | Delete a job posting | No ⚠️ |
| POST | `/api/applications` | Apply to a job | Yes |
| GET | `/api/worker/applications` | Get the current worker's applications | Yes |
| GET | `/api/employer/applications` | Get applications on the employer's jobs | Yes |
| PUT | `/api/applications/:id/status` | Accept or reject an application | Yes* |
| POST | `/api/profile` | Update name, phone, location, skills | Yes |

\* Checks that *someone* is logged in, but not that they own the job the application belongs to. ⚠️ rows have no session check at all. See [Notes & Known Issues](#notes--known-issues).

## Database Schema

| Table | Purpose |
|---|---|
| `user` | Accounts — name, email, username, phone, location, hashed password, skills |
| `service_category` | The six job categories |
| `job` | Job postings, linked to the posting user and a category |
| `application` | Worker applications, linked to a job and a user |
| `review` | Reserved for a future ratings/reviews feature |
| `payment` | Reserved for a future payments feature |

## Notes & Known Issues

**Access control** — worth locking down before this goes anywhere public:
- `DELETE /job/:id` never checks `req.session.userId` — any visitor, logged in or not, can delete any job by ID.
- `POST /api/jobs` doesn't check for a session either; an unauthenticated request just inserts the job with `User_id_FK` as `NULL` instead of being rejected.
- `PUT /api/applications/:id/status` checks that someone is logged in, but not that they're the employer who actually owns the job the application belongs to — any authenticated account can accept or reject any application by ID (and see the associated employer's contact info in the response).

**Other loose ends**
- `config/schema.sql` is missing a comma between the `password` and `Skills` column definitions in the `user` table — it'll throw a syntax error if run directly. The Knex migrations don't have this problem, since `Skills` is added by its own migration.
- `views/job-details.ejs`'s inline script looks up `categoryFilter` and `locationFilter` elements that aren't in the markup (only a search box is rendered), so those `getElementById` calls return `null` and throw before the initial `filterJobs()` call runs.
- `public/js/employer-dashboard.js` calls `GET /api/myjobs` to list the employer's own postings, but that route isn't implemented in `app.js` yet.
- `sessions/*.json` files are committed to the repo. They look like leftovers from an earlier `session-file-store` setup (still listed in `package.json`, though the app now uses `express-mysql-session`, which stores sessions in MySQL instead). Worth deleting, and adding `sessions/`, `.env`, and `node_modules/` to a `.gitignore`.
- The session secret is hardcoded in `app.js` (`secret: 'your_secret_key'`) — move it into `.env` before deploying anywhere public.

## Roadmap
- [ ] Reviews & ratings (the `review` table already exists in the schema)
- [ ] Payments (the `payment` table already exists in the schema)
- [ ] Server-side job search & filtering (currently done client-side in the browser)

## License

ISC — as specified in `package.json`.
