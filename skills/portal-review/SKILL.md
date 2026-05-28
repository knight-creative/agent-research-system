---
name: portal-review
description: Run a pre-commit code review on portal changes. Checks Supabase usage, server/client component boundaries, auth protection, mobile compatibility, undo/safety mechanisms, copy rules, TypeScript, secrets, and database patterns. Invoke before committing any portal work.
allowed-tools:
  - Read
  - Bash
---

# /portal-review

Pre-commit checklist for the Narrow Path portal. Run this before committing any portal code change.

## Step 1 — Get the diff

```bash
cd /Users/theknightfamily/Projects/narrow-path/narrow-path-website/Narrow-Path-main

# Staged changes
git diff --staged

# All uncommitted changes
git diff HEAD

# Changes since branching from main
git diff main...HEAD
```

If specific files were mentioned, read those directly.

---

## Step 2 — Run through the checklist

Work through each category. Report only what fails — if a category is clean, skip it.

### Supabase Client Usage
- `lib/supabase/server.ts` — server components only
- `lib/supabase/client.ts` — browser components only
- `lib/supabase/admin.ts` — API routes only. Bypasses RLS entirely.
  - **BLOCK** admin client used outside `/app/api/`
  - **BLOCK** admin client in server or browser components

### Server vs Client Components
- Default is server. `'use client'` only when interactivity is required.
- **FLAG** client components fetching data directly from Supabase
- **FLAG** `'use client'` that could be removed by lifting data to a parent server component

### Auth and Route Protection
- `/portal/*` — role = 'client' only
- `/admin/*` — role = 'admin' only
- **BLOCK** any protected route missing auth check

### Mobile Compatibility
- Changes must work on iOS Safari, iOS Chrome (WebKit), and Android Chrome
- **BLOCK** keyboard detection using `visualViewport` math — use focus/blur events
- **FLAG** `vh` or `dvh` viewport units in contexts that break when mobile keyboard is open

### Undo / Safety
- Any feature that publishes, sends, or changes client-visible state needs a reversal mechanism
- **BLOCK** publish/send/change with no way to undo it

### Copy and Language
- **FLAG** em dashes (`—`) in user-facing strings
- **FLAG** fear-based, negative, or comparative client-facing language
- **FLAG** headlines phrased as questions

### TypeScript
- **FLAG** `any` type without an inline comment
- **FLAG** type assertions (`as X`) that look like they're papering over a real mismatch

### Secrets and Environment
- **BLOCK** hardcoded credentials, API keys, or secrets
- **BLOCK** `.env` files being committed
- **FLAG** environment variables not managed through Doppler

### Database Changes
- **BLOCK** schema changes not represented by a migration in `supabase/migrations/`
- **FLAG** queries that may bypass RLS unintentionally

---

## Step 3 — Report

Use this format:

- **BLOCK** — must fix before merging (security, data loss, production breakage, costly pattern violation)
- **FLAG** — pattern violation, needs attention before shipping
- **NOTE** — minor, low-priority observation

If nothing is wrong, say so clearly and move on. Don't pad the report.
