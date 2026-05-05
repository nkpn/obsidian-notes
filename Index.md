---
type: moc
updated: 2026-05-05
---

# Vault Index

Map of Content for the session DB. Treat this as the entry point — Claude reads this first to figure out what's already known before starting a new session.

## How this works

- **`sessions/`** — one note per Claude session. Each has a TL;DR, decisions, and `[[entity links]]`. **Reload only the TL;DR + linked entities to save tokens** instead of replaying full transcripts.
- **`entities/`** — atomic notes for projects, people, concepts, files. The graph nodes.
- **`react-lessons/`** — your existing React interview prep notes.
- **`Index.md`** — this file. The MOC / "global summary" Claude reads first.

## Latest sessions

- 2026-05-05 — [[2026-05-05-01-obsidian-session-db-setup]] — set up this whole graph-shaped session DB (current)
- 2026-05-05 — [[2026-05-05-02-cv-merge-speroteck]] *(backfill)* — merged + recruiter-tuned the Speroteck CV
- 2026-05-05 — [[2026-05-05-03-shopify-interview-prep]] *(backfill)* — built study guide / Q&A / system design docs for senior Shopify interview

## Sessions by tag

- `#career` → [[2026-05-05-02-cv-merge-speroteck]], [[2026-05-05-03-shopify-interview-prep]]
- `#interview-prep` → [[2026-05-05-03-shopify-interview-prep]]
- `#shopify` → [[2026-05-05-02-cv-merge-speroteck]], [[2026-05-05-03-shopify-interview-prep]]
- `#bigcommerce` → [[2026-05-05-02-cv-merge-speroteck]]
- `#knowledge-management` → [[2026-05-05-01-obsidian-session-db-setup]]
- `#graphrag` → [[2026-05-05-01-obsidian-session-db-setup]]

## Key entities

**Concepts**
- [[graphrag]] · [[obsidian-knowledge-graph]] · [[shopify-plus]] · [[hydrogen]] · [[shopify-functions]] · [[liquid]] · [[bigcommerce-catalyst]]

**Companies / People**
- [[speroteck]] · [[ipc2u]] · [[nikita-ilin]]

## Conventions (so future-Claude doesn't drift)

1. **Filenames** — `YYYY-MM-DD-NN-slug.md` (matches the `react-lessons/` style).
2. **Frontmatter is required** on session and entity notes. Sessions need `date`, `tags`, `project`, `entities`, `tldr`, `type: session`. Entities need `type: entity`, `category`, `tags`, `mentioned-in`.
3. **Entity notes are atomic** — one concept per note. Heavy linking via `[[wikilinks]]`, no duplication.
4. **TL;DR rule** — every session note opens with a ≤5-sentence TL;DR. That's what Claude reloads in future sessions.
5. **No transcript dumps** — capture decisions, deltas, open threads. Not chat logs.
6. **Backfills** are tagged `#backfill` in frontmatter so they're distinguishable from native sessions.
