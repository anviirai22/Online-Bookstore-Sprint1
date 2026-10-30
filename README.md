# Northstar Books, Sprint 1

An integrated MERN online bookstore application for Team 4 Sprint 1. The four Epic modules run as one React client, one Express API and one MongoDB database.

## Sprint 1 features

- Customer registration, login, logout and session restoration.
- Salted bcrypt password hashes; password hashes are never returned to clients.
- Guest catalogue browsing and title/author/keyword search.
- Customer cart persistence with add, duplicate merge, quantity update and remove actions.
- Server-side stock validation and server-owned cart pricing.
- Admin-only book/category create, update and soft-deactivate operations.
- Server-side JWT authentication and `ADMIN` role enforcement on every admin endpoint.
- Responsive React UI, server integration tests, frontend unit tests, end-to-end smoke script and catalogue p90 measurement script.

## Explicit Sprint 2 exclusions

Checkout, payment, order creation/history, wishlist, reviews, advanced filtering, detailed book pages and extended inventory/order/user/report administration are not implemented in Sprint 1. `OBS-F-006` advanced filtering remains backlog as required by the brief.

## Prerequisites

- Node.js 22 or newer and npm 10 or newer.
- MongoDB 8 locally, or Docker Desktop with its Linux engine running.
- A modern browser.

## Local setup

```bash
npm install
cp .env.example .env
docker compose up -d mongodb
npm run seed
```

If MongoDB is installed outside Docker, start it separately and keep the `MONGODB_URI` from `.env` aligned with that instance. Never commit `.env`.

## Run the application

Start both applications in one terminal:

```bash
npm run dev
```

Or use separate terminals:

```bash
npm run dev:server
npm run dev:client
```

URLs:

- React client: `http://localhost:5173`
- Express API: `http://localhost:5000/api`
- Health check: `http://localhost:5000/api/health`

## Development-only demo accounts

These accounts are created only by the local seed command and use placeholder credentials. Change them for any shared environment.

- Admin: `admin@bookstore.local` / `AdminPass123!`
- Customer: `customer@bookstore.local` / `CustomerPass123!`

The seed script also creates Fiction, Technology and Design categories, active and out-of-stock books, and one inactive book.

## Tests and build

```bash
npm test
npm run test:server
npm run test:client
npm run build
```

Server integration tests use an isolated `mongodb-memory-server` instance. They do not modify development MongoDB data. The client build output is written to `client/dist`.

Run the end-to-end smoke flow with a seeded API running:

```bash
npm run test:e2e
```

Measure catalogue response time against the SRS p90 target:

```bash
PERF_REQUESTS=100 PERF_CONCURRENCY=10 npm run performance
```

The script prints the measured p50, p90 and max latency. It does not claim the 3-second target without a real run.

## API outline

- `POST /api/auth/register`, `POST /api/auth/login`, `POST /api/auth/logout`, `GET /api/auth/me`
- `GET /api/books`, `GET /api/books/categories`
- `GET /api/cart`, `POST /api/cart`, `PATCH /api/cart/:itemId`, `PUT /api/cart/:itemId`, `DELETE /api/cart/:itemId`
- `GET/POST /api/admin/books`, `PATCH/PUT/DELETE /api/admin/books/:bookId`
- `GET/POST /api/admin/categories`, `PATCH/PUT /api/admin/categories/:categoryId`

Authentication uses an HTTP-only same-site JWT cookie. The API also accepts a bearer token for scripts. Logout clears the browser cookie; stateless bearer tokens remain valid until their configured expiry.

## Contribution boundaries

See `docs/SPRINT1_OWNERSHIP.md` for the exact Member 1-4 file lists, Jira mappings and shared-file conflict notes. See `docs/SPRINT1_PR_GUIDE.md` for a truthful manual branch/PR process.

## CI and limitations

`.github/workflows/ci.yml` is configured for push and pull request events. It installs dependencies, starts MongoDB, runs server/client tests and builds the client. It has not been run remotely from this local workspace. The required one-minute demo video and real team-member PR history must be supplied by the team; they are not fabricated here.

Local deployment uses HTTP by design. Production HTTPS termination, secret management, token revocation beyond expiry and operational monitoring remain deployment hardening work.
