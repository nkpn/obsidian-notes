---
date: 2026-05-05
type: session
project: obsidian-session-db
tags: [obsidian, knowledge-management, graphrag, setup]
entities: [[graphrag]], [[obsidian-knowledge-graph]]
tldr: Set up a lightweight graph-shaped session DB in this Obsidian vault. No GraphRAG server — just sessions/, entities/, Index.md MOC, and frontmatter that's GraphRAG-upgrade-friendly. Backfilled the two prior sessions (CV merge, Shopify interview prep).
status: active
---

# Obsidian Session DB — Setup

## TL;DR

Built a knowledge-graph-shaped Obsidian DB to persist Claude sessions and cut token usage on future ones. Approach is "lightweight graph": Obsidian's native `[[wikilinks]]` + tags + a MOC in [[Index]] do the work that a full GraphRAG pipeline would. If we outgrow it, the structure is forward-compatible with [obsidianGraphRAG](https://github.com/Jinstronda/obsidianGraphRAG) or Neural Composer.

## Why

Each new Claude session starts cold and re-derives context. For repeat work (CV iteration, Shopify prep, React lessons), that's wasted tokens. Storing structured session summaries + atomic entity notes means future sessions can reload just the TL;DR + relevant entity notes — typically a 25–250× reduction vs. full transcripts.

## Decisions

- **Storage: native Obsidian markdown.** No external server, no embedding pipeline. Reasoning: per Nikita's preferences (Don't overengineer · Simple beats complex · One way to do things), full GraphRAG infrastructure is out of proportion to the problem.
- **Structure: flat folders + tags + index** (chosen explicitly): `sessions/`, `entities/`, `Index.md` MOC.
- **Filename convention: `YYYY-MM-DD-NN-slug.md`** — matches the existing `react-lessons/` pattern.
- **Frontmatter required on every note.** YAML keys: `type`, `tags`, `entities`, `tldr`, `mentioned-in`. This is what makes the DB queryable both by Obsidian Dataview and any future RAG layer.
- **Backfill scope: 2 prior sessions** — CV merge ([[2026-05-05-02-cv-merge-speroteck]]) and Shopify interview prep ([[2026-05-05-03-shopify-interview-prep]]).
- **Considered and rejected:** running a full GraphRAG plugin (LightRAG / Neural Composer / obsidianGraphRAG). Rejected as overengineering for a personal session DB. Forward-compatible if it ever becomes worth it.

## What got built

```
obsidian-notes/
├── Index.md                ← MOC, entry point
├── sessions/
│   ├── 2026-05-05-01-obsidian-session-db-setup.md   (this note)
│   ├── 2026-05-05-02-cv-merge-speroteck.md          (backfill)
│   └── 2026-05-05-03-shopify-interview-prep.md      (backfill)
├── entities/               ← atomic notes, graph nodes
│   ├── graphrag.md
│   ├── obsidian-knowledge-graph.md
│   ├── shopify-plus.md
│   ├── hydrogen.md
│   ├── shopify-functions.md
│   ├── liquid.md
│   ├── bigcommerce-catalyst.md
│   ├── speroteck.md
│   ├── ipc2u.md
│   └── nikita-ilin.md
└── react-lessons/          ← pre-existing, untouched
```

## How to use this in future Claude sessions

1. Start of session → Claude reads `Index.md` first (cheap).
2. Identifies relevant past sessions / entities by tag.
3. Reads only those notes — not full transcripts.
4. End of session → Claude appends a new session note with TL;DR + entity links + updates `Index.md`.

## Open threads

- Nothing blocks usage now, but the loop in step 4 (Claude writing the wrap-up note at session end) is **manual** — Nikita has to ask. Future improvement: scheduled task or a slash-command-style trigger that runs at session close.
- Decide whether to include `react-lessons/` notes in the entity graph (they're domain knowledge, not session data — currently linked from Index only).

## Token-savings model

- Without DB: full re-explain per session ≈ 5k–50k tokens of context replay.
- With DB: `Index.md` (~400 tok) + 1–3 relevant session TL;DRs (~150 tok each) + 2–5 entity notes (~200 tok each) ≈ 1.5k–2.5k tokens.
- Net: roughly **3–20× reduction**, more on long-running projects.

## Linked entities

[[graphrag]] · [[obsidian-knowledge-graph]] · [[nikita-ilin]]
