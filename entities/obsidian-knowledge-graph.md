---
type: entity
category: concept
tags: [obsidian, knowledge-management, graph]
mentioned-in: [[2026-05-05-01-obsidian-session-db-setup]]
---

# Obsidian Knowledge Graph (lightweight)

The pattern this vault uses: Obsidian's native primitives provide ~80% of [[graphrag]]'s benefit at zero infrastructure cost.

## Mapping

| GraphRAG concept | Obsidian primitive |
|---|---|
| Entity (node) | Atomic note in `entities/` |
| Relationship (edge) | `[[wikilink]]` |
| Community | Tag |
| Global summary | [[Index]] MOC |
| Local query | Open a note, follow its links |
| Traversal | Obsidian Graph view |

## Why this is enough for personal session storage

- Volume is small (10s–100s of notes), not 1000s of documents.
- Recall doesn't need vector search — links + tags + Dataview suffice.
- LLM (Claude) does retrieval at session start by reading [[Index]] + named links — this is the "graph traversal."

## Upgrade path

If we ever outgrow this: drop in a real GraphRAG plugin on top of the same files. Frontmatter (`type`, `tags`, `mentioned-in`) is the stable schema both layers can read.

## Related

[[graphrag]]
