# Perspectives-app

Standalone Next.js frontend for **PERSPECTIVES** — a medical opinion-intelligence platform that surfaces editorial, commentary, and perspective pieces from high-impact peer-reviewed journals, paired with AI-generated briefs and author bio cards.

This repository owns the **reader-facing web application only**. It does not own the ingestion pipeline, the canonical database migrations, or any secret beyond the public Supabase anon key.

## Scope

- Next.js **App Router** (TypeScript), reader-facing specialty feed, article, author, search, and settings pages.
- Server-rendered author pages for SEO.
- Deployed later on **Vercel**.
- Reads data exclusively through the **Supabase Data API** (PostgREST) using the **anon key**.
- Supabase Auth for sign-in/sign-up, with **Row Level Security** enforced on every user-scoped table from day one.

## Explicitly out of scope for this repository

- The Python ingestion/enrichment worker — that lives in [`Perspectives-worker`](https://github.com/MazenKafienah/Perspectives-worker).
- Canonical, executable Supabase SQL migrations — those are owned exclusively by `Perspectives-worker`. This repo must never become a second canonical location for production schema.
- Any LLM call. Summarisation, extraction, and enrichment happen server-side in the worker; the frontend never calls Anthropic or OpenAI directly.
- The Supabase **service-role key**. This repository only ever holds the public anon key. The service-role key belongs solely in the worker's environment.

## Environment variables

See [`.env.example`](.env.example). Only the public, anon-scoped Supabase values belong here:

```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

Never commit a populated `.env.local` or any file containing a real key or secret.

## Migrating off the legacy prototype

The legacy Base44 prototype ([`Perspectives_Prototype`](https://github.com/MazenKafienah/Perspectives_Prototype)) is a Vite 6 + React 18 SPA used only as a **behavioural specification** for this rebuild — not code to port line-by-line. When lifting UI or logic from it:

- The legacy `@base44/sdk` client and `base44Client.js` must **not** be carried into this application. All data access here goes through the Supabase client and the Data API.
- Base44-default dependency bloat (e.g. `three.js`, `leaflet`, `jspdf`, `html2canvas`, carousel/drag-and-drop/rich-text libraries) must not be copied across. Only add a dependency this application actually needs.
- shadcn/ui primitives should be reinstalled fresh (`npx shadcn-ui@latest add <component>`), not copied file-by-file.

## Status

This repository currently contains only MIG-000 governance and documentation. No frontend code, dependencies, or scaffolding has been created yet — that begins in a later, separately authorised phase.
