---
name: client-onboard
description: |
  Step-by-step checklist for onboarding a new Narrow Path client. Use when adding a new paying client or converting a prospect. Covers folder setup, brand files, scheduler config, Doppler, Supabase, portal auth, and Google Drive.
allowed-tools:
  - Bash
  - Read
  - Write
---

# /client-onboard

Step-by-step onboarding for a new Narrow Path client. Complete every step in order.

## Before You Start

Confirm with James before touching anything:

1. **Client name** — exact spelling and casing as it will appear in Google Drive and Supabase (case-sensitive, must match exactly)
2. **Billing tier** — `foundation` | `ascent` | `elevation`
3. **Contact email** — for their portal login
4. **Platforms** — which social platforms are included in their plan (for scheduler config)
5. **Status** — active (paying) or prospect (move from `clients/prospects/` if converting)

---

## Step 1 — Create client folder

```bash
mkdir -p /Users/theknightfamily/Projects/clients/active/[client-slug]/assets
mkdir -p /Users/theknightfamily/Projects/clients/active/[client-slug]/sources/podcast
mkdir -p /Users/theknightfamily/Projects/clients/active/[client-slug]/sources/books
mkdir -p /Users/theknightfamily/Projects/clients/active/[client-slug]/sources/interviews
```

If converting from a prospect, move the existing folder:
```bash
mv /Users/theknightfamily/Projects/clients/prospects/[client-slug] /Users/theknightfamily/Projects/clients/active/
```

---

## Step 2 — Create brand files

Copy and fill in the templates:

```bash
cp /Users/theknightfamily/Projects/clients/templates/BRAND-IDENTITY-template.md \
   /Users/theknightfamily/Projects/clients/active/[client-slug]/BRAND-IDENTITY.md

cp /Users/theknightfamily/Projects/clients/templates/BRAND-VOICE-template.md \
   /Users/theknightfamily/Projects/clients/active/[client-slug]/BRAND-VOICE.md

cp /Users/theknightfamily/Projects/clients/templates/CONTENT-INDEX-template.md \
   /Users/theknightfamily/Projects/clients/active/[client-slug]/CONTENT-INDEX.md
```

Fill in what's known from intake. Run `/brand-crawl` after onboarding to complete BRAND-IDENTITY.md and BRAND-VOICE.md from their website.

**Critical:** Add the Hashtag Strategy section to BRAND-VOICE.md before running the scheduler. The scheduler reads this file directly — no separate brief needed.

---

## Step 3 — Create CLAUDE.md

Create `clients/active/[client-slug]/CLAUDE.md`:

```markdown
# [Client Name]

**Status:** Active
**Tier:** [foundation / ascent / elevation]
**Contact:** [email]
**Type:** [coaching brand / ministry / B2B / etc.]

## What They Do

[1-2 sentence description.]

## Deliverables

- [List: website, social content, course, etc.]

## Files

- `BRAND-IDENTITY.md` — colors, fonts, logos, visual system
- `BRAND-VOICE.md` — voice, mission, pillars, audience, hashtags
- `CONTENT-INDEX.md` — source material tracker
- `sources/` — podcast transcripts, book excerpts, interviews
```

---

## Step 4 — Add to scheduler

Add the client to `clients.json` in the social media scheduler:

```bash
# Edit:
/Users/theknightfamily/Projects/narrow-path/social-media-scheduler/clients.json
```

Add entry:
```json
"[Exact Client Name as in Drive]": {
  "blogId": [Metricool blog ID],
  "industry": "[personal-brand / b2b / b2c / ministry]",
  "initials": "[XX]",
  "tier": "[foundation / ascent / elevation]",
  "platforms": ["instagram", "facebook", "linkedin"]
}
```

The scheduler reads `BRAND-VOICE.md` directly — no `brief.md` needed.

---

## Step 5 — Doppler project

```bash
doppler projects create [client-name-kebab]
```

---

## Step 6 — Supabase profile

In the `npbe-portal` Supabase project:

```sql
INSERT INTO profiles (company_name, tier, role)
VALUES ('[Exact Client Name]', '[tier]', 'client');
```

**Critical:** `company_name` must be character-for-character identical to the Google Drive folder name. Case-sensitive. This is how portal social posts match to each client.

---

## Step 7 — Portal auth account

In Supabase Auth, invite the client's email. After the row appears in `auth.users`, confirm `role = 'client'` in `profiles`.

---

## Step 8 — Google Drive folder

Create the folder in Google Drive with the **exact** name from Step 6. No extra spaces, no different capitalization.

---

## Step 9 — Update clients/CLAUDE.md

Add the client to the Active Clients section:
```
/Users/theknightfamily/Projects/clients/CLAUDE.md
```

---

## Final Checklist

- [ ] `clients/active/[client-slug]/` folder with assets/ and sources/ subfolders
- [ ] `BRAND-IDENTITY.md` created (fill with brand crawl data)
- [ ] `BRAND-VOICE.md` created with Hashtag Strategy section
- [ ] `CONTENT-INDEX.md` created
- [ ] `CLAUDE.md` created
- [ ] Added to scheduler `clients.json` with correct platforms and tier
- [ ] Doppler project created
- [ ] Supabase `profiles` row inserted — company_name matches Drive folder exactly
- [ ] Supabase auth account created — role = client
- [ ] Google Drive folder created — name matches Supabase exactly
- [ ] `clients/CLAUDE.md` updated

## Next steps after onboarding

- `/brand-crawl [url]` — complete BRAND-IDENTITY.md and BRAND-VOICE.md from their site
- `npm run plan` — generate first month of content once brand files are complete
