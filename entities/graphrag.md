---
type: entity
category: concept
tags: [rag, knowledge-graph, llm, retrieval]
mentioned-in: [[2026-05-05-01-obsidian-session-db-setup]]
---

# GraphRAG

Microsoft Research's retrieval approach: instead of vanilla RAG (chunks → vector similarity), it extracts **entities + relationships** from documents into a knowledge graph, clusters into communities (typically Leiden), and generates **hierarchical summaries** at each community level. At query time it can do "global" queries (using community summaries) or "local" queries (traversing the graph from specific entities).

## Why it beats vanilla RAG for connected data

- Vanilla RAG misses cross-document connections — it retrieves chunks independently.
- GraphRAG can answer "what's the bigger picture across all my notes?" via community summaries.
- Better at "who/what is connected to X?" via graph traversal.

## Cost

- Indexing pass needs an LLM to extract entities/relationships per chunk → expensive at first.
- Community detection runs once per index rebuild.
- Storage = graph + embeddings + summaries.

## How it relates to this vault

We're using a **lightweight graph approach** (see [[obsidian-knowledge-graph]]) — the `[[wikilinks]]` + tags + Index MOC are the graph; no LLM extraction or community detection. Forward-compatible with full GraphRAG plugins like [obsidianGraphRAG](https://github.com/Jinstronda/obsidianGraphRAG) or Neural Composer if needed later.

## Related

[[obsidian-knowledge-graph]]
