<h1 align="center">Solory</h1>

<p align="center">
  <strong>The operating system for Swiss sole traders.</strong><br>
  Swiss QR-bill invoicing, clients, time tracking, expenses and contracts in one calm workspace, with an AI assistant that never acts without your approval.
</p>

<p align="center">
  <strong>Launching soon</strong> · waitlist open at <a href="https://www.solory.ch">solory.ch</a>
</p>

<p align="center">
  <a href="#what-it-does">What it does</a> · <a href="#how-its-built">How it's built</a> · <a href="#status">Status</a> · <a href="#about">About</a>
</p>

<p align="center">
  <a href="https://www.solory.ch"><img src="assets/hero.png" width="100%" alt="Two Solory app windows in dark mode: invoice QR-2026-014 with three line items, VAT 8.1%, a total of CHF 3'675.40, a Swiss QR-bill and a matched bank credit, and behind it a camt.053 bank reconciliation with 4 of 4 credits auto-matched. All names and amounts are demo data."></a><br>
  <sub>Product UI with demo data</sub>
</p>

## What it does

- **Swiss QR-bill invoicing.** Invoices carry the Swiss QR payment part with a structured reference, with Swiss VAT (MWST) applied, recurring invoices and payment reminders.
- **Clients and leads.** One CRM for clients, leads and contacts, with the full history of each a click away.
- **Time and expenses.** Track billable hours and turn them into an invoice. Photograph a receipt and the expense is prefilled, VAT lines included.
- **Contracts with e-signature.** Draft from presets, send a one-time signing link, let the signer confirm with a code sent by email, and archive the signed PDF with its certificate page.
- **Bank reconciliation.** Import a camt.053 statement and incoming payments find their invoices. Exact reference matches are recorded automatically; anything less certain waits for your confirmation.
- **An AI assistant that asks first.** It drafts email replies and turns a plain-language request into a multi-step plan, shows exactly what will change, and runs nothing until you approve it.

### On the phone

Solory installs as a web app with push notifications. A receipt photographed on the go is read into a prefilled expense, and with no signal the photo waits in an offline queue until the connection is back.

<p align="center">
  <img src="assets/mobile.png" width="520" alt="Two phones showing Solory on mobile in dark mode: invoice QR-2026-014 with its Swiss QR-bill and matched bank credit, and a bank reconciliation view with 4 of 4 credits matched. Demo data.">
</p>

## How it's built

**Stack:** Next.js · React · TypeScript · Supabase (PostgreSQL) · CSS Modules · OpenAI API · Mailgun · Vitest · Playwright · Vercel

- **Approval is enforced on the server, not in the UI.** Planning is read-only: the model's reply is parsed into a fixed set of typed actions with validated parameters, anything else is dropped, and names are resolved against the account's own records. Once the user confirms, the server validates and resolves the plan again and runs each step through the same actions the forms use, so every permission check still applies. AI usage has a hard monthly cap per account, and every AI feature falls back to plain, non-AI output when AI is off.
- **QR-bills built to the standard.** The 210 x 105 mm payment part is rendered as a separate PDF page and appended to the invoice. Every bill carries a reference derived from the invoice number: a QR reference on a QR-IBAN, an ISO 11649 creditor reference on a regular IBAN. Settled invoices, credit notes and out-of-range amounts never get a payment slip, and the slip follows the invoice's language wherever the standard allows it.
- **Bank import that is safe to repeat.** A bank-agnostic ISO 20022 parser reads camt.053 statements and camt.054 notifications and gives every individual payment a stable key, so re-importing a file never duplicates a payment and a batched QR collection never collapses into one row. Only an exact structured reference is matched automatically, and a payment the user unmatched on purpose stays unmatched.
- **Every account is a closed box.** The server resolves the active account and verifies team membership before any read or write, and Postgres row-level security enforces the same boundary underneath. Optional TOTP two-factor authentication is enforced at the data and file layer, not only on screen. The database and file storage are hosted in Switzerland (Zürich region).
- **Invoices that stay what they were.** An issued invoice can be sealed: SHA-256 fingerprints of its content and of the archived PDF are stored, and database triggers make it immutable. Closed accounting periods can be locked the same way.
- **Five languages, held by tests.** English, German, French, Italian and Hungarian, with each invoice printed in its client's language. Unit tests fail when a key used in the code is missing in any language or a translation drops a placeholder. Vitest covers the money paths in depth (VAT, totals, QR references, numbering, bank import), and a Playwright smoke test is wired into CI.

## Status

- **Launching soon.** The waitlist is open at [solory.ch](https://www.solory.ch).
- **Source code is private.** Solory is a commercial product, so this repository holds the case study and screenshots only.
- **Walkthrough on request.** Happy to show the architecture and the tradeoffs behind it in an interview.

## About

Designed, built and run by **Alex Földvári**, Frontend / Full-Stack Developer in Zürich, Switzerland.

[GitHub](https://github.com/foldvarialex) · [Portfolio](https://foldvari.ch/en) · [LinkedIn](https://www.linkedin.com/in/foldvarialex/) · [alex@foldvari.ch](mailto:alex@foldvari.ch)
