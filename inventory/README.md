# Scott Inventory App

Inventory management app for Scott Lumber Packaging. The entire app is one
self-contained file: [`index.html`](index.html).

> This is **separate** from the marketing website documented in the repo-root
> [`../README.md`](../README.md). They share a repo but are unrelated apps.

## Architecture

- **One file, no build step.** `index.html` holds all markup, styling
  (Tailwind via CDN), and React. JSX is transpiled **in the browser** by Babel
  standalone — there is no bundler, no `package.json`, no `dist/`. Run it by
  opening the file in a browser (or the editor's preview).
- **Backend:** Supabase (Postgres + auth). The client is created from a
  hardcoded project URL + anon key near the top of the file.
- **No local toolchain.** There is no Node/npm on the dev machine, so the file
  cannot be transpiled, linted, or bundled locally. Verify changes by loading
  the page in a browser/preview or by careful manual review — a syntax error
  silently breaks the whole page since Babel runs at load time.

## Database

Tables: `items`, `transactions`, `transaction_audits`, `pending_orders`,
`form_submissions`.

- **Schema is managed by SQL migrations run manually in the Supabase SQL editor
  — not from this repo.** The `SETUP_SQL` constant in `index.html` is a
  reference/setup snippet shown if tables are missing; **do not edit it to
  change schema** and do not rely on it to add columns/tables.
- Item references are UUID `item_id` foreign keys everywhere.
- `transactions.quantity` sign convention: **negative = usage, positive = receipt**.
- Audit triggers on `transactions` fire once per row automatically
  (insert/edit/delete). Expected — don't work around them.
- `pending_orders.status` has a CHECK constraint allowing only
  `'pending' | 'received' | 'cancelled'`. Never write any other value (e.g. the
  receive "orphan" recovery state is React-only, never persisted).

## Key conventions

- `calcMetrics(items, transactions, pendingOrders = [], asOfDate = ...)` is the
  shared metrics engine used by Dashboard, PO Guidance, Reports, and Export. It
  nets pending ("on order") quantities out of the reorder suggestion, so
  changing its math affects all four pages. Callers that don't pass
  `pendingOrders` get on-order = 0.
- Surgical edits only; match surrounding style. Show a diff and get approval
  before committing/pushing.

## Features

- **Multi-line transaction entry:** one entry writes multiple `transactions`
  rows that share a Date + PO #.
- **Pending orders:** the **Orders** page logs placed orders (shown as "On
  Order"). Receiving one writes a positive `transactions` row and flips the
  order to `received` (with a back-link via `transaction_id`); cancelling marks
  it `cancelled` with no transaction. On-order reduces reorder suggestions
  across Dashboard, PO Guidance, Reports, and Export.

## Deploy

The repo auto-deploys to Netlify on push to `main` (see the root README), so
committing and pushing `inventory/index.html` publishes it. There is no separate
build or release step.
