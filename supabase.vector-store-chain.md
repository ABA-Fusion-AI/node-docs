---
node_id: "supabase.vector-store-chain"
title: "Supabase - Retrieve as Tool"
description: "Retrieve documents from Supabase using semantic search, designed for AI agent tools"
category: "actions"
subcategory: "supabase"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - supabase
  - vector-store-chain
  - tool
  - embeddings
related_nodes:
  - supabase.get-many
  - supabase.insert-documents
  - supabase.vector-store-tool
---

<!-- SECTION: header -->

# Supabase - Retrieve as Tool

> **Category:** Actions | **Type:** Action Node

Tool-style retrieval for agents (input query or [[fromAgent]]).

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `supabaseUrl` | `string` | Yes | - | Supabase project URL |
| `supabaseKey` | `string` | Yes | - | Supabase API key |
| `tableName` | `string` | No | `documents` | Table that stores embedded documents |
| `rpcFunctionName` | `string` | No | `match_documents` | RPC function name |
| `limit` | `number` | No | `5` | Max results per query |
| `matchThreshold` | `number` | No | `0.5` | Similarity threshold (0-1) |
| `metadataFilter` | `string` | No | - | JSON object string for metadata filter |
| `threadId` | `string` | No | - | Thread identifier for scoping results |
| `threadIdField` | `string` | Yes* | `thread_id` | Metadata field name for thread ID |
| `embeddingProvider` | `enum` | No | `ollama` | `ollama` or `openai` |
| `ollamaUrl` | `string` | Yes* | - | Ollama base URL (required for `ollama`) |
| `ollamaModel` | `string` | Yes* | `nomic-embed-text` | Ollama model name (required for `ollama`) |
| `openaiApiKey` | `string` | Yes* | - | OpenAI API key (required for `openai`) |
| `openaiModel` | `string` | Yes* | `text-embedding-3-small` | OpenAI model name (required for `openai`) |
| `query` | `string` | Yes | - | Search query (`[[fromAgent]]` for agent tools) |

*Required only when the related field is used.

### Outputs

| Operation | Output |
| --- | --- |
| `retrieve` | `{ documents, count, query, threadId?, timestamp }` |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `string` or `object` | Query string, or object containing `query` or `input` |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `object` | Retrieved documents with `count`, `query`, and `timestamp` |
| `error` | `Error` | Thrown when embedding or RPC calls fail |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### Search query cannot be empty

**Cause:** Incoming input is empty or missing `query` / `input` fields.

**Solution:** Pass a non-empty string or object with `query`.

#### Invalid metadata filter JSON

**Cause:** `metadataFilter` is not valid JSON.

**Solution:** Provide a JSON object string. Invalid filters are ignored.

#### Supabase RPC error

**Cause:** RPC function missing or parameters mismatch.

**Solution:** Verify `rpcFunctionName` and the function signature.

<!-- /SECTION: troubleshooting -->

---


