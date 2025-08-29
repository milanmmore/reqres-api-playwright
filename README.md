# 🎭 Reqres API Testing with Playwright

This repo demonstrates API testing using [Playwright](https://playwright.dev/) against the [Reqres](https://reqres.in/) demo API. It includes modular test cases, environment handling, CI/CD integration, and Slack notifications.

---

## 🚀 Quick Start

### 🔧 Prerequisites

- Node.js ≥ 18
- npm
- GitHub account (for CI/CD)
- Slack webhook (optional)

### 📦 Install Dependencies

```bash
npm ci

```

### 🌍 Environment Setup

Create a .env.local file in the root:

```
API_BASE_URL=https://reqres.in/api
DEFAULT_TIMEOUT=5000
LOG_LEVEL=debug
API_KEY=reqres-free-v1
```

Sensitive values like API_KEY should be injected via GitHub Secrets. But to run locally you need to add in end.local file.

Note : please check https://reqres.in for updated API_KEY

### Run Tests Locally

```
npx playwright test --reporter=html
```

This generates an HTML report in playwright-report/index.html.

### 🧰 Folder Structure

```
reqres-api-playwright/
├── fixtures/
│   ├── data-store.ts          # store and use Dynamically created data
│   ├── payloads.ts            # All the payloads (Body Parameter)
│   ├── queryParams.ts         # Query Parameters
├── node_modules/              # Auto-generated; holds all packages(gitignored)
├── tests/                     # Main test specs
│   ├── 1_create.spec.ts       # Create user (POST)
│   ├── 2_read.spec.ts         # Read user data (GET)
│   ├── 3_update.spec.ts       # Update user (PUT/PATCH)
│   ├── 4_delete.spec.ts       # Delete user (DELETE)
│   ├── 5_auth.spec.ts         # Login/Register flows
│   ├── 6_performance.spec.ts  # Response time checks
│   ├── common/                # Shared setup logic
│   │   └── setup.ts
├── utils/                     # Reusable helpers
│   ├── generateRandomData.ts  # Dynamic payload generation
│   └── scoreHeaders.ts        # Performance header scoring
├── .env.local                 # Local config (gitignored)
├── .env.example               # Template for non-sensitive vars
├── package.json               # Declares dependencies and scripts
├── playwright.config.ts       # Playwright setup and test runner config
├── README.md
├── tsconfig.json              # TypeScript compiler config

```

### ⚙️ GitHub Actions Workflow

- CI/CD runs on every push to main:

- Installs dependencies

- Rebuilds .env.local from .env.example + secrets

- Runs Playwright tests

- Uploads HTML report as artifact

- Sends Slack notification

### 🔐 Secrets Required

```

| Key             | Purpose             |
|-----------------|---------------------|
| API_KEY         | Auth for Reqres API |
| SLACK_WEBHOOK	  | Slack notifications |

```

### Test Coverage

## This repo includes:

- # 🔄 CRUD Analysis

  - Create: Validates user creation with POST requests
  - Read: Fetches user data and validates response structure
  - Update: Tests PUT/PATCH operations for user updates
  - Delete: Ensures DELETE endpoints respond correctly and clean up data

- # 🔐 Authentication Handling

  - Simulates login flows using /login and /register endpoints
  - Validates token generation and error handling for invalid credentials
  - Tests protected routes with and without valid tokens

- # ⚡ Basic API Performance Tests

  - Measures response time thresholds for key endpoints
  - Flags slow responses using configurable timeout values
  - Logs performance metrics for CI/CD visibility

- # ⚙️ CI/CD Orchestration with Caching

  - ✅ Smart caching of dependencies (e.g., Playwright binaries, Node modules) to reduce build time
  - 🔁 Test result caching for reruns and flaky test isolation
  - 📦 Artifact reuse across jobs (e.g., sharing setup or environment files between matrix runs)

- # 🧩 Modular job definitions for parallel spec execution and environment overrides

  - 🛠️ Fallback strategies for cache misses (e.g., auto-install with logging)

- # 📣 Slack Integration for Visibility

  - 📤 Automated test summary notifications with pass/fail stats and links to reports
  - 🧵 Threaded alerts per spec group (e.g., CRUD, auth, performance) for better traceability
  - 🚦 Status indicators (✅/❌/⚠️) for quick triage by team members
  - 🔗 Deep links to CI artifacts (e.g., HTML reports, screenshots, logs)

- # 📁 Artifact Management and Reporting

  - 📄 HTML report generation with collapsible sections and dark mode support
  - 🖼️ Screenshot and trace uploads for failed tests (Playwright .zip or .html)
  - 📊 Performance dashboards using custom headers and response time scoring
  - 🧪 Spec-wise report segregation for interview prep and modular review
  - 🔐 Secure artifact handling via GitHub Actions + Vault/Secrets integration
