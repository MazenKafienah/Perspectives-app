# AGENTS.md — Perspectives-app

Instructions for any AI coding agent (Claude Code, or otherwise) working in this repository.

## What this repository is

The standalone Next.js (App Router) frontend for PERSPECTIVES. Reader-facing only: specialty feed, article detail, author profile, search, settings, auth.

## Hard boundaries — do not cross these

- **No worker code here.** The Python ingestion/enrichment pipeline lives in `Perspectives-worker`. Do not add Python files, cron jobs, or pipeline logic to this repo.
- **No canonical SQL migrations here.** Executable Supabase migrations belong only in `Perspectives-worker/supabase/migrations/`. If you need a schema change, it happens there, not here.
- **No service-role key, ever.** This repo only ever uses the Supabase **anon** key, and only via `NEXT_PUBLIC_SUPABASE_ANON_KEY`. Never place a service-role key in a frontend file, a `NEXT_PUBLIC_` variable, a commit, or a log.
- **No LLM calls from the frontend.** Summarisation and enrichment happen server-side in the worker. This app only reads already-persisted data from Supabase.
- **No Base44 SDK.** Do not import `@base44/sdk` or reintroduce `src/api/base44Client.js`-style code. All data access goes through the Supabase client.
- **Auth and RLS are mandatory.** The legacy prototype ran with `requiresAuth: false`. This rebuild requires Supabase Auth and Row Level Security from the first commit of real code — do not ship an unauthenticated data path for user-scoped tables.

## When porting from the legacy prototype

`Perspectives_Prototype` is a **read-only specification**, not a source to copy wholesale:

- Reinstall shadcn/ui primitives fresh rather than copying `src/components/ui/`.
- Drop unused Base44-default dependencies (three.js, leaflet, jspdf, html2canvas, embla-carousel, dnd-kit/hello-pangea, quill, vaul, canvas-confetti, react-day-picker, etc.) unless a specific PERSPECTIVES feature actually needs them.
- Rewrite the data layer (`base44Client.js`, `AuthContext.jsx`, `usePreferences.js`, etc.) against Supabase; do not attempt to shim the Base44 SDK.

## Current status

MIG-000 governance only. No application code exists yet. Do not scaffold Next.js, install dependencies, or write production code until a separately authorised phase explicitly says to.
