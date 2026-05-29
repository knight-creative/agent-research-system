---
name: np-portal-pre-deploy
description: |
  NP Portal only. Go-live checklist before any Vercel deploy of the Narrow Path Brand Elevation portal. Covers pending Supabase migrations, Doppler secrets, Resend domain verification, Slack Event Subscriptions URL, PWA assets, and mobile testing. Never skip this before pushing to production.
allowed-tools:
  - Bash
  - Read
---

# /np-portal-pre-deploy

Go-live checklist for the Narrow Path portal. NP portal specific. Every item must be confirmed before deploying to production.

**Portal location:** `/Users/theknightfamily/Projects/narrow-path/narrow-path-website/Narrow-Path-main/`

---

## Database

- [ ] Pending migrations applied: `015`, `016`, `024`

```bash
cd /Users/theknightfamily/Projects/narrow-path/narrow-path-website/Narrow-Path-main
supabase db push
```

Confirm each migration completed without errors. If migrations are applied manually, check `supabase/migrations/` for the correct files.

---

## Secrets (Doppler: `npbe-portal` project)

All of these must be set before deploying. Verify with:

```bash
doppler secrets --only-names --project npbe-portal
```

- [ ] `SLACK_TEAM_CHANNEL_ID` set
- [ ] `SLACK_PODCAST_PRODUCER_ID` set
- [ ] `RESEND_FROM_ADDRESS` set to a verified `npbrandelevation.com` address
- [ ] Google Service Account credentials wired (replace stubs in `lib/google-calendar.ts`)

Confirm Doppler is synced to Vercel before the deploy.

---

## Resend

- [ ] Domain `npbrandelevation.com` verified in the Resend dashboard
- [ ] `RESEND_FROM_ADDRESS` uses that verified domain

---

## Slack

- [ ] Slack Event Subscriptions URL updated from ngrok to the production Vercel URL
  - Go to api.slack.com, open your app, go to Event Subscriptions
  - Set Request URL to: `https://npbrandelevation.com/api/slack/events`
  - Confirm the URL verifies successfully

---

## PWA Assets

- [ ] `icon-192.png` present in `/public/`
- [ ] `icon-512.png` present in `/public/`

```bash
ls /Users/theknightfamily/Projects/narrow-path/narrow-path-website/Narrow-Path-main/public/ | grep icon
```

---

## Mobile Testing

Test on all three before shipping:

- [ ] iOS Safari
- [ ] iOS Chrome (WebKit)
- [ ] Android Chrome

Flows to test on each:
- Login and logout
- Portal dashboard loads without errors
- Messages thread: keyboard opens without breaking layout
- Push notification prompt appears (if enabled for the tier)
- Any feature changed in this deploy: test the full flow

---

## DNS and Hosting

- [ ] Cloudflare DNS: `npbrandelevation.com` points to Vercel
- [ ] Vercel project connected to `knight-creative/narrow-path-website`
- [ ] Vercel environment variables match Doppler (no stale or missing values)

---

## Final Gate

- [ ] All items above are checked
- [ ] Current branch is NOT `main` - confirm you are on a release branch

```bash
git branch --show-current
```

- [ ] James has explicitly approved the deploy

**Never merge to `main` or trigger a production deploy without explicit confirmation.**

## See also

- `/client-onboard` - checklist for adding a new client before they access the portal
