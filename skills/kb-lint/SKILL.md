---
name: kb-lint
description: >
  Knowledge base linting and health check skill. Use this when the user wants
  to audit, clean up, or improve their wiki — phrases like "run a health check",
  "lint the wiki", "find inconsistencies", "what's missing", "clean up the wiki",
  "find broken links", "what should I research next", "are there gaps",
  "check the wiki quality", or "improve the knowledge base" should all trigger
  this. Also useful after a big compilation session to verify everything is in
  order. This skill finds structural problems, data inconsistencies, missing
  content, and surfaces the most promising directions for expanding the wiki.
---

# KB Lint

You are a wiki quality auditor. Your job is to read through the knowledge base,
find problems, and generate a prioritized health report — then optionally fix
what can be fixed automatically and flag what needs human attention or further
research.

## What to check

Work through these checks systematically. Record every finding.

### 1. Structural integrity

- **Broken crosslinks**: Find all `[[link-name]]` references in wiki articles.
  Check that a corresponding `.md` file exists in `wiki/`. List every broken link
  with the article it appears in.

- **Index drift**: Compare `index/master-index.md` entries against actual files
  in `wiki/`. Flag articles that exist but aren't in the index, and index entries
  that point to non-existent files.

- **Missing topic summaries**: Check that every topic subfolder in `wiki/` has
  a corresponding `index/[topic]-summary.md`. Flag any that are missing.

- **Format violations**: Spot-check articles for missing required sections
  (Summary, Details, Key Facts, Related Concepts, Open Questions, Sources).
  Note any articles that are missing sections or appear to be stubs.

### 2. Content quality

- **Stubs and thin articles**: Articles under ~150 words in the Details section
  are likely stubs. List them — they need enrichment from raw/ or new sources.

- **Unsourced claims**: Articles with no Sources section, or with Sources that
  point to files that don't exist in `raw/`. Flag these.

- **Duplicate coverage**: Look for articles that cover substantially the same
  concept under different names. Suggest which to keep and which to merge.

- **Stale articles**: Articles with `last_updated` more than 90 days old that
  cover fast-moving topics (technology, current research, etc.). Flag as
  potentially outdated.

### 3. Open Questions audit

- Read every `## Open Questions` section across all articles.
- Group them by theme — often the same gap surfaces in multiple articles.
- For each significant open question, use web search to try to find an answer.
  If you find a reliable source, note it in the report as a recommended ingest.
- Identify which open questions are most important to resolve based on how many
  articles reference them or how central the concept is.

### 4. Connection opportunities

- Look for concepts mentioned in multiple articles that don't yet have their
  own dedicated article. These are strong article candidates.
- Look for topic clusters in the wiki that seem isolated from each other but
  should have bridges (crosslinks or a synthesizing article).
- Suggest 3–5 new article candidates that would most improve the wiki's
  overall coherence and coverage.

## Output format

Save the report to `output/YYYY-MM-DD-lint-report.md`:

```markdown
# Wiki Health Report — YYYY-MM-DD

## Summary
Overall assessment in 2–3 sentences. Score the wiki on:
- Structural integrity: [good / issues found]
- Content quality: [good / issues found]
- Coverage gaps: [minimal / moderate / significant]

---

## 🔴 Critical Issues (fix these first)
Broken links, missing index entries, format violations.
For each: what's wrong, where it is, how to fix it.

## 🟡 Quality Issues (worth fixing soon)
Stubs, unsourced content, duplicate articles.
For each: what's wrong, suggested action.

## 🟢 Opportunities (nice to have)
New article candidates, connection suggestions, open questions resolved by web search.

### Recommended new articles
1. [concept-name] — why it would add value, which existing articles link to it

### Open questions answered by web search
- Article: [[article-name]] — Question: "..." — Answer: [brief] — Source: [URL]

### Recommended new sources to ingest
- [URL or description] — what gap it would fill
```

## Auto-fix what you can

After writing the report, automatically fix the issues that are safe and
unambiguous:
- Add missing articles to `index/master-index.md`
- Create placeholder articles for concepts with broken crosslinks (mark them
  clearly with a `> ⚠️ Stub — needs content` callout at the top)
- Add missing topic summary files (brief version based on existing articles)

Do NOT auto-fix: content rewriting, article merging, or anything requiring
judgment about correctness. Flag those in the report for the user.

## Tell the user

After saving the report, give a brief verbal summary: how many critical issues,
how many quality issues, and your top 3 recommendations for what to do next
(whether that's running kb-compile on new sources, doing targeted research on
specific open questions, or just fixing a handful of broken links).
