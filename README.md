# JARVIS

**A private, single-user AI OS for macOS — voice, screen, camera and second-brain, with safety boundaries built in from day one.**

JARVIS is a personal AI operating layer I built for my own Mac. A web dashboard shows calendar and email context, automation status and a task queue; a local Electron app adds voice, screen and camera awareness, a second-brain memory and application control. Queued tasks are executed by a local bridge through Claude, with a local Ollama fallback.

> Screenshots are intentionally omitted: the real interface displays personal calendar and email data.

## Architecture

```text
Google OAuth ───────┐
Gmail / Calendar ───┤
                    ▼
              Next.js web app
              (Auth.js, server APIs)
                    │
                    ▼
              Supabase Postgres
       tasks · heartbeat · workers · audit
          │                         │
          │                         └── Telegram webhook / pairing
          ▼
   macOS local bridge                 VPS worker (optional)
   launchd poller                     signed HTTPS adapter
          │
          ├── Claude CLI / Agent SDK
          └── Ollama (local fallback)

   Electron JARVIS app
   voice · wake word · screen · camera · memory · Mac control
```

## What it does

- **Dashboard:** calendar, emails that need attention, automation status, meeting prep and search in one view.
- **Task queue:** free-form AI tasks are queued in Postgres and executed by a local launchd bridge; idempotent, with lease-based recovery of stuck tasks.
- **Desktop app (Electron):** wake word and voice conversation, screen and camera understanding, second-brain memory, local application control.
- **Remote control:** paired Telegram access and an optional VPS worker that accepts only strictly scoped, signed commands.

## Design principles

- The browser never receives the Supabase service key, OAuth refresh tokens, pairing codes or worker secrets.
- A queued task is data, not a higher-priority instruction — file, email, web and Telegram content is treated as untrusted.
- Unknown tools and capabilities are denied by default.
- Consequential actions (sending, buying, deleting, publishing, submitting) are never silently enabled in the background bridge.
- Status is honest: `offline`, `not_configured` and `error` are never shown as success.

## Stack

TypeScript · Next.js (App Router) · Auth.js · Supabase / Postgres (RLS, atomic `security definer` functions) · Zod · Electron · LiveKit voice · Claude Agent SDK · Ollama · launchd · Vitest

## O projektu

JARVIS je privatni AI OS za moj Mac: dashboard sa kalendarom i mejlom, red AI zadataka koje izvršava lokalni bridge, i Electron aplikacija sa glasom, ekranom, kamerom i „drugim mozgom“. Poseban fokus je bezbednost: nepoznati alati su zabranjeni, sadržaj mejlova i weba je nepouzdan, a posledične akcije se nikad ne uključuju tiho.

## About this repository

JARVIS is a personal project. This public repository is a showcase containing a description only — it does **not** contain source code, credentials, personal data or build files.
