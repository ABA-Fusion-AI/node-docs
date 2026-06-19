---
node_id: "supabase.vector-store-tool"
title: "Supabase Vector Store Retriever"
description: "Retrieve relevant documents from a Supabase vector store for chain workflows"
category: "actions"
subcategory: "supabase"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - supabase
  - vector-store-tool
  - embeddings
  - retrieval
related_nodes:
  - supabase.get-many
  - supabase.insert-documents
  - supabase.vector-store-chain
---

<!-- SECTION: header -->

# Supabase Vector Store Retriever

> **Category:** Actions | **Type:** Action Node

Retrieval operations with `openai`, `cohere`, or `ollama` embeddings.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `supabaseUrl` | `string` | Yes | - | Supabase project URL |
| `supabaseKey` | `string` | Yes | - | Supabase API key |
| `tableName` | `string` | Yes | - | Table that stores embedded documents |
| `embeddingModel` | `enum` | No | `openai` | `openai`, `cohere`, `ollama` |
| `embeddingApiKey` | `string` | Yes* | - | API key for OpenAI or Cohere |
| `ollamaUrl` | `string` | Yes* | `http://localhost:11434` | Ollama base URL (required for `ollama`) |
| `ollamaModel` | `string` | Yes* | `nomic-embed-text` | Ollama model name (required for `ollama`) |
| `matchingFunction` | `string` | No | `match_documents` | Supabase RPC function name |
| `resultLimit` | `number` | No | `5` | Max results to return |
| `similarityThreshold` | `number` | No | `0.3` | Minimum similarity score (0-1) |
| `metadataFilters` | `object` | No | - | Key-value metadata filters (AND logic) |

*Required only for the selected embedding provider.

### Outputs

| Operation | Output |
| --- | --- |
| `retrieve` | Array of `{ content, metadata, score }` |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `string` or `object` | Query string; objects are stringified before embedding |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `array` | Ranked documents with `content`, `metadata`, and `score` |
| `error` | `Error` | Thrown when embedding or RPC calls fail |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### Missing embedding API key

**Cause:** `embeddingModel=openai` or `cohere` without `embeddingApiKey` or env var.

**Solution:** Provide `embeddingApiKey` or set `OPENAI_API_KEY` / `COHERE_API_KEY`.

#### Supabase RPC error

**Cause:** RPC function missing or parameters mismatch.

**Solution:** Verify `matchingFunction` and ensure it expects `query_embedding`, `match_count`, and `p_threshold`.

<!-- /SECTION: troubleshooting -->

---


