# AI-Powered SME Financial Intelligence & Risk Management Platform

> **Status: Backend Architecture & Implementation Complete (Sections 1–24 Verified)**  
> **Phase 1 Foundation Intact** | **Full Multi-Agent AI Engine Complete** | **MCP Integration Active**  
> Aligned with the *AI-Powered SME Financial Intelligence & Risk Management Platform — Capstone Project Blueprint (Version 1.0)*.

---

## 1. Project Overview

The **AI-Powered SME Financial Intelligence & Risk Management Platform** is an enterprise-grade, browser-based financial intelligence and decision-support platform designed specifically for small and medium-sized enterprises (SMEs).

The platform couples a high-performance **React 19 + TypeScript + Vite + Tailwind CSS** frontend with a robust, production-grade **NestJS 12 + TypeScript + PostgreSQL 16 + Prisma 7** modular monolithic backend. The backend incorporates a deterministic financial calculation engine, multi-horizon cash-flow forecasting (`7`, `30`, `60`, `90` days), **FinMemory** historical intelligence snapshots, automated risk and anomaly surveillance, isolated what-if scenario simulations, and a fully bounded **Multi-Agent AI Architecture** powered by the **Model Context Protocol (MCP)**, **Reusable Agent Skills**, a **Multi-Provider AI Model Gateway** (live-verified with Google Gemini), **Agent Sandboxing**, **Lifecycle Hooks**, and an independent **Verifier AI Agent**.

Technical reference documents:
- **Backend Architecture & Deep Dive**: [`backend/README.md`](./backend/README.md)
- **Frontend REST API Integration Contract**: [`docs/frontend-api-integration.md`](./docs/frontend-api-integration.md)
- **System Architecture Specification**: [`docs/architecture.md`](./docs/architecture.md)
- **Environment Setup & Verification Guide**: [`docs/setup-guide.md`](./docs/setup-guide.md)
- **Engineering Decisions & Notes**: [`docs/development-notes.md`](./docs/development-notes.md)

---

## 2. Project Purpose

Small and medium-sized businesses frequently manage financial data across disconnected spreadsheets, bank exports, manual invoices, and disparate accounting software. Static historical records alone rarely provide timely visibility into upcoming liquidity shortfalls, overdue receivables, anomalous spending, or period-over-period structural shifts.

The platform is engineered to deliver:

- **Financial Visibility**: Consolidated ledger clarity across business bank accounts, income, expenses, customer invoices, supplier bills, and department budgets.
- **Cash-Flow Awareness**: Short-term liquidity visibility and deterministic multi-horizon cash-flow projections (`7`, `30`, `60`, and `90` days) across Base, Optimistic, and Conservative scenarios.
- **Financial Risk Monitoring**: Statistical and rule-based detection of unusual expense patterns, recurring vendor cost inflation, overdue receivables aging, and cash shortfall risks under a strict non-fraud policy.
- **Historical Financial Intelligence**: Time-aware period comparisons powered by **FinMemory**, tracking financial metric snapshots, variance attributions, and prior forecasts against actual outcomes.
- **Evidence-Based Business Decisions**: Deterministic, decimal-safe financial calculations (`@db.Decimal(19, 4)` via `decimal.js`) paired with bounded multi-agent reasoning, what-if sensitivity simulations, verifier audits, and human approval workflows.

---

## 3. Technology Stack

| Layer | Technology | Version | Purpose |
| :--- | :--- | :--- | :--- |
| **Runtime** | Node.js / npm | `v24.20.0` / `12.2.0` | JavaScript/TypeScript execution & package management |
| **Backend Framework** | NestJS (`@nestjs/core`, `@nestjs/common`) | `12.4.1` | Modular monolithic REST API architecture |
| **API Documentation** | OpenAPI / Swagger (`@nestjs/swagger`) | `12.1.1` | Auto-generated interactive REST API docs at `/api/docs` |
| **Database & ORM** | PostgreSQL + Prisma ORM (`@prisma/adapter-pg`) | PG `16` / Prisma `7.10.1` | Relational storage (22 models, 18 enums) & type-safe query client |
| **Decimal Precision** | `decimal.js` | `10.6.0` | Exact fixed-point arithmetic (`19, 4`) preventing binary float drift |
| **Security & Auth** | Helmet + `scrypt` + `jsonwebtoken` | `8.1.0` / `9.0.3` | Cryptographic password hashing, secure JWTs, and security headers |
| **AI Model Gateway** | Google Gemini / OpenAI / Anthropic SDKs | Native REST / SDK | Multi-provider LLM gateway with live Gemini integration verification |
| **AI Protocol** | Model Context Protocol (MCP) | Custom NestJS Core | Standardized tool invocation, schema validation, and discovery |
| **Frontend Framework** | React + TypeScript | `19.3.1` / `6.0.3` | Component-driven UI with strict static typing |
| **Frontend Build Tool** | Vite | `8.3.7` | Fast HMR development server and production bundler |
| **Styling & Design** | Tailwind CSS (`@tailwindcss/vite`) | `4.3.3` | Design tokens (Deep Forest Green, Charcoal, Ivory, Teal/Amber) |
| **Data Visualization** | Recharts | `3.10.1` | Financial chart rendering for analytics and cash-flow projections |
| **Code Quality** | ESLint + Prettier | `10.14.0` / `3.9.10` | TypeScript-aware linting and consistent formatting |
| **Automated Testing** | Jest + Supertest (Backend) / Vitest (Frontend) | Jest `30.5.2` / Vitest `5.1.0` | Unit, integration, E2E, and component testing |

---

## 4. Architecture Overview

The platform uses a **Modular Monolithic Architecture** with strict compile-time boundaries, high cohesion, and loose coupling:

```text
┌────────────────────────────────────────────────────────────────────────────┐
│                        Browser Client (React 19 + Vite)                    │
│  ┌──────────────────┐  ┌───────────────────────┐  ┌─────────────────────┐  │
│  │ Application Shell│  │ Design Token System   │  │ Typed HTTP Client   │  │
│  │ & Landing Page   │  │ (Light & Dark Modes)  │  │ (apiClient.ts)      │  │
│  └────────┬─────────┘  └───────────────────────┘  └──────────┬──────────┘  │
└───────────┼──────────────────────────────────────────────────┼─────────────┘
            │ HTTPS / REST (/api/v1/*)                         │
            ▼                                                  ▼
┌────────────────────────────────────────────────────────────────────────────┐
│                     NestJS 12 Modular Monolith Backend                     │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │ Security Pipeline: Helmet | CORS | JwtAuthGuard | OrgRbacGuard       │  │
│  └──────────────────────────────────┬───────────────────────────────────┘  │
│                                     │                                      │
│  ┌──────────────────────────────────┴───────────────────────────────────┐  │
│  │ Core Domain Modules (Deterministic Finance & Audit)                  │  │
│  │ - Auth & Organizations (Multi-Tenant RBAC with 7 Roles)              │  │
│  │ - Financial Accounts & Ledger (Decimal-Safe Transactions)            │  │
│  │ - CSV Import Engine (2-Step Staging, Mapping, Duplicate Detection)   │  │
│  │ - Invoices, Bills, Partial Payments & AR/AP Aging                   │  │
│  │ - Budgets & Variance Attribution | Financial Analytics (10 Metrics)  │  │
│  │ - Cash-Flow Forecasting (7/30/60/90 Days) | FinMemory Snapshots      │  │
│  │ - Anomaly & Risk Monitoring (8 Detectors, Non-Fraud Review Policy)   │  │
│  │ - Scenario Planning (Isolated What-If Simulations)                   │  │
│  │ - Reports, Scheduled Tasks, Notifications & Immutable SHA-256 Audit  │  │
│  └──────────────────────────────────┬───────────────────────────────────┘  │
│                                     │                                      │
│  ┌──────────────────────────────────▼───────────────────────────────────┐  │
│  │ Multi-Agent AI Orchestration Subsystem                               │  │
│  │ - Supervisor Orchestrator (10-Step Workflow Execution)               │  │
│  │ - 8 Specialist AI Agents + Verifier AI Agent                         │  │
│  │ - Agent Sandbox Service (Tenant Scoping, Tool Allowlists, Blocked Ops)│ 
│  │ - MCP Integration & Tool Registry (8 Approved Financial Tools)       │  │
│  │ - Agent Skills Registry (9 Versioned Executable Skills)              │  │
│  │ - AI Model Provider Gateway (Gemini Live Verified | OpenAI | Anthropic)│
│  │ - Pre-, During-, Post-, and Error-Handling Lifecycle Hooks           │  │
│  └──────────────────────────────────┬───────────────────────────────────┘  │
│                                     │                                      │
│                            ┌────────▼───────────────────────────────────┐  │
│                            │ Prisma 7 ORM (@prisma/adapter-pg)          │  │
│                            │ 22 Models | 18 Enums | @db.Decimal(19, 4)  │  │
│                            └────────────────────┬───────────────────────┘  │
└─────────────────────────────────────────────────┼──────────────────────────┘
                                                  │ TCP (5432)
                                                  ▼
                                    ┌───────────────────────────┐
                                    │   PostgreSQL 16 Database  │
                                    └───────────────────────────┘
```

---

## 5. Folder Structure

```text
ai-powered-sme-financial-intelligence/
├── frontend/
│   ├── public/
│   │   └── favicon.svg
│   ├── src/
│   │   ├── components/
│   │   │   ├── layout/                 # AppShell, Header, Footer
│   │   │   └── ui/                     # Badge, Card, StatusIndicator
│   │   ├── config/                     # Centralized environment config (env.ts)
│   │   ├── hooks/                      # useSystemHealth, useTheme
│   │   ├── pages/                      # LandingPage
│   │   ├── services/                   # apiClient, healthService
│   │   ├── test/                       # Vitest test setup
│   │   ├── types/                      # TypeScript definitions (api, health)
│   │   └── utils/                      # Date/currency formatters
│   ├── package.json
│   └── vite.config.ts
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma               # 22 PostgreSQL models & 18 enums (@db.Decimal(19, 4))
│   │   └── migrations/                 # Committed SQL migrations applied to PostgreSQL 16
│   ├── src/
│   │   ├── common/
│   │   │   ├── decorators/             # @Roles(), @Public(), @CurrentUser()
│   │   │   ├── dto/                    # PaginationQueryDto, ApiErrorResponseDto
│   │   │   ├── filters/                # AllExceptionsFilter (safe JSON error envelopes)
│   │   │   ├── guards/                 # JwtAuthGuard, OrgRbacGuard, RateLimitGuard
│   │   │   ├── interceptors/           # LoggingInterceptor (zero-secret HTTP logging)
│   │   │   └── utils/                  # DecimalMath (Decimal.js), SecurityUtil (scrypt, JWT, AES-256)
│   │   ├── config/                     # Startup environment validation (env.validation.ts)
│   │   ├── database/                   # PrismaService with live SELECT 1 readiness
│   │   ├── health/                     # Liveness (/api/health) and Readiness (/api/health/ready)
│   │   └── modules/
│   │       ├── audit/                  # Immutable SHA-256 integrity-hashed AuditEvent service & API
│   │       ├── auth/                   # Registration, login, session revocation, password recovery
│   │       ├── organizations/          # Multi-tenant workspaces & 7-role memberships
│   │       ├── financial-transactions/ # Chart of accounts & decimal-safe income/expense ledger
│   │       ├── csv-import/             # 2-step CSV upload, column mapping, preview, duplicate detection
│   │       ├── invoices-payables/      # Customer invoices, supplier bills, partial payments & aging
│   │       ├── budgets/                # Monthly/quarterly budgets, variance attribution & revisions
│   │       ├── financial-analytics/    # 10 deterministic financial metrics with formulas & guards
│   │       ├── cash-flow-forecasting/  # 7/30/60/90-day Base/Optimistic/Conservative projections
│   │       ├── finmemory/              # Monthly snapshots, completeness scoring, period comparisons
│   │       ├── risk-monitoring/        # 8 anomaly/risk detectors, non-fraud policy & review triage
│   │       ├── scenario-planning/      # Isolated what-if sensitivity simulations
│   │       ├── ai-orchestration/       # Supervisor, 8 Specialist Agents, MCP, Skills, Gateway & Sandbox
│   │       │   ├── dto/                # AI execution and task DTOs
│   │       │   ├── hooks/              # Pre, During, Post, and Error lifecycle hooks
│   │       │   ├── mcp/                # McpIntegrationService & ToolRegistryService (8 approved tools)
│   │       │   ├── model-provider/     # AiModelProviderService (Gemini, OpenAI, Anthropic, Fallback)
│   │       │   ├── sandbox/            # AgentSandboxService (tenant scoping, tool allowlists, logs)
│   │       │   ├── skills/             # AgentSkillsRegistryService (9 versioned executable skills)
│   │       │   └── supervisor-orchestrator.service.ts # 10-step orchestration coordinator
│   │       └── reports/                # Financial reports, CSV/PDF exports, Tasks & Notifications
│   ├── test/
│   │   ├── health.e2e-spec.ts          # Health & readiness E2E suite
│   │   └── platform-complete.e2e-spec.ts # Full-stack PostgreSQL E2E suite (Sections 17 & 24)
│   ├── package.json
│   └── prisma.config.ts
├── docs/
│   ├── architecture.md                 # Full system architecture specification
│   ├── development-notes.md            # Technical decisions and environment notes
│   ├── frontend-api-integration.md     # Complete frontend-to-backend REST API contract
│   └── setup-guide.md                  # Comprehensive setup instructions
├── docker-compose.yml                  # PostgreSQL 16 development database container
└── README.md
```

---

## 6. Frontend Setup

```bash
cd frontend
cp .env.example .env
npm install
npm run dev
```

The Vite development server starts on **`http://localhost:5173`** and proxies `/api` requests to the configured backend server (`http://localhost:3000`).

---

## 7. Backend Setup

```bash
cd backend
cp .env.example .env
npm install
npm run prisma:generate
npm run prisma:migrate
npm run build
npm run start:dev
```

The NestJS REST API starts on **`http://localhost:3000`**.
- Health liveness endpoint: `http://localhost:3000/api/health`
- Database readiness endpoint: `http://localhost:3000/api/health/ready`
- Interactive Swagger / OpenAPI documentation: `http://localhost:3000/api/docs`

---

## 8. PostgreSQL Setup

### Option A: Docker Compose (Recommended)
From the repository root:
```bash
docker compose up -d
```
This starts a PostgreSQL 16 container on port `5432` with database `sme_financial_intelligence_dev`.

### Option B: Local PostgreSQL Instance
Create the development database manually:
```sql
CREATE DATABASE sme_financial_intelligence_dev;
```
Then update `DATABASE_URL` in `backend/.env` with your PostgreSQL credentials.

---

## 9. Prisma Configuration

Prisma 7 is configured using:
- `backend/prisma.config.ts`: Reads `DATABASE_URL` from environment variables via `dotenv/config` and `defineConfig` (`prisma/config`).
- `backend/prisma/schema.prisma`: Configures the `prisma-client` generator (`output = "../src/generated/prisma"`, `moduleFormat = "esm"`), `postgresql` datasource, and 22 relational models with `@db.Decimal(19, 4)`.
- `backend/src/database/prisma.service.ts`: Connects to PostgreSQL using `@prisma/adapter-pg` (`PrismaPg`) with live `SELECT 1` readiness verification.

Useful Prisma commands (run inside `backend/`):
```bash
npm run prisma:generate   # Generate Prisma Client into src/generated/prisma
npm run prisma:migrate    # Run interactive schema migrations
npm run prisma:deploy     # Apply committed migrations in CI/production
npm run prisma:studio     # Open Prisma Studio GUI
```

---

## 10. Environment Variables

Never commit real `.env` files or API secrets.

### Frontend (`frontend/.env.example`)
| Variable | Example Value | Description |
| :--- | :--- | :--- |
| `VITE_API_BASE_URL` | `http://localhost:3000/api` | Base URL for backend REST API requests |

### Backend (`backend/.env.example`)
| Variable | Example Value | Description |
| :--- | :--- | :--- |
| `PORT` | `3000` | HTTP port for the NestJS API server |
| `NODE_ENV` | `development` | Runtime mode (`development`, `test`, `production`) |
| `FRONTEND_URL` | `http://localhost:5173` | Allowed frontend origin for CORS |
| `DATABASE_URL` | `postgresql://postgres:postgrespassword@localhost:5432/sme_financial_intelligence_dev?schema=public` | PostgreSQL connection URL |
| `JWT_SECRET` | `your-secure-jwt-secret-at-least-32-chars-long` | Secret for signing access tokens |
| `JWT_EXPIRATION` | `86400` | JWT expiration time in seconds (24h) |
| `AI_PROVIDER` | `gemini` | Active AI provider (`gemini`, `openai`, `anthropic`, `deterministic-grounded`) |
| `GEMINI_API_KEY` | `AIzaSy...` | Google Gemini API key (live verified) |
| `OPENAI_API_KEY` | `sk-...` | OpenAI API key (optional) |
| `ANTHROPIC_API_KEY` | `sk-ant-...` | Anthropic Claude API key (optional) |
| `AI_TIMEOUT_MS` | `10000` | Per-request LLM timeout (milliseconds) |
| `AI_MAX_RETRIES` | `2` | Maximum retries with exponential backoff |
| `AI_RATE_LIMIT_PER_MIN` | `60` | AI invocation rate limit per organization |

---

## 11. Development Commands

From the repository root:
```bash
npm run dev:backend       # Start NestJS backend in watch mode (port 3000)
npm run dev:frontend      # Start Vite frontend dev server (port 5173)
npm run lint              # Run ESLint across both backend and frontend
npm run format            # Format backend and frontend code with Prettier
npm run format:check      # Check code formatting without modifying files
```

---

## 12. Testing Commands

From the repository root:
```bash
npm test                  # Run all backend unit tests and frontend Vitest tests
npm run test:backend      # Run backend Jest unit tests (including AI, MCP, Sandbox tests)
npm run test:backend:e2e  # Run backend Supertest E2E integration tests (Sections 17 & 24)
npm run test:frontend     # Run frontend Vitest component & API communication tests
```

---

## 13. Build Commands

From the repository root:
```bash
npm run build             # Build both backend (nest build) and frontend (tsc -b && vite build)
npm run build:backend     # Compile NestJS backend to backend/dist/
npm run build:frontend    # Typecheck and bundle React frontend to frontend/dist/
```

---

## 14. Health-Check Endpoints

### 1. Application Liveness (`GET /api/health`)
Operates independently of database availability.

**Response (`200 OK`)**:
```json
{
  "status": "ok",
  "service": "SME Financial Intelligence API",
  "timestamp": "2026-11-10T11:30:00.000Z"
}
```

### 2. Database Readiness (`GET /api/health/ready`)
Executes a live `SELECT 1` query against PostgreSQL via Prisma ORM.

**Response when PostgreSQL is connected (`200 OK`)**:
```json
{
  "status": "ready",
  "service": "SME Financial Intelligence API",
  "database": {
    "status": "connected",
    "latencyMs": 3
  },
  "timestamp": "2026-11-10T11:30:00.000Z"
}
```

**Response when PostgreSQL is unreachable (`503 Service Unavailable`)**:
```json
{
  "statusCode": 503,
  "error": "SERVICE_UNAVAILABLE",
  "message": "Database connection is not ready",
  "database": {
    "status": "disconnected"
  },
  "path": "/api/health/ready",
  "timestamp": "2026-11-10T11:30:00.000Z"
}
```

---

## 15. Troubleshooting

- **`Environment configuration validation failed: DATABASE_URL is required`**:
  Ensure `backend/.env` exists (`cp backend/.env.example backend/.env`) and contains a valid `postgresql://` connection string.
- **`GET /api/health/ready` returns `503 Service Unavailable`**:
  Verify that PostgreSQL is running on port `5432` (`docker compose up -d`) and that the `sme_financial_intelligence_dev` database exists.
- **CORS errors in browser**:
  Ensure `FRONTEND_URL` in `backend/.env` matches the exact origin of the Vite dev server (`http://localhost:5173`).
- **Missing Prisma Client types (`Cannot find module '../generated/prisma/client.js'`)**:
  Run `npm run prisma:generate` inside `backend/`.

---

## 16. Phase 1 Scope & Historical Foundations

By design, **Phase 1** established the project's architectural foundation:
- Application shell, landing page, and design token system.
- Modular monolith scaffolding, health checks, and database readiness diagnostics.
- Prevention of premature monolithic drift or binary floating-point calculations.

Every core backend capability deferred during Phase 1 has now been fully implemented and verified in **Sections 1 through 24**:
- Complete PostgreSQL schema with 22 relational models and `@db.Decimal(19, 4)` precision.
- Multi-tenant RBAC across 7 roles (`PLATFORM_ADMIN`, `BUSINESS_OWNER`, `FINANCE_MANAGER`, `ACCOUNTANT`, `OPERATIONS_MANAGER`, `FINANCIAL_ANALYST`, `AUDITOR`).
- Deterministic calculation engines for 10 financial metrics, cash-flow forecasting, AR/AP aging, budgets, and anomaly detection.
- Complete Multi-Agent AI system with MCP tools, Skills, AI Model Provider Gateway, Agent Sandboxing, and Verifier AI.

---

## 17. Multi-Agent AI Architecture

The platform implements an advanced multi-agent architecture composed of a **Central Supervisor Orchestrator**, **8 Specialist Agents**, and an independent **Verifier AI Agent**:

```text
                                  ┌────────────────────────┐
                                  │   User Prompt / API    │
                                  └───────────┬────────────┘
                                              │
                                              ▼
                             ┌─────────────────────────────────┐
                             │  Central Supervisor Orchestrator│
                             │  - Input Sanitization & Pre-Hook│
                             │  - Task Classification          │
                             │  - Agent Selection & Dispatch   │
                             └───────────────┬─────────────────┘
                                             │
               ┌─────────────────────────────┼─────────────────────────────┐
               │ (Parallel Execution in Isolated Sandboxes via MCP Tools)  │
               ▼                             ▼                             ▼
   ┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐
   │  Finance Data Analyst │   │ Cash-Flow Forecasting │   │ Expense Anomaly Agent │
   │  skill.financial-data-│   │ skill.cash-flow-      │   │ skill.expense-and-    │
   │  analysis & aging     │   │ forecasting           │   │ anomaly-analysis      │
   └───────────┬───────────┘   └───────────┬───────────┘   └───────────┬───────────┘
               │                           │                           │
               ├───────────────────────────┼───────────────────────────┤
               ▼                           ▼                           ▼
   ┌───────────────────────┐   ┌───────────────────────┐   ┌───────────────────────┐
   │ Financial Risk Agent  │   │ Scenario Planning Agt │   │ Strategic Recommender │
   │ skill.financial-risk- │   │ skill.scenario-       │   │ skill.strategic-      │
   │ assessment            │   │ planning              │   │ recommendations       │
   └───────────┬───────────┘   └───────────┬───────────┘   └───────────┬───────────┘
               │                           │                           │
               └───────────────────────────┼───────────────────────────┘
                                           │
                                           ▼
                             ┌───────────────────────────┐
                             │ Financial Reporting Agent │
                             │ skill.financial-reporting │
                             └─────────────┬─────────────┘
                                           │ Specialist Outputs + Evidence Citations
                                           ▼
                             ┌───────────────────────────┐
                             │      Verifier AI Agent    │
                             │ - Arithmetic Consistency  │
                             │ - Citation Verification   │
                             │ - Non-Fraud Policy Check  │
                             └─────────────┬─────────────┘
                                           │
                                           ▼
                             ┌───────────────────────────┐
                             │ Result Synthesis & Store  │
                             │ - Post-Execution Hooks    │
                             │ - Immutable Audit Event   │
                             │ - WorkflowRun Persistence │
                             └───────────────────────────┘
```

### Specialist Agents and Capabilities

1. **Central Supervisor Orchestrator (`SUPERVISOR_ORCHESTRATOR`)**:
   Coordinates the end-to-end workflow, manages lifecycle hooks, validates RBAC, classifies user intent, delegates work to specialist agents, and invokes the Verifier AI Agent.
2. **Finance Data Analyst Agent (`FINANCE_DATA_ANALYST`)**:
   Evaluates normalized financial accounts, operating cash flow, liquidity ratios, working capital, and budget attribution.
3. **Cash-Flow Forecasting Agent (`CASH_FLOW_FORECASTING`)**:
   Interprets multi-horizon cash trajectories (`7`, `30`, `60`, `90` days), analyzes burn rates, and explains liquidity shortfall risks.
4. **Expense & Anomaly Detection Agent (`EXPENSE_ANOMALY_DETECTION`)**:
   Surveils spending patterns, vendor charge spikes, duplicate invoices, and sudden outflows under a strict non-fraud policy.
5. **Financial Risk Assessment Agent (`FINANCIAL_RISK_ASSESSMENT`)**:
   Calculates composite financial risk scores, debt service vulnerability, and customer payment default exposure.
6. **Scenario Planning Agent (`SCENARIO_PLANNING`)**:
   Simulates what-if macroeconomic and operational shocks (revenue drop, delayed collections, expense inflation) in non-destructive memory.
7. **Strategic Recommendation Agent (`STRATEGIC_RECOMMENDATION`)**:
   Synthesizes findings from upstream specialists into prioritized, actionable business guidance with clear tradeoffs and human-in-the-loop caveats.
8. **Financial Reporting Agent (`FINANCIAL_REPORTING`)**:
   Compiles narrative executive summaries, board briefs, and variance disclosures fully grounded in deterministic calculations.
9. **Verifier AI Agent (`VERIFIER_AI`)**:
   Acts as an independent safety and accuracy barrier, validating mathematical consistency, verifying source record citations, and ensuring compliance with the platform non-fraud policy.

---

## 18. Model Context Protocol (MCP) Integration & Tool Registry

The platform integrates the **Model Context Protocol (MCP)** via `McpIntegrationService` and `ToolRegistryService`, exposing **8 approved, deterministic financial tools**. Agents cannot query databases directly; they interact exclusively through these validated, permission-checked tools:

| # | MCP Tool Name | Responsible Agent(s) | Description |
| :- | :--- | :--- | :--- |
| 1 | `mcp.finance.get_deterministic_metrics` | `FINANCE_DATA_ANALYST` | Computes 10 decimal-safe metrics (operating cash flow, runway, current/quick ratios, gross margin, working capital, budget variance, aging). |
| 2 | `mcp.finance.get_accounts_and_ledger_summary` | `FINANCE_DATA_ANALYST` | Retrieves chart of accounts, bank balances, and filtered transaction ledger summaries. |
| 3 | `mcp.forecast.get_or_generate_cash_flow_forecast` | `CASH_FLOW_FORECASTING` | Generates or retrieves deterministic 7/30/60/90-day cash-flow forecasts across Base, Optimistic, and Conservative scenarios. |
| 4 | `mcp.risk.get_or_scan_anomalies_and_alerts` | `EXPENSE_ANOMALY_DETECTION`, `FINANCIAL_RISK_ASSESSMENT` | Runs statistical/rule-based anomaly detection across 8 risk categories with non-fraud policy enforcement. |
| 5 | `mcp.aging.get_receivables_and_payables_aging` | `FINANCE_DATA_ANALYST`, `FINANCIAL_RISK_ASSESSMENT` | Computes structured receivables (AR) and payables (AP) aging buckets (`0-30`, `31-60`, `61-90`, `90+` days) with overdue totals. |
| 6 | `mcp.budget.get_budget_variance_summary` | `FINANCE_DATA_ANALYST` | Retrieves active budgets, actual vs. budgeted spend, and variance attributions. |
| 7 | `mcp.scenario.simulate_what_if_scenario` | `SCENARIO_PLANNING` | Executes non-destructive sensitivity simulations (revenue shocks, expense spikes, payment delays). |
| 8 | `mcp.finmemory.get_historical_snapshots` | `FINANCE_DATA_ANALYST`, `FINANCIAL_REPORTING` | Retrieves monthly FinMemory historical intelligence snapshots, metric trajectories, and period comparisons. |

### MCP Tool Governance
- **Strict Input Validation**: Tool inputs are checked against exact schemas; unknown or extraneous parameters are rejected immediately.
- **JSON-RPC 2.0 Compliance**: Standardized MCP response parsing and error propagation.
- **Fail-Safe Server Resilience**: If an external MCP server disconnects, requests fail safely with audit logging rather than crashing.

---

## 19. Reusable Agent Skills

Agent capabilities are encapsulated in **9 modular, versioned, and executable Skills** registered in `AgentSkillsRegistryService`. Each skill defines explicit domain instructions, input contracts, permitted MCP tools, execution constraints, and structured output schemas:

1. **`skill.financial-data-analysis`** (`v1.0.0`):  
   Computes and analyzes core financial health indicators, liquidity ratios, and working capital trends.
2. **`skill.aging-and-budget-attribution`** (`v1.0.0`):  
   Identifies overdue receivables/payables concentrations and attributes budget variances to specific cost centers.
3. **`skill.cash-flow-forecasting`** (`v1.0.0`):  
   Interprets multi-horizon cash projections, evaluates burn rates, and explains liquidity warnings.
4. **`skill.expense-and-anomaly-analysis`** (`v1.0.0`):  
   Scans for statistical expense outliers, duplicates, and vendor cost drift under the platform non-fraud policy.
5. **`skill.financial-risk-assessment`** (`v1.0.0`):  
   Evaluates composite risk levels, liquidity pressure, and customer default risk.
6. **`skill.scenario-planning`** (`v1.0.0`):  
   Runs isolated what-if stress tests (e.g., -20% revenue shock, 30-day payment delay) and evaluates closing cash impact.
7. **`skill.strategic-financial-recommendations`** (`v1.0.0`):  
   Formulates prioritized, risk-aware working capital and cash-preservation recommendations.
8. **`skill.financial-reporting`** (`v1.0.0`):  
   Drafts executive narrative briefs, variance commentaries, and structured board summaries.
9. **`skill.financial-output-verification`** (`v1.0.0`):  
   Independently audits arithmetic consistency, verifies source record citations, and confirms non-fraud language.

---

## 20. AI Model Provider Gateway

The `AiModelProviderService` provides a unified, multi-provider abstraction layer with production controls:

- **Supported Providers**:
  - **Google Gemini** (`gemini-2.5-pro` for reasoning/verification, `gemini-2.5-flash` for specialist agents) — **Live integration verified**.
  - **OpenAI** (`gpt-4o`, `gpt-4o-mini`, `o3-mini`) — Fully configurable.
  - **Anthropic** (`claude-3-7-sonnet-20250219`, `claude-3-5-haiku-20241022`) — Fully configurable.
  - **Deterministic-Grounded Fallback Engine** — Built-in offline fallback that uses deterministic financial services when external AI APIs are unconfigured or unavailable.
- **Zero-Secret Logging**: API keys are loaded strictly from server-side environment variables and are never logged, persisted in audit events, or sent to clients.
- **Resilience & Rate Limiting**: Built-in request timeouts (`10,000ms`), exponential backoff retries (`max 2 retries`), and sliding-window rate limiting (`60 req/min/org`).
- **Token & Cost Telemetry**: Tracks prompt tokens, completion tokens, execution latency, and estimated USD costs per agent run.

---

## 21. 10-Step AI Orchestration Workflow

Every multi-agent workflow request is executed through a deterministic 10-step lifecycle coordinated by `SupervisorOrchestratorService`:

```text
 1. Pre-Execution Hooks & Input Sanitization
    └── Validates JWT, verifies RBAC, checks rate limits, sanitizes prompt injection attempts.
 2. Task Classification & Agent Selection
    └── Classifies user prompt into domain intents and selects required specialist agents.
 3. Sandbox Context Initialization
    └── Creates isolated execution context with a unique sandboxId scoped strictly to organizationId.
 4. Specialist Agent Skills Loading
    └── Resolves versioned skills and binds allowlisted MCP tools from ToolRegistryService.
 5. Parallel Specialist Execution (Independent)
    └── Executes data, forecasting, anomaly, and risk agents concurrently in isolated sandboxes.
 6. Sequential Specialist Execution (Synthesis)
    └── Runs strategic recommendation and reporting agents using upstream findings.
 7. Sandboxed MCP Tool Execution
    └── Enforces permission checks and tenant boundaries on every tool invocation.
 8. Sequential Verifier AI Execution
    └── Verifies arithmetic consistency, source record citations, and non-fraud compliance.
 9. Result Synthesis & Post-Execution Lifecycle Hooks
    └── Aggregates findings, compiles token usage and cost metrics, and runs post-execution hooks.
10. Persistence & Immutable Audit Logging
    └── Stores WorkflowRun and AgentRun records with citations and logs SHA-256 AuditEvent.
```

---

## 22. Sandboxing, Lifecycle Hooks, RBAC & Security Controls

### 22.1 Agent Sandbox Security (`AgentSandboxService`)
- **Strict Multi-Tenant Isolation**: Every agent execution context is strictly bound to the requesting user's `organizationId`. Any attempt to access cross-tenant data throws an immediate security exception.
- **Least-Privilege Tool Allowlists**: Each `AgentType` can invoke only tools explicitly permitted in its configuration.
- **Globally Prohibited Operations**:
  - `raw_database_query` / direct Prisma execution
  - `shell_execute` / `child_process_exec`
  - `read_env_secrets` / reading process environment
  - `unrestricted_network_fetch`
  - `initiate_payment_transfer` / `automatic_fund_transfer`
- **Execution Guardrails**: Per-agent timeout enforcement (`15,000ms`), maximum tool call caps (`8` calls/run), and cancellation token support.
- **Sandbox Audit Logging**: Any blocked operation triggers an immediate `BLOCKED_BY_SANDBOX` entry in the immutable audit log.

### 22.2 Workflow Lifecycle Hooks (`WorkflowHooksService`)
- **Pre-Execution Hook**: Validates actor role, checks organization quotas, and sanitizes input text against prompt injection patterns.
- **During-Execution Hook**: Records timeline milestones (`TASK_CLASSIFIED`, `AGENT_STARTED`, `TOOL_INVOKED`, `AGENT_COMPLETED`) for real-time progress tracking.
- **Post-Execution Hook**: Validates verification status, flags human-review requirements, and tracks source record citation completeness.
- **Error-Handling Hook**: Safely handles failures, releases sandboxes, logs error details, and records audit trail events without leaking stack traces.

### 22.3 Role-Based Access Control (7 Platform Roles)
Enforced on every endpoint via `JwtAuthGuard` and `OrgRbacGuard`:
1. `PLATFORM_ADMIN` — Full platform administration and tenant management.
2. `BUSINESS_OWNER` — Full workspace management, financial mutations, exports, and AI workflows.
3. `FINANCE_MANAGER` — Ledger management, invoice/bill creation, budget approvals, and forecasts.
4. `ACCOUNTANT` — Ledger entries, CSV imports, invoices, payables, and reconciliations.
5. `OPERATIONS_MANAGER` — Operational spend review, payables tracking, and department budget monitoring.
6. `FINANCIAL_ANALYST` — Analytical review, what-if scenario simulations, and AI report drafting.
7. `AUDITOR` — Strictly read-only access across all records, reports, and immutable audit logs.

### 22.4 Non-Fraud Compliance Policy
The platform strictly enforces that anomaly detection signals represent statistical outliers or rule-based warnings for human review—**never** legal or factual determinations of fraud. All agent prompts, outputs, and verifier checks reinforce this policy.

---

## 23. Backend Implementation Milestones (Sections 1–24 Complete)

All 24 sections of the Capstone Backend Architecture Specification are **100% complete, fully implemented, and verified** by comprehensive unit and E2E test suites:

| Section | Domain / Capability | Status | Key Deliverables & Test Verification |
| :---: | :--- | :---: | :--- |
| **1** | System Architecture & Modular Monolith | **COMPLETED** | NestJS 12 modular monolith structure, unified error handling, logging. |
| **2** | PostgreSQL Schema & Prisma 7 ORM | **COMPLETED** | 22 models, 18 enums, `@db.Decimal(19, 4)` fixed-point precision. |
| **3** | Environment Validation & Security | **COMPLETED** | Fail-fast bootstrap validation, Helmet headers, CORS policies. |
| **4** | Health & Database Readiness Diagnostics | **COMPLETED** | Liveness (`/api/health`) and PostgreSQL readiness (`/api/health/ready`). |
| **5** | Authentication & Session Management | **COMPLETED** | Secure registration, login, JWT issuance, session revocation, password reset. |
| **6** | Multi-Tenant Workspaces & Organizations | **COMPLETED** | Organization isolation, membership management, currency configuration. |
| **7** | Role-Based Access Control (RBAC) | **COMPLETED** | 7 roles enforced via `JwtAuthGuard` and `OrgRbacGuard`. |
| **8** | Financial Accounts & Ledger Transactions | **COMPLETED** | Chart of accounts, decimal-safe double-entry ledger transactions. |
| **9** | CSV Import & Validation Pipeline | **COMPLETED** | 2-step staging, column mapping, preview, duplicate and injection checks. |
| **10** | Invoices, Bills, Payments & AR/AP Aging | **COMPLETED** | Customer invoices, supplier bills, partial payments, automated aging buckets. |
| **11** | Budgets, Revisions & Variance Analysis | **COMPLETED** | Department/category budgets, revision history, actual-vs-budget variance. |
| **12** | Deterministic Financial Analytics | **COMPLETED** | 10 financial metrics with exact formulas, missing-data guards, and citations. |
| **13** | Multi-Horizon Cash-Flow Forecasting | **COMPLETED** | 7/30/60/90-day projections across Base, Optimistic, and Conservative scenarios. |
| **14** | FinMemory Historical Intelligence | **COMPLETED** | Monthly metric snapshots, completeness scoring, period and forecast comparisons. |
| **15** | Anomaly & Risk Monitoring | **COMPLETED** | 8 anomaly detectors, non-fraud triage policy, alert resolution workflows. |
| **16** | Scenario Planning & What-If Simulations | **COMPLETED** | Non-destructive sensitivity simulations (revenue shocks, expense spikes). |
| **17** | Core Platform E2E Integration Suite | **COMPLETED** | Full PostgreSQL E2E suite covering all financial and auth domains. |
| **18** | Reports, Exports, Tasks & Notifications | **COMPLETED** | Business Health, Cash-Flow, and Risk reports with RBAC-controlled CSV exports. |
| **19** | Immutable SHA-256 Audit Trail | **COMPLETED** | Cryptographic hash chaining on all sensitive financial and security events. |
| **20** | Multi-Agent Supervisor & 8 Specialists | **COMPLETED** | Orchestration coordinator, 8 specialized agents, and Verifier AI Agent. |
| **21** | Model Context Protocol (MCP) Integration | **COMPLETED** | MCP service and tool registry with 8 approved deterministic tools. |
| **22** | Reusable Agent Skills Registry | **COMPLETED** | 9 versioned executable skills with schemas and execution constraints. |
| **23** | AI Model Provider Gateway | **COMPLETED** | Multi-provider gateway (Gemini live verified, OpenAI, Anthropic, Fallback). |
| **24** | Agent Sandboxing, Lifecycle Hooks & E2E | **COMPLETED** | Sandbox security, Pre/During/Post/Error hooks, and complete E2E verification. |

---

## 24. Remaining Optional Production Enhancements

The core backend platform is completely implemented and production-ready. The following optional enhancements represent client-side and cloud-infrastructure extensions:

1. **Frontend Dashboard UI Pages**:
   - Connecting the React frontend components to the completed `/api/v1` backend endpoints for transactions, invoices, cash-flow forecasting charts, and the AI agent chat/queue interface (the complete integration contract is ready in [`docs/frontend-api-integration.md`](./docs/frontend-api-integration.md)).
2. **External Accounting & Banking OAuth Sync**:
   - Optional live OAuth2 connectors for automated daily bank statement synchronization (e.g., Plaid, QuickBooks, Xero).
3. **Cloud Infrastructure & CI/CD Deployment**:
   - Containerized cloud deployment orchestration (e.g., Kubernetes, AWS ECS, Google Cloud Run) with automated GitHub Actions CI/CD pipelines.

---

## 25. Technical References & Documentation

For detailed technical specifications, API schemas, and architecture diagrams, refer to:

- **[`backend/README.md`](./backend/README.md)**: Deep dive into the backend architecture, module map, decimal arithmetic standards, and testing procedures.
- **[`docs/frontend-api-integration.md`](./docs/frontend-api-integration.md)**: Comprehensive REST API contract with endpoints, request/response DTOs, headers, and error envelopes.
- **[`docs/architecture.md`](./docs/architecture.md)**: System architecture specification, security posture, and domain boundaries.
- **[`docs/setup-guide.md`](./docs/setup-guide.md)**: Step-by-step development environment setup and database initialization guide.
- **[`docs/development-notes.md`](./docs/development-notes.md)**: Engineering decisions, package version compatibility, and design tokens.
