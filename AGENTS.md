# Agent & Skill Index

Quick reference for the full agent system. For full instructions, read the individual files.

---

## Global Agents
`~/.claude/agents/` — available in every session

| Agent | Trigger phrases | What it does |
|---|---|---|
| `brand-crawl` | "brand crawl", "build a brand kit", "research [client]'s brand" | Crawls a client URL with Firecrawl, extracts colors/typography/voice/assets, writes BRAND-KIT.md, builds a Figma brand board |
| `social-writer` | "write posts", "draft captions", "social content for [client]" | Writes platform-native social media copy. Reads client brief.md and BRAND-KIT.md before writing. Knows all platform specs and copywriting frameworks |
| `figma-designer` | "build in Figma", "create a brand board", "design [X]", "social slides" | Brand boards, UI mockups, component libraries, social media templates. Knows all platform dimensions and NP design tokens |
| `web-dev` | "build a page", "build a site", "add a section", "design a landing page" | Full website builds following the Vibe Coding sequence. Design system standards, SEO, performance, security, accessibility, pre-launch gate |
| `portal-reviewer` | "review my portal changes", "check this code", "does this look right" | Reviews portal code against established patterns: Supabase client usage, server vs client components, mobile rules, undo/safety, copy rules |
| `research-director` | "run research", "check for updates", manually or weekly schedule | Weekly system health check: AI model releases, stack versions, platform algorithm changes, security advisories, workflow consolidation opportunities. Runs automatically every Monday at 8am ET |
| `competitive-analysis` | "analyze this competitor", "research [company]", "run a comp analysis", "what is [company] doing" | Scrapes a competitor's website, social profiles, Meta Ad Library, content, and job postings. Produces structured report with gap analysis. Three modes: quick / standard / deep |
| `seo-aeo-geo` | "audit this site for SEO", "check our GEO presence", "are we showing up in AI answers", "optimize for answer engines", "run an SEO audit for [client]", "what keywords should [client] target" | Dual-mode: audits a site (technical SEO, on-page, AEO, GEO, content gaps) and maintains standing knowledge on what's working. Works from public data; flags where paid tools (GSC, Ahrefs) would unlock more |

---

## Project Agents
Scoped to a specific directory — only active when working inside that project

| Agent | Location | Trigger phrases | What it does |
|---|---|---|---|
| `portal-dev` | `narrow-path/narrow-path-website/Narrow-Path-main/.claude/agents/` | "build", "add", "fix", "update" anything in the portal | Full-stack portal development. Knows the complete codebase: design tokens, Supabase patterns, route structure, auth gates, tier gating, component patterns, pre-commit quality gate |

---

## Skills (Slash Commands)
`~/.claude/skills/` — invoke with `/skill-name`

| Skill | Command | What it does |
|---|---|---|
| Client Onboard | `/client-onboard` | Step-by-step checklist for adding a new NP client: folder setup, Doppler project, Supabase profile, portal auth account, Google Drive folder |
| Pre-Deploy | `/pre-deploy` | Portal go-live checklist: migrations, Doppler secrets, Resend verification, Slack URLs, PWA assets, mobile testing, DNS |
| Frontend Design | `/frontend-design` | Guidance for distinctive, production-grade web interfaces: aesthetic direction, typography, motion, visual depth |

---

## Scheduled Routines
Remote agents running on a schedule — no local machine required

| Routine | Schedule | What it does | View results |
|---|---|---|---|
| `research-director-weekly` | Every Monday, 8am ET | Full system research report: models, stack, platforms, security, workflow consolidation, new tools | [claude.ai/code/routines](https://claude.ai/code/routines/trig_016s8qFbY86nkPaAvZrjFgE7) |

---

## Adding a New Agent

**Before creating — run the pre-build audit:**
- Does an existing agent already handle this? Read the table above.
- Can the job be added as a new workflow section in an existing agent?
- If a new agent is genuinely needed, does it replace something? Update or remove the old one.

If it's truly new:
1. Create `~/.claude/agents/[name].md` (global) or `.claude/agents/[name].md` (project-scoped)
2. Add frontmatter: `name`, `description` (this drives automatic routing), `tools`
3. Write the system prompt — be specific, add examples, include a quality gate
4. Add a row to the Global Agents table above
5. Commit to `knight-creative/agent-research-system` so it's versioned

## Adding a New Skill

**Before creating — run the pre-build audit:**
- Does an existing agent or skill already cover this workflow?
- Is this genuinely a skill (a guided human-invoked workflow) or should it be an agent?

If it's truly new:
1. Create `~/.claude/skills/[name]/SKILL.md`
2. Add frontmatter: `name`, `description`, `allowed-tools`
3. Write the workflow — step by step
4. Invoke with `/name` in any session

## Training an Agent (making it better)

After any session where an agent did something wrong or exceptionally well:
1. Open the agent file
2. Add one rule, one example, or one clarification
3. Commit: `git commit -m "feat: [agent-name] — [what you improved]"`
4. The improvement persists in every future session
