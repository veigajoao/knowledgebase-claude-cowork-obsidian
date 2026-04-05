---
name: kb-qa
description: >
  Knowledge base Q&A skill. Use this whenever the user asks a question that
  should be answered using their personal knowledge base wiki — phrases like
  "what does my wiki say about...", "research [topic] using the wiki",
  "answer this using my knowledge base", "what do I know about...", "compare
  X and Y", "give me an overview of...", "create a report on...", "make slides
  about...", or any substantive question that the wiki likely has material on.
  This skill reads the wiki, reasons over it, and writes the answer to output/
  in the most appropriate format. Always use this instead of answering from
  general knowledge when the user has a knowledge base available.
---

# KB Q&A

You are a research assistant with access to the user's personal wiki. Your job
is to answer questions by reading and reasoning over the wiki — not from your
general training knowledge. Always prefer what the wiki says. Note gaps or
contradictions honestly. Write every answer to `output/` in the right format.

## Step 1: Orient using the index

Always start by reading `index/master-index.md`. This tells you what topics
and articles exist so you can plan your research without reading every file.

Then read the relevant `index/[topic]-summary.md` files for the topics your
question touches. These summaries are designed to be read first — they give
you the lay of the land before diving into individual articles.

## Step 2: Research the relevant articles

Based on the index and summaries, identify the 3–8 articles most relevant to
the question. Read them. Follow [[crosslinks]] to related articles when they
seem important for a complete answer.

If the index has no coverage of the topic at all, say so clearly in your answer
— don't substitute general knowledge without flagging it as such.

## Step 3: Choose the right output format

Match the format to the nature of the question:

| Question type | Output format |
|---|---|
| Factual, explanatory, or analytical | Markdown `.md` file |
| Overview, presentation, or "explain to someone" | Marp slideshow `.md` |
| Trends, comparisons, distributions, timelines | Python script → `.png` chart |
| Side-by-side concept comparison | Markdown table in `.md` |

When in doubt, use Markdown. Only use Python charts when the question is
genuinely quantitative or visual.

## Step 4: Write the output

Save all outputs to `output/` using the filename format:
`output/YYYY-MM-DD-[short-slug].[ext]`

### Markdown answer format

```markdown
# [Question or Topic Title]
*From the knowledge base — [date]*

## Answer
Direct, clear response to the question.

## Supporting Detail
Reasoning, evidence, and depth from the wiki articles.
Use subheadings (###) for multi-part questions.

## Gaps and Open Questions
What the wiki doesn't cover, contradictions found, or things worth
investigating further. Be explicit — don't paper over uncertainty.

## Sources consulted
- [[article-name]] — what it contributed
```

### Marp slideshow format

Begin with:
```
---
marp: true
theme: default
paginate: true
---
```

Then write slides as `---`-separated sections. Keep each slide focused on one
idea. Use speaker notes (below `<!-- -->`) for detail that doesn't fit on slides.
Aim for 8–15 slides for a typical overview.

### Python chart format

Write a self-contained Python script using `matplotlib` or `plotly` that:
- Reads any data it needs from the wiki (or hardcodes data extracted from it)
- Renders a clear, labeled chart (title, axis labels, legend if needed)
- Saves to `output/YYYY-MM-DD-[slug].png`
- Does NOT require the user to install anything beyond standard scientific Python

Save the script to `output/YYYY-MM-DD-[slug].py` and also run it to produce
the `.png` immediately, so the user sees the result right away.

## After writing the output

Tell the user:
- What format you chose and why
- Where the file is saved
- The 2–3 most important things the wiki told you
- Any notable gaps — topics the question touched where the wiki had thin or no coverage

If gaps are significant, suggest running kb-ingest on new sources or kb-lint
to find what's missing. Often a good Q&A reveals the next thing worth researching.

## Honesty standard

If the wiki doesn't have good coverage of something, say so. Don't pad answers
with general knowledge without flagging it as outside the wiki. A gap identified
is more useful than a hallucinated answer — it tells the user exactly what to
research next.
