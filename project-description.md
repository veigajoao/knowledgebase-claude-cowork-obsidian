# Project Description

> Paste one of the options below into the **Description** field of your Cowork project.
> Use the short version if the field has a character limit.

---

## Full version

LLM-maintained wiki. Drop sources in raw/ → ingest → compile → query.

Skills: kb-ingest (process raw/) → kb-compile (write wiki/) → kb-qa (answer questions) → kb-lint (health check)

Folders: raw/ (sources, read-only) | wiki/ (articles) | index/ (master index) | output/ (Q&A results) | tools/ (scripts)

Chain: "Ingest new files and compile the wiki" runs both steps at once.

---

## Short version

LLM wiki. raw/ → kb-ingest → kb-compile → wiki/
Query with kb-qa, audit with kb-lint.
Never edit wiki/ directly.
