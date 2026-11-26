# poc-express-prisma-railway

> A cloud-native Express REST microservice backed by PostgreSQL and Prisma ORM, configured for zero-downtime deployment pipelines on Railway via Procfile.

**Inception (First Commit):** `2022-08-31` • **Batch / Horizon:** `batch-2021-2022` • **Architecture Domain:** `Cloud Native Backend / Railway Deployment`

---

## 📌 Architecture & Overview

`expressjs-prisma` serves as a cloud-native REST backend proof-of-concept demonstrating declarative database modeling and migration automation for cloud PaaS hosting (specifically Railway and Heroku). Built on Express 4, TypeScript, and Prisma ORM, it implements a structured Todo entity lifecycle backed by a PostgreSQL database.

The project highlights automated CI/CD migration triggers: the build lifecycle script `yarn migrate:deploy && tsc` ensures database migrations apply automatically during container compilation before the application process launches.

```
                              +-----------------------------------------------+
                              |               expressjs-prisma                |
                              |                 (Port: 3000)                  |
                              |                                               |
[ HTTP Client / Browser ]     |  +--------------------+                       |
       |                      |  | Body Parsers       |                       |
       | HTTP (GET/POST/PUT)  |  | (JSON, Raw, Text)  |                       |
       v                      |  +---------+----------+                       |
+---------------------------+ |            |                                  |
| Express App Routing       +-+->+---------v----------+                       |
+---------------------------+ |  | Route Handlers     |                       |
                              |  | (/todos, /todos/:id)                       |
                              |  +---------+----------+                       |
                              |            |                                  |
                              |            v                                  |
                              |  +--------------------+  +------------------+ |
                              |  | Prisma Client      |->| PostgreSQL DB    | |
                              |  | (PrismaClient-JS)  |  | (Railway Cloud)  | |
                              |  +--------------------+  +------------------+ |
                              +-----------------------------------------------+
```

* **Context:** Cloud infrastructure blueprint evaluating automated schema migrations, zero-downtime database deployment pipelines, and Prisma ORM query execution within containerized environments.
* **Key Patterns:**
  * **Automated Migration on Build:** Incorporating `prisma migrate deploy` into the build phase, guaranteeing schema synchronization prior to startup.
  * **UUID Entity Identification:** Primary keys generated via native UUID v4 identifiers (`@default(uuid())`).
  * **PaaS Process Execution:** Deployment process management orchestrated via `Procfile` (`web: yarn start`).

---

## 🛠 Tech Stack

* **Runtime & Framework:** Node.js (>= 16), Express 4.17.1, TypeScript 4.3.5
* **Database & ORM:** PostgreSQL, Prisma ORM 2.30.0 (`@prisma/client`, `prisma`)
* **Deployment & Tooling:** Railway PaaS, `Procfile`, Nodemon 2.0.7, ts-node 9.1.1

---

## ✨ Features & Endpoints

* **Todo Resource Lifecycle:** Comprehensive CRUD operations covering creation, retrieval, updates, and deletion.
* **Automated Chronological Sorting:** Task querying pre-sorted by `createdAt` descending.
* **Self-Documenting Root Endpoint:** HTML landing route exposing available REST routes and HTTP verbs.
* **Declarative PostgreSQL Schema:** Migration history versioned in `prisma/migrations`.

### Key API Routes / Interfaces

| Method | Route | Description |
|---|---|---|
| `GET` | `/` | HTML landing page displaying API route documentation. |
| `GET` | `/todos` | Retrieve all todo items ordered by creation date descending. |
| `POST` | `/todos` | Create a new todo record (defaults text to "Empty todo" if omitted). |
| `GET` | `/todos/:id` | Fetch a single todo item by its UUID. |
| `PUT` | `/todos/:id` | Update an existing todo item's text or completion state. |
| `DELETE` | `/todos/:id` | Remove a todo record by UUID. |

---

## 🚀 Getting Started

### Prerequisites

* Node.js `>= 16.0.0`
* Active PostgreSQL database instance
* Yarn package manager (`npm install -g yarn`)

### Installation

```bash
# Clone the repository
git clone https://github.com/fabogit/poc-express-prisma-railway.git
cd poc-express-prisma-railway

# Install dependencies
yarn install
```

### Environment Configuration

Configure your PostgreSQL database connection URL in `.env`:

```env
PORT=3000
DATABASE_URL="postgresql://user:password@localhost:5432/todo_db?schema=public"
```

### Database Migration & Generation

```bash
# Run migrations in development
yarn migrate:dev

# Generate Prisma Client
yarn generate
```

### Running the Application

```bash
# Development mode with live reload
yarn dev

# Production build and execution
yarn build
yarn start
```

---

## 📄 License & Author

Developed by [@fabogit](https://github.com/fabogit). Based on the Railway starter template by Jake Runzer.
