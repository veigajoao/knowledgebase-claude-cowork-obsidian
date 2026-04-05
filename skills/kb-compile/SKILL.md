---
name: kb-compile
description: >
  Knowledge base compilation skill. Use this when the user wants to turn ingested
  source material into wiki articles — phrases like "compile the wiki", "update
  the wiki", "write articles from the new sources", "process the ingest report",
  "build out the wiki", or "I've ingested some files, now what?" should trigger
  this. This is the core wiki-building step that runs after kb-ingest. Also
  triggers when the user asks to "add [topic] to the wiki" or "create an article
  about [concept]".
---

# KB Compile

You are the wiki author. Your job is to read ingest reports and source material
from `raw/`, then write or update concept articles in `wiki/` and keep the master
index current. The user rarely touches wiki files directly — you are their author
and editor.

## Before you start

Read `index/master-index.md` first. Understanding what already exists prevents
you from creating duplicate articles and helps you spot where to add crosslinks.
If it doesn't exist yet, you'll create it at the end.

Then read the latest `index/ingest-report-*.md` (or all unacted-on reports if
there are several). These are your compilation instructions — the ingest step has
already done the analysis; your job now is to turn those recommendations into
actual articles.

## Article format

Every article in `wiki/` must follow this structure exactly:

```markdown
---
tags: [topic, subtopic]
last_updated: YYYY-MM-DD
sources: [raw/filename-or-URL]
---

# Concept Name

## Summary
One concise paragraph. A reader unfamiliar with this topic should understand
the core idea after reading this alone.

## Details
Thorough explanation. Use subheadings (###) freely for complex topics.
Cite sources inline as (Source: filename).

## Key Facts
- Bullet points of the most important discrete, concrete facts.

## Related Concepts
- [[concept-name]] — one sentence on why it's related

## Open Questions
Unresolved ambiguities, contradictions between sources, or gaps worth
investigating. Leave this empty rather than inventing content.

## Sources
- [Title](raw/filename or URL)
```

## File naming and organization

- Path: `wiki/[topic]/[concept-name].md`
- Filenames: lowercase, hyphens, no spaces (e.g. `transformer-attention.md`)
- Topic folders: broad enough to group related articles, not so broad they're
  meaningless. Use the ingest report's topic tags as a guide.
- When in doubt, create the folder — a wiki with clear topic structure is easier
  to navigate and query than a flat pile of files.

## Compilation steps

### 1. For each recommended new article

Write the full article using content from the relevant `raw/` files. Be
thorough in the Details section — this is a reference document, not a stub.
Vague or minimal articles defeat the purpose of the knowledge base.

Cross-reference other articles generously in Related Concepts. Good crosslinking
is what makes the wiki queryable rather than just a pile of summaries.

### 2. For each recommended enrichment to an existing article

Read the existing article first. Then add or update only what the new source
genuinely adds — don't dilute or contradict content without reason. Update
`last_updated` and add the new source to the Sources section.

### 3. Update the master index

After all articles are written or updated, sync `index/master-index.md`:

```markdown
# Master Index
Last updated: YYYY-MM-DD

| Article | Path | Summary | Tags | Updated |
|---------|------|---------|------|---------|
| Concept Name | wiki/topic/concept-name.md | One-line summary | tag1, tag2 | YYYY-MM-DD |
```

Add new rows for new articles, update existing rows for enriched articles.

### 4. Update topic summaries

For each topic folder that received new or updated articles, write or update
`index/[topic]-summary.md`. This is a 1–2 paragraph overview of everything
the wiki currently covers in that topic, with links to the key articles. These
are what kb-qa reads first when answering topic-specific questions.

### 5. Mark ingest reports as acted on

Update the relevant rows in `index/ingest-log.md` from `ready-for-compile` to
`compiled-YYYY-MM-DD`.

### 6. Tell the user what you built

Summarize: how many articles created, how many enriched, which topics now have
coverage. Mention any Open Questions that stood out as worth investigating — the
user may want to run kb-lint or do a targeted Q&A on those.

## Quality bar

A compiled wiki article should be good enough that someone could read it and
walk away genuinely understanding the concept — not just knowing that it exists.
Aim for the quality of a strong Wikipedia entry: clear, specific, well-sourced,
and honestly acknowledging what's uncertain or contested.

If a source is thin, say so in Open Questions rather than padding the article.
A short, honest article is better than a long, vague one.
