---
name: kb-ingest
description: >
  Knowledge base ingestion skill. Use this whenever the user wants to process new
  source material into their knowledge base — phrases like "ingest this", "process
  new files", "I added some articles to raw/", "scan for new sources", or "what's
  unprocessed in raw/" should all trigger this skill. Also triggers when the user
  drops files into raw/ and wants them ready for wiki compilation. This is always
  the first step before kb-compile.
---

# KB Ingest

You are scanning the `raw/` directory for new or unprocessed source documents and
preparing them for wiki compilation. Your job is to read and understand the source
material, extract what matters, and write a structured ingest report.

## Companion notes

Sources in `raw/` can have an optional companion note file — same filename with
`.note.md` appended (e.g. `paper.pdf` → `paper.pdf.note.md`). These notes capture
the researcher's context: why they found the source interesting, what question they
were trying to answer, where they found it.

**Before ingesting**, check whether the user included any context in their message
(e.g. "ingest this, I found it while reading about X"). If they did, write a
companion note file into `raw/` before proceeding:

```
raw/source-filename.note.md
```

Use this format for generated notes:

```
Found via: [how they came across it, from their message]
Context: [what they were thinking or researching]
Questions: [what they're hoping it answers]
Date added: YYYY-MM-DD
```

Only generate a note if the user actually provided context. If they said nothing
beyond "ingest this", skip the note — don't fabricate context.

If a `.note.md` already exists for a file (written by the user themselves),
use it as-is without overwriting it.

## What "unprocessed" means

A file in `raw/` is unprocessed if it doesn't appear in `index/ingest-log.md`.
Companion `.note.md` files are never treated as source files to ingest — they are
metadata and should be skipped when scanning for new sources.

If `index/ingest-log.md` doesn't exist yet, treat all non-note files as unprocessed.

## Steps

### 1. Check what's already been processed
Read `index/ingest-log.md` if it exists. Note which files have already been
ingested so you skip them.

### 2. Handle any user-provided context
If the user's message contains context about why they're adding the source,
write the companion `.note.md` file now, before ingesting.

### 3. Scan raw/ for new files
List all files in `raw/` (recursively). Identify those not yet in the ingest log.
Skip any `.note.md` files — these are companions, not sources.
Common source types: `.md`, `.txt`, `.pdf`, `.html`, `.png`, `.jpg`.

### 4. For each unprocessed file, extract:
- **Title / identifier** — what is this source?
- **Source type** — article, paper, repo README, dataset description, image, etc.
- **Core topic(s)** — what subject(s) does it cover? (be specific, not generic)
- **Key concepts** — the 3–8 most important concepts, entities, or ideas it introduces
- **Key claims or findings** — what does it actually say? The substance.
- **Researcher's context** — if a companion `.note.md` exists, include its content
  here verbatim. This is the human's voice and should be preserved, not paraphrased.
- **Suggested wiki articles** — which existing wiki articles should this enrich?
  Which new articles might it warrant?
- **Quality / reliability notes** — is the source dated, opinionated, primary vs
  secondary? Any caveats worth noting?

For image files: describe what they depict and what concepts they illustrate.
For code/repo files: focus on what the code does and what concepts it demonstrates.

### 5. Write the ingest report

Save to `index/ingest-report-YYYY-MM-DD-HHMM.md` (use today's date and current time,
e.g. `ingest-report-2026-04-05-1430.md`). This ensures multiple ingests on the
same day don't overwrite each other. Use this format:

```
# Ingest Report — YYYY-MM-DD HH:MM

## Summary
N new files processed. Key topics: [brief list].

---

## [filename]
- **Type**: article / paper / image / etc.
- **Topics**:
- **Key concepts**:
- **Key claims**:
- **Researcher's context**: [from .note.md if present, otherwise omit this field]
- **Suggested wiki articles**:
  - New: [concept-name] — reason
  - Enrich: [existing-article] — what to add
- **Notes**:
```

### 6. Update the ingest log

Append each newly processed file to `index/ingest-log.md`:

```
| raw/filename | YYYY-MM-DD | [topic] | ready-for-compile |
```

Create the file with a header row if it doesn't exist:
```
| File | Ingested | Topic | Status |
|------|----------|-------|--------|
```

### 7. Tell the user what you found

Summarize: how many files processed, what the main topics were, and what wiki
articles you're recommending for creation or enrichment. If you generated any
companion notes, mention them so the user knows their context was captured.

Then tell them clearly: the ingest report is saved and ready, but the wiki has
not been changed yet. To turn this into wiki articles, they should run kb-compile
— either as a separate task or by chaining it in one prompt next time
(e.g. "ingest new files and compile the wiki").

## What good ingest looks like

A good ingest report is specific and opinionated. Don't just summarize — make
concrete recommendations. "This should create a new article on transformer
attention mechanisms, and enrich the existing neural-networks.md with the
benchmarking data from section 3" is useful. "This covers AI topics" is not.

The researcher's context, when present, is gold — it tells you not just what the
source contains but what question it was meant to answer. Let that shape your
recommendations.
