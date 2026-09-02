# w⁴ My Lawfirm 

[![W4 OS Platform](https://img.shields.io/badge/Platform-W4%20OS%20%7C%20YouMeOS-00d2ff?style=for-the-badge&logo=electron&logoColor=white)](https://github.com)
[![Ecosystem](https://img.shields.io/badge/Ecosystem-Xophz%20COMPASS-62c9ff?style=for-the-badge&logo=compass&logoColor=white)](https://github.com)
[![Monorepo](https://img.shields.io/badge/Monorepo-pnpm%20Workspaces-f69220?style=for-the-badge&logo=pnpm&logoColor=white)](https://pnpm.io)
[![Frontend](https://img.shields.io/badge/Frontend-AngularJS%201.2%20%2B%20Vite%206-dd0031?style=for-the-badge&logo=angular&logoColor=white)](https://vitejs.dev)
[![Backend](https://img.shields.io/badge/Backend-PHP%20%2B%20WordPress%20REST-777bb4?style=for-the-badge&logo=php&logoColor=white)](https://wordpress.org)
[![Database](https://img.shields.io/badge/Database-MariaDB%2011-003545?style=for-the-badge&logo=mariadb&logoColor=white)](https://mariadb.org)
[![i18n](https://img.shields.io/badge/i18n-4%20Locales%20(EN%20%7C%20PT%20%7C%20ES%20%7C%20FR)-success?style=for-the-badge)](apps/lawfirm/app/js/dictionaries/)
[![Test Suite](https://img.shields.io/badge/Tests-152%2F152%20Passing-brightgreen?style=for-the-badge)](apps/lawfirm/scripts/test-i18n.js)

Turnkey legal practice management, lawsuit and case lifecycle tracking, client portal, financial administration, and document management platform built for W4 OS, YouMeOS, and Xophz COMPASS.

---

## Table of Contents

* [Overview](#overview)
* [Key Features](#key-features)
* [Architecture and Workspace Layout](#architecture-and-workspace-layout)
* [Docker and Local Services](#docker-and-local-services)
* [Setup and Installation](#setup-and-installation)
* [Development Workflow](#development-workflow)
* [Internationalization (i18n) Engine](#internationalization-i18n-engine)
* [Testing and Quality Assurance](#testing-and-quality-assurance)
* [REST API and RPC Gateway](#rest-api-and-rpc-gateway)
* [Honorable Mentions](#honorable-mentions)
* [License](#license)

---

## Overview

W4 Lawfirm provides law offices, solo practitioners, and corporate legal departments with an integrated solution for tracking legal matters, court filings, client registries, schedules, and billing workflows. 

Packaged as a modern monorepo, the platform combines a responsive single-page application (SPA) with a resilient WordPress / YouMeOS Spark plugin and a containerized backend.

---

## Key Features

* **Case and Lawsuit Tracking:**
  * Complete lifecycle management across distribution, court hearings, appeals, and settlements.
  * Tribunal, court division, procedural class, and judge metadata indexing.
  * Activity timelines, case status indicators, and movement history.
* **Client Registry and CRM:**
  * Unified management of individuals (PF) and corporate entities (PJ).
  * Contact profiles, address management, representation records, and associated matters.
* **Financial Administration:**
  * Accounts payable and accounts receivable tracking.
  * Fee schedules, retainers, expense reimbursements, and payment status tracking.
  * Company-scoped financial charts and transaction ledgers.
* **Agenda and Calendar:**
  * Hearing dates, statutory deadline trackers, and task assignments.
  * Team scheduling and notification triggers.
* **Document and Media Management:**
  * Multipart document upload and secure file streaming.
  * Tenant-isolated file storage scoped by company account.
* **Enterprise Internationalization (i18n):**
  * Native 4-locale runtime switching without browser reload: English (`en_US`), Portuguese (`pt_BR`), Spanish (`es_ES`), and French (`fr_FR`).
  * 100% dictionary key parity across semantic keys with multi-tier fallback hierarchy.
* **Xophz COMPASS / YouMeOS Spark Integration:**
  * Native registration with the YouMeOS Spark ecosystem (`/wp-json/my-lawfirm/v1/manifest`).
  * Flexible frontend routing: standalone routes (`/lawfirm`, `/my-lawfirm`), configurable custom slug, or full site takeover.

---

## Architecture and Workspace Layout

The repository uses pnpm workspaces to manage the client app and server plugin concurrently.

```
lawfirm/
├── .env.example                               # Default environment configurations
├── docker-compose.yml                         # Container definitions for DB, WordPress, Node, and PMA
├── package.json                               # Root monorepo orchestration scripts
├── pnpm-workspace.yaml                        # Workspace packages definition
├── apps/
│   └── lawfirm/                               # Client SPA package (diego-lawfirm)
│       ├── app/                               # AngularJS 1.2+ application source
│       │   ├── css/                           # Styling and layout assets
│       │   ├── img/                           # Application imagery and icons
│       │   ├── js/                            # Application logic
│       │   │   ├── app.js                     # Main module, routes, and constants
│       │   │   ├── controllers/               # 35+ view controllers
│       │   │   ├── dictionaries/              # en_US, pt_BR, es_ES, fr_FR UMD catalogs
│       │   │   ├── directives.js              # Custom UI directives
│       │   │   ├── i18n.js                    # Core runtime i18n engine and filters
│       │   │   └── services.js                # Core services, API RPC, and feedback notifications
│       │   ├── view/                          # 45+ HTML view templates and partials
│       │   └── index.html                     # Application HTML shell
│       ├── scripts/                           # Tooling, CLI generators, audit, and test suites
│       │   ├── build.js                       # Build script syncing bundle to plugin public/dist
│       │   ├── i18n-audit.js                  # Automated translation audit and missing key scanner
│       │   ├── i18n-generate.js               # CLI generator to scaffold new locale catalogs
│       │   └── test-i18n.js                   # Native zero-dependency Node.js test suite
│       └── vite.config.js                     # Vite dev server and bundle config
└── plugins/
    └── xophz-compass-diego-lawfirm/           # WordPress & YouMeOS backend plugin
        ├── admin/                             # Admin menus, settings, and rewrite flushing
        ├── includes/                          # PHP backend core
        │   ├── class-diego-lawfirm-api.php    # REST API endpoints controller
        │   ├── class-diego-lawfirm-core.php   # Business logic and entity models
        │   ├── class-diego-lawfirm-db.php     # 35+ practice database table managers
        │   ├── class-diego-lawfirm-rpc.php    # RPC action dispatcher
        │   └── class-diego-lawfirm-connectors.php # YouMeOS / COMPASS spark registry
        ├── public/                            # Public endpoint handlers and compiled assets
        └── xophz-compass-diego-lawfirm.php    # Main plugin entrypoint
```

---

## Docker and Local Services

The development environment is containerized via `docker-compose.yml`:

| Service | Container Name | Host Port | Internal Port | Description |
|---|---|---|---|---|
| **YouMeOS / WordPress** | `w4-lawfirm` | `80` / `443` | `80` / `443` | Core WordPress engine with custom plugins mounted |
| **MariaDB** | `w4-lawfirm-db` | `3306` | `3306` | Relational database hosting 35+ practice tables |
| **Node Dev Server** | `w4-lawfirm-node` | `8090` | `8090` | Vite development server for instant hot-reloading |
| **phpMyAdmin** | `w4-lawfirm-pma` | `8080` | `80` | Database management web interface |

---

## Setup and Installation

### 1. Prerequisites

* [Docker](https://www.docker.com/) and Docker Compose v2+
* [Node.js](https://nodejs.org/) v20.x or higher
* [pnpm](https://pnpm.io/) v9.x or higher

### 2. Clone and Configure Environment

```bash
# Clone the repository
git clone https://github.com/your-org/lawfirm.git
cd lawfirm

# Copy environment defaults
cp .env.example .env
```

### 3. Start Containerized Infrastructure

```bash
docker compose up -d
```

### 4. Install Dependencies

```bash
pnpm install
```

---

## Development Workflow

### Root Monorepo Commands

* **Start Development Mode (Vite Server):**
  ```bash
  pnpm dev:lawfirm
  ```
* **Build Frontend Assets:**
  Compiles and synchronizes all application assets into `plugins/xophz-compass-diego-lawfirm/public/dist/`:
  ```bash
  pnpm build:lawfirm
  ```

### Accessing Local Services

* **Application Frontend:** `http://localhost/lawfirm` (or custom configured slug)
* **Vite Dev Server Direct:** `http://localhost:8090`
* **phpMyAdmin Console:** `http://localhost:8080` (Server: `mariadb`, User: `wordpress`, Password: `wordpress`)
* **WordPress Admin:** `http://localhost/wp-admin`

---

## Internationalization (i18n) Engine

The application includes an internationalization architecture built in [apps/lawfirm/app/js/i18n.js](file:///home/xopher/www/x/w4/lawfirm/apps/lawfirm/app/js/i18n.js).

### Features

* **Global and AngularJS Directives:**
  * Direct text translation: `i18n="NAV_CLIENTS"` or `{{ 'NAV_CLIENTS' | i18n }}`
  * Attribute translation: `i18n-placeholder="SEARCH_PROMPT"`, `i18n-title="EDIT_ITEM"`, `i18n-value="SAVE_CHANGES"`, `i18n-aria-label="CLOSE_MODAL"`
  * Dynamic locale selector: `<span i18n-switcher></span>`
* **Multi-Tier Fallback Hierarchy:**
  `Active Locale Dictionary` -> `WordPress wp.i18n` -> `Base en_US Dictionary` -> `Fallback pt_BR Dictionary` -> `Raw Key String`
* **Pluralization and Interpolation:**
  * Named substitution: `__('WELCOME_USER', { name: 'Alex' })`
  * Positional substitution: `__('PAGE_COUNT', 1, 10)`
  * Pluralization: `_n('FILE_COUNT_ONE', 'FILE_COUNT_OTHER', count, { count: count })`
* **Context Disambiguation:**
  `_x('CLOSE', 'action')` vs `_x('CLOSE', 'proximity')`

### Supported Locales

| Locale Code | Language | Native Name | Dictionary Location |
|---|---|---|---|
| `en_US` | English (United States) | English | [en_US.js](file:///home/xopher/www/x/w4/lawfirm/apps/lawfirm/app/js/dictionaries/en_US.js) |
| `pt_BR` | Portuguese (Brazil) | Português (Brasil) | [pt_BR.js](file:///home/xopher/www/x/w4/lawfirm/apps/lawfirm/app/js/dictionaries/pt_BR.js) |
| `es_ES` | Spanish (Spain) | Español | [es_ES.js](file:///home/xopher/www/x/w4/lawfirm/apps/lawfirm/app/js/dictionaries/es_ES.js) |
| `fr_FR` | French (France) | Français | [fr_FR.js](file:///home/xopher/www/x/w4/lawfirm/apps/lawfirm/app/js/dictionaries/fr_FR.js) |

### CLI Translation Utilities

* **Automated Translation Audit:**
  Scans all HTML templates and JS controllers for translation coverage, missing keys, and dictionary parity:
  ```bash
  node apps/lawfirm/scripts/i18n-audit.js
  ```
* **Locale Generator:**
  Scaffolds a new dictionary catalog adhering to 100% key parity with the base catalog:
  ```bash
  node apps/lawfirm/scripts/i18n-generate.js --locale de_DE --name "Deutsch"
  ```

---

## Testing and Quality Assurance

The project includes an opaque-box, requirement-driven test suite built on native Node.js (`node:test` and `node:assert`).

```bash
# Run the full test suite
node apps/lawfirm/scripts/test-i18n.js
```

### Test Suite Structure

* **Tier 1 (Feature Coverage):** 65 unit tests covering core translation methods, switching logic, fallback chains, pluralization, directives, and dictionaries.
* **Tier 2 (Boundary & Corner Cases):** 65 tests verifying empty strings, prototype pollution defense, missing parameters, invalid locale codes, and malformed inputs.
* **Tier 3 (Cross-Feature Combinations):** 16 pairwise interaction tests verifying runtime switching across active DOM directives, async controller responses, and UMD dictionary updates.
* **Tier 4 (Real-World Workflows):** 6 end-to-end user workflows (Client CRUD, Lawsuit Tracking, Financial Ledgers, Permissions Management, SPA Navigation, and Locale Scaffolding).

---

## REST API and RPC Gateway

The backend exposes standardized endpoints under `/wp-json/my-lawfirm/v1` (with `/wp-json/diego-lawfirm/v1` alias):

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/wp-json/my-lawfirm/v1/manifest` | Returns the YouMeOS Spark structural manifest and integration metadata. |
| `GET` | `/wp-json/my-lawfirm/v1/status` | Returns system diagnostics, active database tables, and connection health. |
| `POST` | `/wp-json/my-lawfirm/v1/rpc` | Main authenticated RPC controller (Clients, Processes, Tasks, Finance, Auth). |
| `POST` | `/wp-json/my-lawfirm/v1/upload` | Secure multipart upload handler with company tenancy isolation. |
| `POST` | `/wp-json/my-lawfirm/v1/download` | Secure binary stream download handler for stored case documentation. |

---

## Honorable Mentions

* **Diego Carreira:** Original creator and architect of the ADVCOMPANY legal practice management domain model and database architecture.
* **YouMeOS & W4 OS Core Team:** System architecture for the containerized OS runtime, Spark registration layer, and platform connectors.
* **Xophz COMPASS Engineering:** Monorepo modernization, Vite bundling pipeline, multi-locale i18n engine refactor, and automated test suite.

---

## License

Internal proprietary software. All rights reserved under the W4 OS / Xophz COMPASS ecosystem guidelines.