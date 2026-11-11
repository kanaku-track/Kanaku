# KANAKU — Offline-First Personal & Collaborative Financial OS

**Founder & Project Owner:** Shaik Ashraf  
**Development Model:** Independently developed with AI-assisted engineering  
**Application Status:** Beta  
**Version:** `11.10.26-beta`  
**Frontend Deployment:** Vercel (`https://kanaku-fawn.vercel.app`)  
**Backend Deployment:** Render (`https://kanaku-api.onrender.com`)  
**Database:** Supabase PostgreSQL (Managed Cloud, RLS Policies, 64 Models, 29 Migrations)

---

## 1. Master Excel Status Workbook (Single Source of Truth)

To ensure any developer, auditor, or investor can immediately inspect the project status without reading extensive Markdown files:

👉 **[`KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx`](KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx)** *(also mirrored at [`docs/KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx`](docs/KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx))*

This multi-sheet workbook contains:

- **Executive Dashboard:** Overall health, URLs, version, completion %, test pass rates.
- **Features (Done Well):** All 35 feature domains with business capabilities, implementation quality highlights, and endpoints.
- **Verification (Tests Done):** Exact test suites, passed/failed counts, execution commands, and verified scenarios (190 Vitest frontend suites, 93 Jest backend suites, group lifecycle, OCR accuracy, security gates).
- **Pending Tasks & Requirements:** Clear task matrix with categories (Category C Hold-Gated, Category D UI Pending, Category E Planned), priority (P0–P3), status, and next engineering steps.
- **API & Route Security:** 492 security-reviewed endpoints with auth, roles, and rate limits.
- **Database Models:** 64 PostgreSQL models + 25 Dexie IndexedDB offline tables.
- **Third-Party Integrations & Device Permissions:** Brevo HTTPS, Supabase, Web Push, Gemini/OpenAI, and hardware permissions.
- **Re-generate anytime:** Run `npm run docs:excel` to dynamically update the workbook.

---

## 2. Enterprise Repository Architecture

```text
KANAKU/
├── frontend/             # React 18 + Vite + Tailwind CSS v4 + Dexie.js (v19) SPA/PWA
│   ├── src/
│   │   ├── app/          # Core routing, view pages, modal forms
│   │   ├── features/     # 35 domain feature barrel modules (transactions, groups, settlements, etc.)
│   │   ├── shared/       # Reusable UI primitives, design tokens, hooks, utils
│   │   ├── layouts/      # Application, dashboard, auth, and mobile shells
│   │   ├── navigation/   # Bottom nav, top bars, breadcrumbs, sidebar navigation
│   │   ├── state/        # Centralized stores (Zustand, Dexie live queries)
│   │   ├── permissions/  # Client-side RBAC, capability checks, biometric gates
│   │   ├── validation/   # Zod client validation schemas
│   │   └── error-handling/# Global error boundaries, toast handlers, retry logic
├── backend/              # Express 5 + Node.js + TypeScript + Prisma 7 ORM API (/api/v1)
│   ├── src/
│   │   ├── modules/      # 35 feature domain controllers, services, and routes
│   │   ├── api/          # Central API router mounting all v1 routes
│   │   ├── authentication/# Session tokens, JWT verification, biometric step-up, Google OAuth
│   │   ├── authorization/# Role-based access control (User, Advisor, Admin, Auditor)
│   │   ├── database/     # Prisma client singleton, connection pooling, health checks
│   │   ├── integrations/ # External APIs (Brevo email, FCM Web Push, Razorpay, Sentry)
│   │   ├── notifications/# Multi-channel dispatcher (Push, Email, In-app)
│   │   ├── jobs/         # Background queues, scheduled ledger syncs, cleanup workers
│   │   ├── error-handling/# Centralized AppError classes, async wrappers, audit logging
│   │   └── logging/      # Structured Winston loggers, Loki streams, security audits
├── database/             # Consolidated PostgreSQL reference, schemas, and migrations
│   ├── schema/           # Authoritative schema reference and Prisma introspection
│   ├── migrations/       # Production migration index and verification notes
│   ├── seeds/            # Test and initial environment seeding scripts
│   ├── indexes/          # Performance indexing strategies
│   ├── constraints/      # Financial integrity and ledger constraints
│   ├── views/            # Analytical views and aggregation helpers
│   ├── functions/        # Supabase and PL/pgSQL database functions
│   ├── policies/         # Row-Level Security (RLS) policies
│   ├── queries/          # Optimized query reference library
│   └── backups/          # Pre-deploy and verification database snapshots
├── integrations/         # Third-party integration configurations and contracts
│   ├── email/            # Brevo HTTPS API primary email integration
│   ├── notifications/    # Firebase Cloud Messaging (FCM) & Web Push
│   ├── cloud-storage/    # Supabase S3-compatible document storage
│   ├── authentication/   # Google OAuth, Passkeys (WebAuthn), OTP
│   ├── analytics/        # Privacy-first application telemetry
│   ├── payment/          # Razorpay payment gateway & UPI settlements
│   └── monitoring/       # Sentry error tracking & Grafana observability
├── testing/              # Enterprise test strategy and verification index
│   ├── unit/             # Frontend Vitest & backend Jest unit test guides
│   ├── integration/      # Two-account lifecycle & financial settlement integration
│   ├── api/              # OpenAPI contract audits and Playwright API runners
│   ├── end-to-end/       # Playwright E2E browser automation specs
│   ├── security/         # Route guard, permission matrix, and secret scans
│   ├── performance/      # Load testing and throughput benchmarks
│   └── reports/          # Audit summaries, test coverage, and execution logs
├── infrastructure/       # Deployment blueprints, CI/CD, and hosting configuration
│   ├── vercel/           # Frontend SPA hosting and edge proxy definitions
│   ├── render/           # Backend Express API container deployment (render.yaml)
│   ├── ci-cd/            # GitHub Actions automated workflows (.github/workflows)
│   └── monitoring/       # Grafana Agent, Loki, and Vector log shipper
├── configuration/        # Centralized environment and application configuration
│   ├── environments/     # .env templates and secret variable guides
│   ├── feature-flags/    # Runtime and build-time feature toggles
│   └── application/      # Brand rules, financial constants, currency settings
├── scripts/              # Engineering and operational tooling
│   ├── security/         # Route guard and secret scanning automation
│   ├── qa/               # UI inventory and regression testing scripts
│   ├── contracts/        # API contract generators and validation auditors
│   └── mobile/           # Capacitor build, sync, and signing helpers
├── docs/                 # Complete 18-volume enterprise documentation library
│   ├── 00-project-overview/  # Executive summary, engineering rules, procedure guides, CONTRIBUTING.md
│   ├── 01-architecture/      # Trust boundaries, sequence flows, data models
│   ├── 02-frontend/          # React 18, Dexie sync, PWA lifecycle
│   ├── 03-backend/           # Express 5, middleware, socket events
│   ├── 04-database/          # Schema reference, indexing, RLS
│   ├── 05-features/          # Complete inventory of all 35 features
│   ├── 06-roles-and-permissions/# Multi-role RBAC security matrix
│   ├── 07-security/          # Audit reports, crypto standards, secret policies, SECURITY.md
│   ├── 08-api-reference/     # 492 security-reviewed endpoints & OpenAPI spec
│   ├── 09-integrations/      # Third-party APIs, webhooks, and fallbacks
│   ├── 10-deployment/        # Vercel, Render, and Supabase runbooks
│   ├── 11-testing-and-quality/# Test suites, coverage metrics, and gates
│   ├── 12-monitoring-and-errors/# Loki log streams, Sentry alerts, metrics
│   ├── 13-release-management/# Versioning, CHANGELOG.md, release procedures
│   ├── 14-pending-features/  # Roadmap, technical debt, and TASK.md
│   ├── 15-known-issues/      # Bug trackers, workarounds, edge-case catalog
│   ├── 16-maintenance/       # Maintenance procedures, database tuning, MEMORY.md
│   └── 17-audit-reports/     # Baseline audit reports & certification
├── KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx # Master status & task workbook (1-click access)
├── LICENSE               # MIT License (© 2026 Shaik Ashraf)
├── README.md             # Repository documentation entrypoint
└── AGENTS.md             # Turborepo agent guidelines
```

---

## 3. Engineering Living Controls & Documentation Library

| Control Document | Location | Purpose |
| --- | --- | --- |
| **Status Workbook (Excel)** | [`KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx`](KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx) | Master spreadsheet: what is done well, tests done, pending tasks |
| **Engineering Rules** | [`docs/00-project-overview/RULES.md`](docs/00-project-overview/RULES.md) | Non-negotiable release controls, brand rules, security gates |
| **Contributing Guide** | [`docs/00-project-overview/CONTRIBUTING.md`](docs/00-project-overview/CONTRIBUTING.md) | Engineering contribution standards and PR protocols |
| **Agent Procedures** | [`docs/00-project-overview/AGENT.md`](docs/00-project-overview/AGENT.md) | Mutation procedures, scratch database rules, hand-off protocols |
| **Security Policy** | [`docs/07-security/SECURITY.md`](docs/07-security/SECURITY.md) | Vulnerability disclosure and security controls |
| **Pending Tasks** | [`docs/14-pending-features/TASK.md`](docs/14-pending-features/TASK.md) | Tracked technical debt, acceptance tests, launch milestones |
| **Changelog** | [`docs/13-release-management/CHANGELOG.md`](docs/13-release-management/CHANGELOG.md) | Detailed version release history |
| **Decisions & Memory** | [`docs/16-maintenance/MEMORY.md`](docs/16-maintenance/MEMORY.md) | Architecture Decision Records (ADRs) and constraints |
| **Baseline Audit** | [`docs/17-audit-reports/PROJECT_DISCOVERY_AND_BASELINE_AUDIT.md`](docs/17-audit-reports/PROJECT_DISCOVERY_AND_BASELINE_AUDIT.md) | Baseline audit report |

---

## 4. Getting Started

### Prerequisites

- Node.js 24 (`.nvmrc`)
- npm 11+
- Git

```bash
# Clone the repository
git clone https://github.com/Alpha-ashu/Kanaku.git
cd Kanaku

# Configure environment variables
cp backend/.env.example backend/.env
cp frontend/.env.example frontend/.env

# Install dependencies and generate Prisma client
npm run setup

# Start development servers (Frontend :5173, Backend :5001)
npm run dev
```

> **CRITICAL SECURITY NOTE:** `backend/.env` in production connects to Supabase. Never run tests, migrations, or destructive commands against production credentials. Local development and test runs utilize local PostgreSQL or isolated test databases.

---

## 5. Verification & Quality Gates

Every code change must satisfy the full automated gate before submission:

```bash
# 1. Security Gate (0 secrets leaked, route-guard validation)
npm run security:gate

# 2. Type Check (0 TypeScript errors across all workspaces)
npm run type-check

# 3. Lint Gate (ESLint check within baseline warning ceilings)
npm run lint

# 4. Frontend Vitest Suite (190 test suites, 1,451+ tests)
npm --prefix frontend run test:unit

# 5. Backend Jest Unit Suite (93 test suites, 844+ tests)
npm --prefix backend test

# 6. Update Status Workbook (Generates KANAKU_ENTERPRISE_PROJECT_STATUS.xlsx)
npm run docs:excel
```

---

## 6. Infrastructure & Deployment

- **Frontend (Vercel):** Continuous deployment via Vercel GitHub integration. Production domain: `https://kanaku-fawn.vercel.app`.
- **Backend (Render):** Render Blueprint (`render.yaml`) builds `backend/Dockerfile`. Production domain: `https://kanaku-api.onrender.com`.
- **Database (Supabase):** PostgreSQL with Row Level Security, additive migrations in `backend/prisma/migrations/`.
- **Mobile (Capacitor):** Native Android and iOS projects under `android/` and `ios/`. Release builds generated via GitHub Actions.

---

## 7. License & Copyright

Copyright © 2026 Shaik Ashraf. All rights reserved.  
Licensed under the [MIT License](LICENSE).
