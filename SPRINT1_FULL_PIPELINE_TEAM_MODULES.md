# BUILD BRIEF — Online Bookstore: Complete Sprint 1 Working Pipeline (Team 4)

> **Instruction for OpenCode:** Implement and verify the **entire integrated, runnable Sprint 1 application**, not the whole two-sprint product. This is a build brief, not evidence of completed work. The four ownership areas below are boundaries for code review and genuine member contributions; they are **not four separate apps**.

## 0. GIT SAFETY — STRICT

You may inspect and modify project files, install dependencies, start services, seed a local development database, and run tests. **Do not** run `git add`, `git commit`, `git push`, `git pull`, `git merge`, `git checkout`, `git switch`, `git reset`, `git rebase`, `git cherry-pick`; do not create/switch branches, edit `.git/`, change remotes or author identity, or generate artificial GitHub history. Read-only `git status`, `git diff`, `git log` are allowed. Leave working-tree changes uncommitted. **Never claim code authored by OpenCode was independently written by a team member.** Each member must actually inspect, test, improve and commit their own work under their own configured GitHub identity, and raise their own PR. Ask before overwriting uncommitted changes that belong to someone else.

## 1. Mission and authoritative sources

Project: **Online Bookstore**, Team 4. Selected stack: **MERN** (MongoDB, Express, React, Node.js), JavaScript. Sprint 1: **19–23 October 2026**. Jira reference: https://anvii06227.atlassian.net/jira/software/projects/OBS/boards/1/backlog?epics=visible . The live Jira board may require authentication; do not pretend to have read it. Use the agreed Sprint 1 work-item list below and flag differences for human confirmation.

Source documents (if present in the repository):
- `SRS_Online_Bookstore_Team4(1).pdf`
- `SAD_Team4_OnlineBookstore(1).pdf`
- `Test_Plan_Online_Bookstore_Team4(1).pdf`

Follow these documents for system behavior, layered architecture, and test strategy. This brief narrows the implementation to the **10 agreed Sprint 1 Jira items**. If the source docs permit Django as an alternative, **use MERN**, which the team selected. The sample `BUILD_STUDENT_RECORD_SYSTEM (1).md` is a **format/process reference only**: do not copy its MySQL, student/faculty modules, academic requirements or full-project scope.

**Deliverable:** A real end-to-end, integrated Sprint 1 application with React UI, Express REST APIs, MongoDB persistence, authentication, customer catalogue, cart, admin catalogue management, role-based access control, local run scripts, seed data, tests, GitHub Actions workflow, and truthful documentation. No placeholder endpoints or fake test results.

## 2. How to work

1. **Inspect first:** Read the existing repository structure, `package.json`, `.env.example`, Docker Compose, frontend/backend, models, routes, tests, CI and docs. Preserve existing working code, dependencies, conventions and unrelated Sprint 2 code already present. Do not rebuild the repository blindly.
2. Produce `docs/SPRINT1_GAP_ANALYSIS.md`: for each of the 10 items mark existing / partial / missing and identify supporting files. Do not claim implementation without evidence.
3. Implement the **complete Sprint 1 pipeline**, in phases below. Do actual edits and run real commands. Do not stop at plans, scaffolding or UI mocks.
4. Narrate major phases concisely, e.g. `[Phase 3/8] Authentication: implementing registration, login, logout and password hashing.` After each phase report files changed, checks run, pass/fail and next phase.
5. Maintain `SPRINT1_IMPLEMENTATION_LOG.md` with phases, decisions, commands, real results, remaining defects and limitations. Do not fabricate screenshots, timings, test passes, PRs or member commits.
6. Ask a focused question before any **materially ambiguous decision** (e.g. whether to reorganize a substantial existing codebase, cart persistence rules if unspecified, authentication migration that would break existing users). For minor choices choose sensible defaults and document them in `docs/DECISIONS.md`.
7. Keep tests close to modules and provide a test traceability matrix. Repair regressions introduced by your work.
8. **Never run Git write commands**. Prepare PR guidance only. Real team members handle commits/PRs/reviews.

## 3. Exactly 10 Sprint 1 Jira work items

| # | Jira item / SRS mapping | Points | Owner | Required result |
|---|---|---:|---|---|
| 1 | **OBS-F-001 — User Registration** | 3 | Member 1 | New customer registers with unique email and validated password; stored in MongoDB. |
| 2 | **OBS-F-002 — User Login and Logout** (also **OBS-F-003**) | 5 | Member 1 | Valid login authenticates; invalid login fails; logout ends usable session. |
| 3 | **OBS-SR-001 — Secure Password Storage** | 3 | Member 1 | Salted one-way password hashes; never plaintext in DB or logs. |
| 4 | **OBS-F-004 — Browse Books** | 5 | Member 2 | Public catalogue lists available/active books with title, author, price, cover, stock/availability. |
| 5 | **OBS-F-005 — Search Books** | 3 | Member 2 | Search by title, author, keyword with correct matching and no-results behavior. |
| 6 | **OBS-NF-001 — Catalogue Response Time** | 3 | Member 2 | Measure 90th percentile catalogue response; SRS target **≤ 3 seconds**; document load and measured results. |
| 7 | **OBS-F-008 — Add Books to Cart** | 5 | Member 3 | Customer adds available book; correct quantity/price; invalid/out-of-stock rejected. |
| 8 | **OBS-F-009 — Update or Remove Cart Items** | 3 | Member 3 | Customer changes quantity/removes item; totals update; stock limits enforced. |
| 9 | **OBS-F-016 — Manage Books and Categories** | 5 | Member 4 | Admin can create/update/manage books and categories; changes visible in catalogue. |
| 10 | **OBS-SR-002 — Role-Based Access Control** | 5 | Member 4 | Admin-only APIs enforced server-side; guest/customer denied even via direct requests. |

**Total: 10 Jira work items / 40 story points.** Work item 2 covers both F-002 and F-003. SRS `OBS-F-006` (filtering) is not in the agreed 10 items; **do not silently mark it done**. Record it as unallocated/backlog unless Jira confirms it belongs to Sprint 1.

### Member ownership and Epic boundaries

- **Member 1 — Authentication & Security (11 points):** registration, login/logout, secure password storage.
- **Member 2 — Book Catalogue (11 points):** browse, search, catalogue response-time measurement.
- **Member 3 — Shopping & Orders (8 points):** add to cart, update/remove cart items; **not checkout/orders yet**.
- **Member 4 — Administration & Security (10 points):** admin book/category management and RBAC.

Use module names based on these Epics. Assign source files and test files to these areas in `docs/SPRINT1_OWNERSHIP.md`. Avoid duplicate shared dependencies and avoid hardcoded user names in application package names. Member ownership is **documentation for actual review**, not fabricated Git attribution.

## 4. Target repository layout — one app, four Epic-owned modules

**Adapt to existing layout rather than moving working files unnecessarily.** If the repo already uses `client/` and `server/`, retain those folders. If starting fresh, prefer:

```text
online-bookstore/
  client/
    src/
      app/                   # shared routing, app shell, providers
      components/            # shared UI
      api/                   # shared API client
      features/
        authentication-security/    # Member 1 frontend
        book-catalogue/            # Member 2 frontend
        shopping-orders/           # Member 3 frontend (cart only)
        administration-security/   # Member 4 frontend
      tests/
    package.json
  server/
    src/
      app.js                 # shared Express app
      server.js
      config/                # MongoDB, environment
      middleware/            # common errors; auth middleware may live in member module
      models/                # common models only if existing architecture requires
      modules/
        authentication-security/    # Member 1 API/service/model/tests
        book-catalogue/            # Member 2 API/service/model/tests
        shopping-orders/           # Member 3 API/service/model/tests
        administration-security/   # Member 4 API/service/tests
    package.json
  e2e/                       # cross-module E2E smoke tests
  load/                      # Member 2 performance script
  docs/
    SPRINT1_GAP_ANALYSIS.md
    SPRINT1_OWNERSHIP.md
    SPRINT1_TEST_MATRIX.md
    DECISIONS.md
    SPRINT1_PR_GUIDE.md
  .github/workflows/ci.yml
  docker-compose.yml          # MongoDB (and app if existing setup supports it)
  .env.example
  .gitignore
  package.json                # root scripts if repository already supports them
  README.md
  SPRINT1_IMPLEMENTATION_LOG.md
```

**Cross-module integration is essential:** a single shared User/Book/Category/Cart data model, one MongoDB database, one API, one React app. Do not create four separate React apps, servers, MongoDBs, package-locks at root, or duplicated login systems. Shared app setup/configuration may be owned by an integration maintainer, but track such files as **shared** rather than falsely attributing them to one member. If file moves are risky, keep existing layout and use clear ownership documentation instead.

## 5. Required working flows and acceptance criteria

### Member 1 — Authentication & Security

Implement `OBS-F-001`, `OBS-F-002`, `OBS-F-003`, `OBS-SR-001`:
- `POST /api/auth/register`: validate unique email and password rules, create `CUSTOMER`, persist a strong salted password hash (`bcrypt` or existing secure equivalent), never return hash.
- `POST /api/auth/login`: verify credentials, establish secure JWT/session consistent with existing app, return safe user info.
- `POST /api/auth/logout` (or equivalent): invalidate/remove active client credentials; if token-based, document revocation/expiry behavior accurately.
- `GET /api/auth/me` if useful to restore logged-in state. Use shared authentication middleware for protected routes.
- React register/login/logout UI with validation, loading and error states. Do not log credentials/tokens. Do not put real secrets in source control.
- Tests: valid registration, duplicate email, weak/invalid password, valid/invalid login, protected route unauthenticated, logout, password stored hashed.

### Member 2 — Book Catalogue

Implement `OBS-F-004`, `OBS-F-005`, `OBS-NF-001`:
- `GET /api/books`: list active/available books from MongoDB, not only hardcoded UI data; support title/author/keyword search and sensible pagination where appropriate.
- Book cards show title, author, price, cover image or fallback, stock/availability; guests can browse and search.
- Search handles empty input, no results, special characters and backend errors safely.
- Add indexes for common search fields where appropriate and a reproducible performance script under `load/` that records **p90 ≤ 3 seconds** under a clearly stated workload; do not claim it meets target unless measured.
- Tests: book list from seeded DB, keyword/author/title match, no match, inactive books excluded, safe query handling, recorded performance evidence.
- `OBS-F-007` detailed book page and `OBS-F-006` advanced filters are **not automatically part of this sprint**.

### Member 3 — Shopping & Orders (Cart only for Sprint 1)

Implement `OBS-F-008`, `OBS-F-009`:
- `GET /api/cart`, `POST /api/cart`, `PUT/PATCH /api/cart/:itemId`, `DELETE /api/cart/:itemId` or equivalent consistent existing routes.
- Persist cart for authenticated customers in MongoDB (unless existing approved design specifies another approach); cart belongs to current user, never to user ID supplied by untrusted client.
- Only available books can be added. Validate positive integer quantity, stock availability, and removal; totals use **server-stored prices**, never client-supplied totals.
- React cart page, add-to-cart buttons, quantity adjustment, remove action, accurate count/total and empty state.
- Tests: add valid item, duplicate add behavior, quantity update, remove, zero/negative/fractional quantity rejected, exceeds stock rejected, cross-user cart isolation.
- Do **not** implement checkout, payment, order creation/history/cancellation, wishlist or reviews.

### Member 4 — Administration & Security

Implement `OBS-F-016`, `OBS-SR-002`:
- Admin book/category create/edit/manage endpoints and React admin screens, integrated with Member 2's shared Book/Category model.
- Require authentication and server-side `ADMIN` role on **every** admin endpoint. Normal customers and guests must receive 401/403 as appropriate; hiding frontend buttons is not sufficient.
- Allow managing book title, author, price, category, stock, cover URL and active status. Validate fields; reflect changes in public catalogue.
- Seed at least one admin and one customer account for local demo using documented development-only credentials and hashed passwords; never seed real secrets into committed code.
- Tests: admin create/update book/category; invalid data rejected; guest/customer blocked from admin API; direct HTTP bypass attempts blocked; public catalogue reflects changes.
- Full inventory/order administration `OBS-F-017` and review moderation `OBS-F-018` are Sprint 2 scope, not Sprint 1.

## 6. Cross-module integrated demo — must actually work

1. Start MongoDB and the Express/React apps using documented commands.
2. Seed a few categories, active/inactive books, a development admin and a customer.
3. Open frontend; **guest** browses and searches catalogue.
4. New customer registers, logs in, sees authenticated state, logs out and back in.
5. Customer adds a book to cart, changes quantity, removes it; totals and MongoDB data remain correct.
6. Customer is denied admin pages and **direct admin API** access.
7. Admin logs in, creates/updates a book/category; refreshed public catalogue shows the change.
8. Run tests and build; report actual outcomes.

Do not fake this flow with static cards, hard-coded API responses, or browser-only role checks.

## 7. Infrastructure, environment, security and local startup

- Prefer the repository's existing scripts, Node version, MongoDB Docker Compose service, and dependency conventions.
- Ensure `.env.example` documents safe placeholders (MongoDB URL, server port, client API base URL, JWT/session secret). `.env` is ignored; no real secrets in Git.
- Seed reproducible **development-only** demo data. Provide commands such as `npm install`, `docker compose up -d mongodb`, `npm run seed`, `npm run dev` **only if they actually exist/work**; otherwise implement matching scripts or document exact working alternatives.
- Use robust API errors, 400/401/403/404/409 as appropriate; avoid exposing stack traces or internal secrets.
- Keep MongoDB access on server only. Enforce role and ownership checks server-side. Validate inputs and quantities.
- A full HTTPS deployment is Sprint 2 security requirement `OBS-SR-004`; do not falsely claim HTTPS is configured if running localhost HTTP. Use safe local auth defaults and document production hardening needs.

## 8. Automated tests and GitHub Actions

Implement **real** unit and integration tests, normal, invalid and boundary cases, and at least one automated or documented reproducible end-to-end Sprint 1 smoke flow. Follow the uploaded Test Plan's categories and use test case IDs, e.g.:

| SRS / work item | Suggested evidence |
|---|---|
| F-001 | `TC-AUTH-01` registration success, duplicate/invalid rejected |
| F-002/F-003 | `TC-AUTH-02`, `TC-AUTH-03` login/logout/protected access |
| SR-001 | `TC-SEC-01` password hash vs plaintext |
| F-004 | `TC-CAT-01` catalogue list and visibility |
| F-005 | `TC-SRCH-01` title/author/keyword and no results |
| NF-001 | `TC-PERF-01` measured p90 under documented conditions |
| F-008 | `TC-CART-01` add to cart, stock check |
| F-009 | `TC-CART-02` update/remove, quantity limits |
| F-016 | `TC-ADM-01` admin book/category management |
| SR-002 | `TC-SEC-02` direct API role restrictions |

Create `docs/SPRINT1_TEST_MATRIX.md` mapping all 10 Jira items to tests and files. Prefer existing test tools; if absent, choose a lightweight compatible setup (e.g. Vitest/Jest, Supertest, React Testing Library, Playwright only if practical). Use a disposable test database, not development data.

Configure `.github/workflows/ci.yml` to run on **push and pull_request**, install dependencies, lint (if available), build and run unit/integration tests. Supply a MongoDB service/container if integration tests require it. Do not automate PR creation or Git commits. A workflow file existing locally is **not proof** it passed on GitHub; report that separately.

## 9. Documentation and required outputs

Update/create:
- `README.md`: truthful Sprint 1 features, prerequisites, local setup, seed, URLs, demo accounts, build/test commands, limitations and explicit Sprint 2 exclusions.
- `docs/SPRINT1_OWNERSHIP.md`: member ↔ Epic ↔ SRS IDs ↔ frontend/backend/test paths, and shared integration files.
- `docs/SPRINT1_GAP_ANALYSIS.md`: before/after state for all 10 work items.
- `docs/SPRINT1_TEST_MATRIX.md`: acceptance criteria, tests and actual results.
- `docs/SPRINT1_PR_GUIDE.md`: safe, manual workflow for each teammate (see next section).
- `SPRINT1_IMPLEMENTATION_LOG.md`: commands actually run, pass/fail, unresolved issues, decisions and deviations from SRS/SAD/Test Plan.

Faculty also requires a **one-minute Sprint 1 working-product demo video uploaded to the repo** and documentation of deviations. Do not fabricate a video or claim it is uploaded; add an honest checklist for humans.

## 10. Real GitHub contribution workflow — do not fake authorship

After OpenCode produces the integrated Sprint 1 working tree, **a single user pushing everything first would make that user's commit the original author of those files**. Merely asking teammates to pull and re-push unchanged files will **not** produce genuine individual contribution history. Do not recommend that.

Instead, produce a clear, feasible manual integration plan:
1. Assign actual review/ownership of each module to the corresponding member, using `docs/SPRINT1_OWNERSHIP.md`.
2. The team chooses a shared base repo/branch and coordinates who stages which files. **Do not publish all generated feature modules in one person's initial commit if the goal is member-specific initial commits.** The integrator can commit only common scaffold/configuration first (manually).
3. Each member obtains the base repo, **reviews, runs, improves and validates** their own assigned module. They create a branch using their own GitHub account and commit their own actual changes. Existing generated code should be attributed transparently as AI-assisted; do not manufacture authorship.
4. Member 1 PR: `feature/sprint1-authentication-security` — auth/security module and tests.
5. Member 2 PR: `feature/sprint1-book-catalogue` — catalogue/search/performance and tests.
6. Member 3 PR: `feature/sprint1-shopping-cart` — cart module and tests.
7. Member 4 PR: `feature/sprint1-admin-rbac` — admin catalogue/RBAC and tests.
8. Coordinate shared User/Book models, auth middleware, React routes and API wiring carefully: shared edits belong to a named integration PR or coordinated owner; do not duplicate files or overwrite each other's work.
9. Repository owner reviews each PR; CI runs; merge after review. GitHub displays actual commit author/PR creator/reviewer based on their authenticated actions.
10. Keep Jira statuses, points and issue references truthful; PR descriptions should list the actual work item IDs and test results.

**Important:** OpenCode must **not** split code by fabricating commits, impersonating identities, modifying Git author settings, or pushing on anyone's behalf. It should output the exact list of files each teammate needs to review and own, with shared-file conflicts highlighted.

## 11. Execution phases — perform, don't only plan

- **Phase 1/8:** Inspect repo, preserve existing code, create gap analysis and decisions.
- **Phase 2/8:** Verify shared MERN infrastructure, MongoDB connection, models, seed and environment.
- **Phase 3/8:** Member 1 authentication + secure password storage; unit/integration tests.
- **Phase 4/8:** Member 2 catalogue + search + performance script; tests.
- **Phase 5/8:** Member 3 cart persistence + stock checks + UI; tests.
- **Phase 6/8:** Member 4 admin catalogue + server-side RBAC; tests.
- **Phase 7/8:** Integrate all modules into one app; GitHub Actions; docs; E2E smoke flow.
- **Phase 8/8:** Start from documented commands, seed, run lint/build/unit/integration/E2E/performance as supported, fix failures, report **actual** results. If an environment blocks a check, state it clearly.

## 12. Definition of done

Sprint 1 is done only when:
- all 10 planned items are implemented or any exception is explicitly identified;
- the application starts locally and MongoDB persists real data;
- registration, login/logout, browsing/search, cart and admin catalogue/RBAC work together;
- guest/customer cannot access admin endpoints;
- unit/integration tests and build pass **when actually executed**;
- performance test exists and results are honestly reported against the p90 target;
- CI workflow is committed-ready (and its actual remote run status is not invented);
- README, ownership map, test matrix, log and PR guide are complete;
- no Sprint 2-only feature was implemented merely to claim completeness;
- no Git write commands were run and no artificial member contribution history was created.

## 13. Final OpenCode report format

Provide:
1. Implemented Sprint 1 items table with SRS IDs, owner, code paths and status.
2. Exact startup commands, local URLs and development-only credentials.
3. Test commands, counts, failures/passes and performance measurements **actually observed**.
4. GitHub Actions workflow path and whether it has been run remotely.
5. Member-wise file ownership and a PR sequence with shared-file conflict notes.
6. Missing work, deviations and manual requirements (especially demo video and real GitHub PRs).

**Begin by inspecting the current repository. Then build and verify the full Sprint 1 pipeline, not Sprint 2 and not a disconnected set of member demos.**
