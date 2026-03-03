# ai HandyBoost™

**AI-powered local handyman dispatch platform** — Voice-first, no-app, SMS-based service matching for local markets.

---

## Overview

ai HandyBoost connects customers who need handyman services with local professionals through **AI voice**, **SMS**, and **mobile web** — no app download required. Built for lean operations and designed for scalability and future AI ecosystem integration.

### Highlights

| Traditional approach        | ai HandyBoost                          |
|----------------------------|----------------------------------------|
| Search → Website → Call    | Call/SMS → AI → Instant dispatch       |
| App download, login        | SMS link → Mobile web, zero friction   |
| Subscription-heavy         | Pay-per-job model                      |

---

## Screenshots

### Dashboard (Overview)

![Dashboard Overview](docs/screenshots/dashboard-overview.png)

*KPI cards, job/vendor/customer counts, and high-level metrics.*

### Jobs & Vendors

| Jobs list | Active Vendors |
|-----------|----------------|
| ![Jobs](docs/screenshots/dashboard-jobs.png) | ![Vendors](docs/screenshots/dashboard-vendors.png) |

### Regional Data & Operations

| Regional data (ZIP/map) | Operations (SMS, logs) |
|-------------------------|-------------------------|
| ![Regional](docs/screenshots/dashboard-regional.png) | ![Operations](docs/screenshots/dashboard-operations.png) |

---

## Architecture (High-Level)

```
┌─────────────────────────────────────────────────────────────┐
│  INPUT CHANNELS                                             │
│  SMS  ·  Mobile Web  ·  (Future: External AI / MCP)         │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  CONTEXT LAYER                                               │
│  Customer  ·  Vendor  ·  Job  ·  Interaction                 │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  CORE AGENT ENGINE                                           │
│  Rule-based matching (ZIP, availability) → extensible        │
└─────────────────────────────┬───────────────────────────────┘
                              ▼
┌─────────────────────────────────────────────────────────────┐
│  ACTION LAYER                                                │
│  SMS  ·  Database  ·  Payment Links  ·  Mobile Web          │
└─────────────────────────────────────────────────────────────┘
```

- **Modular monolith** — Bounded domains (jobs, vendors, customers, agent, actions); single deploy, extract to services when needed.
- **Event-driven** — Domain events (e.g. job created, vendor accepted) drive workflows.
- **Optional async** — Sync by default; Redis/ARQ for fast webhook response when required.
- **MCP-ready** — Context and tools designed for future Model Context Protocol exposure (e.g. ChatGPT, voice assistants).

---

## Tech Stack

| Layer      | Technology                          |
|-----------|--------------------------------------|
| Backend   | FastAPI, Python 3.11+                |
| Database  | PostgreSQL (Supabase / self-hosted)  |
| Queue     | ARQ + Redis (optional)               |
| SMS       | Telnyx                               |
| Payments  | Stripe Payment Links                 |
| Voice     | Optional (e.g. Vapi)                 |
| Frontend  | Admin dashboard (server-rendered); mobile web (Next.js) |
| Hosting   | VPS / PaaS (e.g. Vultr, Railway)      |

---

## Project Structure (Summary)

```
├── apps/
│   ├── core/          # Config, events, dependencies
│   ├── context/       # Domain context (Customer, Vendor, Job)
│   ├── events/        # Webhooks (SMS, optional voice)
│   ├── agent/         # Core matching engine
│   ├── actions/       # SMS, DB, Stripe, mobile links
│   ├── jobs/          # Job domain
│   ├── vendors/       # Vendor domain
│   ├── customers/     # Customer domain
│   └── admin/         # Dashboard, KPIs, operations
├── tasks/             # Background jobs (ARQ)
├── static/            # Admin UI assets
└── main.py            # FastAPI app entry
```

---

## Implemented Features (Summary)

- **Admin dashboard** — KPIs, jobs/vendors/customers lists, regional data (ZIP + map), SMS/call logs, monthly reports, activity log, survey and outreach tools.
- **Vendor flow** — SMS link → mobile web accept/complete (no app, no login).
- **Customer flow** — Voice/SMS intake → job creation → matching by ZIP/availability.
- **Payments** — Stripe payment links sent after job completion.
- **Data acquisition** — Scheduled vendor discovery by region (ZIP); optional Places APIs.
- **Automation** — Scheduled collection runs, monthly report generation, survey email batches.

---

## License

Proprietary. For portfolio and demonstration purposes only.

---

*This repository is a **showcase** for ai HandyBoost. The live product and full source are maintained separately.*
