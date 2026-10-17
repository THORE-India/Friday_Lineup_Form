# THORE PULSE — Friday Lineup Form

A compact client-side application used by The House of Real Estate (THORE) Operations team to collect and manage weekly "Friday Lineup" sales visit entries and to present an administrative dashboard for campaign/marketing workflows.

This repository contains a static front-end (HTML/CSS/JavaScript) that acts as a thin client for two backend services: a Supabase project (used as the primary data store/auth backend) and a Google Apps Script endpoint used to update a CRM (called "Leadrat" in the code). The project is intended for internal operational use by THORE's sales and marketing teams.

## Overview and objectives

- Provide a simple, mobile-friendly form (index.html) for sales executives to schedule weekend customer visits and record lead/visit metadata.
- Surface an administrative dashboard (dashboard.html) for marketing/campaign workflows and campaign approval pipelines.
- Keep the client-side app lightweight so it can be hosted on any static hosting platform and connect to Supabase + an optional CRM webhook (Google Apps Script) for cross-system updates.

## Key features

- Responsive, accessible HTML/CSS form for submitting visit/lead entries.
- Client-side validation, date/time pickers (flatpickr), and autocomplete patterns for project/developer fields.
- Dual submission flow visible to users: the app attempts to write to Supabase and also make a CRM update; the UI reports status for both operations.
- Administrative dashboard with charts and tables (uses Chart.js) that reads from Supabase tables and supports role-based views inside the dashboard code.
- Simple gating logic: the form is only available during a configurable weekly window (the code enforces Friday 10:00–21:00 and additionally checks an external access.json file).

## Technology stack

- Language(s): HTML, CSS, JavaScript (ES6)
- Framework / runtime: Static client-side web app (no server-side runtime included)
- Notable libraries and services:
  - Supabase JS client (@supabase/supabase-js) — used as the primary BaaS platform
  - flatpickr — date/time picker in the form
  - Chart.js — charts used on the dashboard
  - Google Apps Script (external) — optional CRM update endpoint

## High-level architecture

- index.html — public user-facing form for Sales Executives. Collects input and performs two actions:
  1) Inserts/updates lead entry in Supabase.
  2) Calls a Google Apps Script endpoint (Leadrat) to update CRM lead status.
- dashboard.html — administrative view, authenticates via Supabase session, reads multiple Supabase tables and renders metrics, boards, and campaign details (marketing workflow UI).
- marketing.html, closed.html, no_authorization.html — supporting pages shown in specific conditions (marketing workflows, closed/gated access, and access-denied views).

How it fits together: the static pages are intended to be hosted as static assets. The pages use Supabase client credentials included in the files to read and write data; the dashboard enforces role-specific UI in JavaScript and relies on Supabase RPCs and tables for access control and data. A Google Apps Script URL is used to call an external CRM updater.

## Repository structure

```
LICENSE                 # MIT license
README.md               # (this file)
index.html              # Main Friday Lineup form (entry point for Sales Executives)
dashboard.html          # Administrative dashboard (Marketing / Operations)
marketing.html          # Marketing workflow UI (related to campaigns)
closed.html             # User-facing "form closed" page used when outside open window
no_authorization.html   # Access-denied page
```

Files of interest inside HTML pages (searchable):
- index.html: constants SUPABASE_URL and SUPABASE_ANON_KEY, LEADRAT_GAS_URL, LEADRAT_APP_SECRET, ACCESS_JSON_URL, and the main form submission logic (submitForm), weekend date calculations, and UI helpers.
- dashboard.html / marketing.html: substantial client-side logic that queries Supabase tables, uses Chart.js, and implements a multi-role marketing/campaign workflow.

## Prerequisites

- Modern web browser (Chrome, Edge, Firefox, Safari) with JavaScript enabled.
- Internet access for the CDN-hosted libraries (flatpickr, Chart.js, Supabase JS) and the remote services the pages expect (Supabase, the Google Apps Script CRM URL, external access.json).
- Optional: a Supabase project (to host data) and a deployed Google Apps Script (if you want the CRM update behavior).

## Safe local development and preview

These are static pages — no build step is required. For best results, serve files over a local HTTP server (some browsers block certain fetch operations when loading files via file://).

Example (from repository root):

- Using Python 3's simple server:

  python -m http.server 8000

  Then open: http://localhost:8000/index.html

- Using a lightweight live server (npm package) for auto-reload during edits:

  npx live-server --port=8000

Notes:
- The dashboard page performs Supabase authentication (sign-in with email/password) and will try to call Supabase endpoints. To use the dashboard you will need a Supabase project and credentials (see configuration section).

## Configuration (safe example)

This repository includes configuration values inline in the HTML files (for example, SUPABASE_URL and SUPABASE_ANON_KEY). Do not commit secrets to the repo. Instead, for local testing create a small config file and include it before the application script.

Example safe file: config.example.js

```js
// Copy to config.js and fill with your project's values. NEVER commit real secrets to public repos.
window.APP_CONFIG = {
  SUPABASE_URL: 'https://your-project.supabase.co',
  SUPABASE_ANON_KEY: 'public-anon-key-placeholder',
  LEADRAT_GAS_URL: 'https://script.google.com/macros/s/your-script-id/exec',
  LEADRAT_APP_SECRET: 'replace-with-secret',
  ACCESS_JSON_URL: 'https://raw.githubusercontent.com/your-org/your-repo/main/access.json'
};
```

To use this approach, edit the HTML files to load config.js before the main inline script and update the code to read values from window.APP_CONFIG instead of hard-coded constants. (Search for SUPABASE_URL and SUPABASE_ANON_KEY in index.html and dashboard.html.)

Assumption: the repository's current code stores configuration directly in the HTML files. I recommend extracting configuration to a separate file (as above) before deploying or sharing the site.

## How to run (summary)

- Quick preview (no configuration):
  - Serve the repository root with a local HTTP server and open index.html. The form will operate in a limited client-only mode (it attempts to fetch an external access.json and may redirect to closed.html if not open).

- Dashboard usage (requires Supabase):
  - Create a Supabase project and the necessary tables (the dashboard code references tables such as `profiles`, `campaign_requests`, `campaign_items`, `project_master`; the exact schema and RLS policies are not included in this repo).
  - Provide a Supabase URL and anon key by editing a config file or replacing the inline constants in the HTML.
  - Open dashboard.html and sign in using Supabase-authenticated credentials.

## Deployment overview

This is a static site and can be hosted on any static hosting provider (GitHub Pages, Netlify, Vercel, S3+CloudFront). For public hosting:

- Remove or rotate any embedded secrets.
- Prefer hosting the front-end and exposing only a secured backend for any CRM or privileged operations.
- If you use Supabase, configure RLS and row-level permissions to avoid exposing or allowing unsafe writes from client-side anon keys.

## Security & privacy considerations (important)

During analysis of the repository I found values embedded in the client-side HTML that are sensitive and should be addressed immediately:

- Supabase URL and anon key are present inline in index.html and other pages. Publicly publishing an anon key can expose your Supabase project to misuse if RLS/permissions are not correctly configured.
- A Google Apps Script endpoint URL and an associated app secret (LEADRAT_APP_SECRET) are present inline. Exposed webhook URLs and shared secrets allow unauthorized third parties to call or impersonate your CRM update endpoint.

Recommendations:
- Rotate the exposed Supabase key(s) and the Google Apps Script secret immediately.
- Move any privileged operations (CRM updates, secrets handling) to a server-side function where secrets are stored securely and requests are validated.
- Ensure Supabase RLS (Row Level Security) policies are applied and limit anonymous write operations.
- Do not commit secrets; use environment/configuration management supported by your hosting platform.

## Known limitations (verified)

- The app is purely client-side; sensitive operations performed entirely on the client (using embedded keys) are a security risk unless Supabase policies mitigate them.
- The repository does not include server-side code or a deployment pipeline. The exact Supabase database schema and required RPCs/policies are not provided.
- The dashboard enforces role-based UI in JavaScript and depends on Supabase RLS / RPCs whose behavior must be provisioned separately.
- The form gating depends on an external access.json hosted on another GitHub repository; if that file is unavailable the form may be closed or show an error.

## Contribution

If you want to contribute improvements:

- Open an issue describing the change you'd like to make.
- Fork the repository and submit a pull request with a concise description and screenshots where applicable.
- Keep secrets out of commits. Use the config.example.js pattern shown above.

## License

This repository includes an MIT License (see LICENSE).

---

If you want, I can:
- Produce a minimal config.js and a short patch to the HTML pages to read configuration from it (I will not commit secrets); or
- Draft guidance for the Supabase schema and RLS policies the dashboard expects (I will only propose a minimal, safe schema and will avoid sensitive production details).
