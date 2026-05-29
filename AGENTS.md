# Agent & Skill Index

Quick reference for the full system. For full instructions, read the individual files.

---

## Global Agents
`~/.claude/agents/` : auto-routed based on description matching

| Agent | Triggers | What it does |
|---|---|---|
| `brand-crawl` | "brand crawl", "build a brand kit", "research [client]'s brand", "synthesize this into the brand voice" | Crawls a URL, extracts brand elements, writes BRAND-IDENTITY.md + BRAND-VOICE.md, builds a Figma brand board. Also synthesizes podcasts/books/interviews into existing brand voice files. |
| `social-writer` | "write posts", "draft captions", "social content for [client]", "repurpose this episode" | Writes platform-native social copy. Reads BRAND-VOICE.md first. Repurposes long-form content into posts and Sheets-ready content plans. |
| `figma-designer` | "build in Figma", "create a brand board", "design [X]", "build social slides" | Brand boards, UI mockups, social templates, component libraries. Reads BRAND-IDENTITY.md first. |
| `web-dev` | "build a page", "build a site", "add a section", "create a page" | Website and web app builds. Follows Vibe Coding sequence. Reads project CLAUDE.md for brand voice before writing copy. |
| `competitive-analysis` | "analyze this competitor", "research [company]", "who are [client]'s competitors", "find competitors for [niche]" | Researches competitors. Discovery mode (find unknown competitors) or analysis mode (deep research on a specific company). |
| `research-director` | "run research", "check for updates", or runs automatically on schedule | Weekly system health check: AI models, stack security/updates, platform algorithms, workflow consolidation. Self-improves its own methods. |

---

## Project Agents
Scoped to a specific directory : only active when working inside that project

| Agent | Location | Triggers | What it does |
|---|---|---|---|
| `portal-dev` | `narrow-path/narrow-path-website/Narrow-Path-main/.claude/agents/` | "build", "add", "fix", "update" anything in the portal | Full-stack portal development: design tokens, Supabase patterns, route structure, auth, tier gating, component patterns |

---

## Skills (Slash Commands)
`~/.claude/skills/` : you invoke these deliberately with `/name`

| Skill | Command | What it does |
|---|---|---|
| Client Onboard | `/client-onboard` | Onboarding checklist for a new client: folder structure, brand files, scheduler config, Doppler, Supabase, portal auth, Google Drive |
| NP Portal Pre-Deploy | `/np-portal-pre-deploy` | NP portal only. Go-live checklist: Supabase migrations, Doppler secrets, Resend, Slack webhook, PWA assets, mobile testing, DNS |
| NP Portal Review | `/np-portal-review` | NP portal only. Pre-commit code review: Supabase patterns, auth routes, mobile, undo/safety, copy rules, TypeScript, secrets |
| UI Design | `/ui-design` | Design standards for all web/app interfaces: typography, color, spacing, motion, visual depth, components, accessibility, performance, current CSS |
| SAG Audit | `/sag-audit` | Full SEO + AEO + GEO audit on a client site: technical SEO, on-page, featured snippets, AI citation presence, content gaps |
| Brand Voice Update | `/brand-voice-update` | Synthesize new content (podcast, book, interview) into a client's BRAND-VOICE.md. Extracts signal only, never rewrites existing guidance. |
| Quick Commit | `/quick-commit` | Stage all changes, generate a Conventional Commits message from the diff, commit, and push. Never pushes to main. |

---

## Scheduled Routines
Remote agents running on a schedule : no local machine required

| Routine | Schedule | What it does | View |
|---|---|---|---|
| `research-director-weekly` | Every Monday, 8am ET | System health report: models, stack, platforms, security, workflow consolidation. Self-improves its research methods. | [routines](https://claude.ai/code/routines/trig_016s8qFbY86nkPaAvZrjFgE7) |

---

## Hooks
Deterministic rules that fire automatically : no AI judgment, no tokens spent deciding

| Hook | Trigger | What it does |
|---|---|---|
| SessionStart | Every session start | Echoes date, working directory, and git branch for context |
| PreToolUse | Write or Edit on any `.env` file | Blocks the operation. .env files are managed by Doppler only. |
| PostToolUse | Write or Edit on `.ts`, `.tsx`, `.md`, `.json` | Warns if an em dash is found in the written file. NP copy rules prohibit them. |

---

## When to use what

| Situation | Use |
|---|---|
| Delegate a project : research, writing, building, designing | **Agent** |
| Follow a standard procedure you always consciously trigger | **Skill** |
| Something that must always happen, no exceptions | **Hook** |
| Keep checking on something while this session is open | **Loop** (`/loop`) |
| Something that must run on a schedule unattended | **Routine** |

---

## Adding a New Agent

**Run the pre-build audit first:**
- Does an existing agent already handle this?
- Can the job be added as a new section in an existing agent?
- If new: what does it replace? Update or remove the old one.

If it's genuinely new:
1. Create `~/.claude/agents/[name].md`
2. Frontmatter: `name`, `description` (drives routing), `tools`
3. Open with task-execution language : what to DO, not who to BE
4. Add a row to the table above
5. Commit to `knight-creative/agent-research-system`

## Adding a New Skill

**Run the pre-build audit first:**
- Does an existing skill or agent already cover this?
- Is this a procedure you manually invoke, or work you delegate? (skill vs. agent)

If it's genuinely new:
1. Create `~/.claude/skills/[name]/SKILL.md`
2. Frontmatter: `name`, `description`, `allowed-tools`
3. Write as a step-by-step procedure
4. Add a row to the Skills table above

## Improving an Agent or Skill

After any session where something worked exceptionally well or failed:
1. Open the file
2. Add one rule, one example, or one clarification
3. Commit: `git commit -m "feat: [name] : [what you improved]"`
4. The improvement persists in every future session
