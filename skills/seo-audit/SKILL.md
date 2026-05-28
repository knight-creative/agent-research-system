---
name: seo-audit
description: Run a full SEO / AEO / GEO audit on a client site. Covers technical SEO, on-page quality, featured snippet opportunities, AI citation presence, and content gaps. Invoke with a URL and client name. Saves a dated audit report to the client folder.
allowed-tools:
  - Bash
  - Read
  - Write
---

# /seo-audit

Full search visibility audit across three disciplines: SEO (ranking), AEO (answer engines), and GEO (AI citations). Run this deliberately on a client site when an audit is needed.

## Before starting — confirm you have

- Site URL
- Client name (for saving the report)
- Client's 3-5 primary services or topics
- Google Search Console access (ask the client — transforms guesswork into fact)

---

## Phase 1 — Technical SEO

```bash
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl scrape "<url>" -o .firecrawl/home.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl scrape "<url>/robots.txt" -o .firecrawl/robots.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl crawl "<url>" --max-depth 2 --limit 20 --wait --progress -o .firecrawl/site-crawl.json

# Core Web Vitals — free, no key
curl "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<url>&strategy=mobile" -o .firecrawl/pagespeed-mobile.json
curl "https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<url>&strategy=desktop" -o .firecrawl/pagespeed-desktop.json
```

Check:
- LCP < 2.5s, INP < 200ms, CLS < 0.1, mobile score > 90
- Title tags: present, unique, 50-60 chars, keyword included
- Meta descriptions: present, unique, 140-160 chars
- H1: one per page, contains primary keyword
- robots.txt: nothing important accidentally blocked
- Sitemap: present and current
- HTTPS confirmed, internal links to key pages

---

## Phase 2 — On-Page SEO

For each key service or pillar page:

```bash
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl scrape "<url>/[page]" -o .firecrawl/[page].json
```

Check per page:
- One clear primary keyword in title, H1, and first 100 words
- Content fully answers searcher intent
- E-E-A-T signals: author info, credentials, dates, citations
- Schema markup present and valid
- Image alt text descriptive and keyword-informed

---

## Phase 3 — AEO (Answer Engine Optimization)

```bash
YEAR=$(date +%Y)
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl search "[primary service] [niche]" --limit 5 -o .firecrawl/serp-1.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl search "how to [primary service outcome]" --limit 5 -o .firecrawl/serp-faq.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl search "best [service type] for [audience]" --limit 5 -o .firecrawl/serp-best.json
```

Check:
- Featured snippets: who holds them for the client's target queries?
- Does the client's content have the format to win them? (question in H2, direct 40-60 word answer immediately after)
- People Also Ask: what questions appear? Are they answered on the site?
- FAQ schema implemented on relevant pages?
- Google Business Profile complete?

**Featured snippet formula:** H2/H3 = the question. First paragraph = direct answer in 40-60 words. Then supporting depth.

---

## Phase 4 — GEO (Generative Engine Optimization)

Check if the client appears in AI-generated answers:

```bash
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl scrape "https://www.perplexity.ai/search?q=[primary+service+query]" -o .firecrawl/perplexity-1.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl scrape "https://www.perplexity.ai/search?q=[brand+name]" -o .firecrawl/perplexity-brand.json
```

LLMs cite content that is:
- Specific and factual (not vague or hedged)
- Well-structured (H2/H3/H4, bullet lists, tables)
- Attached to named entities (real people, organizations, places)
- Original: named frameworks, proprietary data, unique methodologies
- Authoritative: backlinks, credentials, consistent NAP across the web

Check: Is the brand mentioned in AI answers? Does the About page clearly state who they are, what they do, who they serve, where they're based? Any original citable claims?

---

## Phase 5 — Content Gaps

```bash
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl search "[top competitor] [niche] content" --limit 5 -o .firecrawl/competitor-content.json
FIRECRAWL_API_KEY="fc-b1907a948ee649b2b219ddc7ec74a6db" /Users/theknightfamily/.npm-global/bin/firecrawl search "[niche] FAQ site:reddit.com OR site:quora.com" --limit 5 -o .firecrawl/community-questions.json
```

Surface: topics worth creating, questions worth answering, formats worth adding.

---

## Save the report

Save to: `clients/[active|prospects]/[client-name]/seo-audit-[YYYY-MM-DD].md`

Report structure:
- Executive summary (most important finding + single highest-impact action)
- Overall score table (Technical / On-Page / AEO / GEO: Green/Yellow/Red)
- Technical SEO: Core Web Vitals table, critical issues, prioritized fixes
- On-Page: page-by-page summary
- AEO: featured snippet opportunities, FAQ/PAA gaps, schema audit
- GEO: AI citation status, what's blocking citation, action items
- Content gaps: topics to create, questions to answer
- Priority action plan (stack-ranked, effort vs. impact)
- Paid tool upgrade path (only if tooling limited the audit)

---

## Rules

- Ask about Google Search Console first — it's free and changes everything
- Technical issues before content — great content on a slow site is wasted work
- GEO is the highest-leverage gap for most clients right now — prioritize it
- One dated file per audit — never overwrite a previous one
- If tooling limited the audit, say so explicitly — don't fill gaps with guesses
