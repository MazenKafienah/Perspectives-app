# CLAUDE.md — Perspectives-app

This file is read by Claude Code when working in this repository. The full set of rules lives in [AGENTS.md](AGENTS.md) — read that file first; it applies to Claude Code exactly as it does to any other agent.

## Quick summary

- This repo owns the standalone Next.js App Router frontend only.
- It reads Supabase through the Data API using the **anon key only**. It never holds a service-role key.
- Worker code and canonical Supabase SQL migrations belong in `Perspectives-worker`, not here.
- No LLM calls from this codebase.
- Auth and Row Level Security are required from the start — the legacy prototype's `requiresAuth: false` is not carried forward.
- The legacy Base44 SDK and its client bootstrapping must not be reintroduced here.

## Current phase

This repository is in **MIG-000** (governance bootstrap) as of the commit that added this file. No frontend scaffolding, dependency installation, or production code has been written yet. Do not treat any prior planning-document draft of the frontend as already implemented — check the actual repository state before assuming code exists.
