---
name: client-onboard
description: |
  Step-by-step checklist for onboarding a new Narrow Path client. Use when adding a new paying client or converting a prospect. Covers folder setup, Doppler project creation, Supabase profile, portal auth account, and Google Drive folder. Critical: company_name in Supabase must exactly match the Drive folder name.
allowed-tools:
  - Bash
  - Read
  - Write
---

# /client-onboard

Step-by-step onboarding for a new Narrow Path client. Complete every step in order.

## Before You Start

Confirm these three things with James before touching anything:

1. **Client name** — exact spelling and casing as it will appear in Google Drive and Supabase (case-sensitive, must match exactly)
2. **Billing tier** — `foundation` | `ascent` | `elevation`
3. **Contact email** — for their portal login

---

## Step 1 — Create client folder

```bash
mkdir -p /Users/theknightfamily/Projects/clients/[client-name]/assets
```

Create `clients/[client-name]/CLAUDE.md`:

```markdown
# [Client Name]

**Status:** Active
**Type:** [coaching brand / ministry / B2B / etc.]
**Tier:** [foundation / ascent / elevation]
**Contact:** [email]

## Brand

[1-2 sentence description of what they do and who they serve.]

## Deliverables

- [List deliverables: website, course, social content, etc.]
```

---

## Step 2 — Create Doppler project

Run in the NP BE workspace context (default unless you're inside `/personal`):

```bash
doppler projects create [client-name-kebab]
```

Add any client-specific secrets to the new project as they're identified (Metricool token, custom API keys, etc.).

---

## Step 3 — Add Supabase profile

In the `npbe-portal` Supabase project, insert into the `profiles` table:

```sql
INSERT INTO profiles (company_name, tier, role)
VALUES ('[Exact Client Name]', '[tier]', 'client');
```

**Critical:** `company_name` must be an exact character-for-character match with the Google Drive folder name created in Step 5. This is how portal social posts are matched to each client. Case-sensitive.

---

## Step 4 — Create portal auth account

In Supabase Auth, invite the client's email address. After the row is created in `auth.users`, confirm their `role` is set to `client` in the `profiles` table.

---

## Step 5 — Create Google Drive folder

Manually create a folder in Google Drive with the **exact** name used in Step 3. No extra spaces, no different capitalization. If the names don't match, their social posts will not appear in the portal.

---

## Step 6 — Update clients/CLAUDE.md

Add the client to the Active Clients table:

```
/Users/theknightfamily/Projects/clients/CLAUDE.md
```

---

## Step 7 — Confirm everything

- [ ] `clients/[client-name]/` folder created with CLAUDE.md and `assets/` subdirectory
- [ ] Doppler project created in NP BE workspace
- [ ] Supabase `profiles` row inserted — `company_name` matches Drive folder exactly
- [ ] Supabase auth account created — role = `client`
- [ ] Google Drive folder created — name matches Supabase `company_name` exactly
- [ ] `clients/CLAUDE.md` updated with new client
- [ ] Billing tier confirmed and set in Supabase

## See also

- `/brand-kit` — run a brand crawl after onboarding to build their brand kit
- `/pre-deploy` — checklist before going live with any client-facing work
