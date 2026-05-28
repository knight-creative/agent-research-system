---
name: voice-synthesis
description: Synthesize new content (podcast transcripts, book excerpts, interviews, social archives) into a client's BRAND-VOICE.md. Extracts signal only: signature phrases, new stories, evolved positioning, new themes. Never rewrites existing voice guidance. Invoke when new content arrives that should inform how we write for a client.
allowed-tools:
  - Read
  - Write
  - Bash
---

# /voice-synthesis

Synthesize new content into a client's BRAND-VOICE.md. This is how brand knowledge grows over time without bloating the voice file.

**Invoke when:** a client produces new content (podcast episode, book chapter, interview, social post archive) that should inform how we write for them.

**Rule:** Extract signal, never bulk-copy. The source file stays in `sources/`. The voice file stays lean.

---

## Before starting

Confirm:
- Client name (to find the right files)
- Source material (file path or pasted content)
- Type of content (podcast / book / interview / social / other)

---

## Step 1: Read context

```
Read: clients/active/[client-name]/BRAND-VOICE.md
Read: clients/active/[client-name]/CONTENT-INDEX.md
```

Understand what voice signal is already captured so you only extract what's genuinely new.

---

## Step 2: Save the source

If the source is pasted content or a new file, save it to the appropriate subfolder:

```
clients/active/[client-name]/sources/podcast/[episode-slug].md
clients/active/[client-name]/sources/books/[chapter-slug].md
clients/active/[client-name]/sources/interviews/[interview-slug].md
```

---

## Step 3: Extract new signal

Scan the source for patterns worth adding to BRAND-VOICE.md. Look for:

**Signature phrases**: exact words or phrases the client uses repeatedly that aren't already in the voice file. These are the phrases that make their voice distinctly theirs.

**Named frameworks or systems**: any process, methodology, or system they refer to by name. These are AEO gold: AI engines cite named systems.

**Storytelling anchors**: specific personal stories or anecdotes with a clear setup and lesson. Named stories ("The grocery clerk moment", "The Duck Dynasty deal") are repeatable content assets.

**Vocabulary patterns**: words they always use, words they never use. What's in their vocabulary and what's conspicuously absent.

**Audience framing**: how they describe the people they serve. Exact language matters here.

**Evolved positioning**: if they describe themselves or their work differently than what's in the voice file, note it. Brands evolve.

**Do NOT add:**
- Full transcripts or long passages (those stay in sources/)
- One-off phrases that only appeared once
- Content that contradicts existing voice guidance without clear evidence of a genuine shift

---

## Step 4: Update BRAND-VOICE.md

Append new signal to the relevant sections. Never rewrite existing content.

For new signature phrases, add to the Signature Phrases section (create it if it doesn't exist).

For new stories, add a Storytelling Anchors section.

For vocabulary patterns, add a Voice Vocabulary section.

If the brand has genuinely evolved (new positioning, dropped messaging), note it explicitly:

```
**Positioning update [YYYY-MM-DD]:** Previously described as [X]. Now consistently framing as [Y] across [source].
```

---

## Step 5: Update CONTENT-INDEX.md

Mark the source as synthesized:

```
| [Source title] | [filepath] | [what was extracted] | Yes: [YYYY-MM-DD] |
```

Add a synthesis note at the bottom:

```
- [YYYY-MM-DD]: Synthesized [source]. Added: [list of what was added to BRAND-VOICE.md].
```

---

## Step 6: Check scheduler brief

If this client has content running through the scheduler, check whether the extracted signal should also update the scheduler's brand knowledge. The scheduler reads BRAND-VOICE.md directly now, so no separate update is needed unless the posting strategy or hashtags changed.

---

## What good synthesis looks like

**Source:** Jon Alwinson podcast episode on follow-up strategy

**Extracted:**
- Signature phrase: "Most deals don't die. They're abandoned." (appears 3x)
- Signature phrase: "60% of customers say no four times before saying yes"
- Story: "The Arks Outdoors deal" (Duck Dynasty connection, failure became fuel)
- Vocabulary: "attack list", "squeeze", "territory" (sales as hunting metaphors)
- Audience framing: "the person who's tired of hearing just work harder"

**Not extracted:** 5-minute tangent about his dog. One-time reference to a client name. Full transcript of the intro.
