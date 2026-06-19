---
node_id: "supabase.get-many"
title: "Supabase - Get Many (Vector Search)"
description: "Search similar documents in Supabase using vector embeddings with optional metadata filtering"
category: "actions"
subcategory: "supabase"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - supabase
  - get-many
  - vector-search
  - embeddings
related_nodes:
  - supabase.insert-documents
  - supabase.vector-store-chain
  - supabase.vector-store-tool
---

<!-- SECTION: header -->

# Supabase - Get Many (Vector Search)

> **Category:** Actions | **Type:** Action Node

Vector search documents in Supabase using embeddings (RPC-first with fallback).

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `supabaseUrl` | `string` | Yes | - | Supabase project URL |
| `supabaseKey` | `string` | Yes | - | Supabase API key |
| `tableName` | `string` | No | `documents` | Table that stores documents and embeddings |
| `prompt` | `string` | Yes | - | Search query to embed |
| `limit` | `number` | No | `10` | Max results to return |
| `matchThreshold` | `number` | No | `0.6` | Similarity threshold (0-1) |
| `queryName` | `string` | No | `match_documents_getmany` | RPC function name |
| `metadataFilter` | `string` | No | - | JSON object string for metadata filter |
| `embeddingProvider` | `enum` | Yes | - | `ollama` or `openai` |
| `ollamaUrl` | `string` | Yes* | - | Ollama base URL (required for `ollama`) |
| `ollamaModel` | `string` | Yes* | `nomic-embed-text` | Ollama model name (required for `ollama`) |
| `openaiApiKey` | `string` | Yes* | - | OpenAI API key (required for `openai`) |
| `openaiModel` | `string` | Yes* | `text-embedding-3-small` | OpenAI model name (required for `openai`) |

*Required when the selected provider is used.

### Outputs

| Operation | Output |
| --- | --- |
| `search` | Search response object with `documents`, `count`, and `searchMethod` |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `none` | This node uses the `prompt` configuration field; incoming input is ignored |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `object` | Search response containing `documents`, `count`, and `searchMethod` |
| `error` | `Error` | Thrown when embedding or API calls fail |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### RPC function not found

**Cause:** The RPC function in `queryName` is missing or misnamed.

**Solution:** The node falls back to client-side cosine similarity. Create the RPC function or fix the name for better performance.

#### Invalid metadata filter JSON

**Cause:** `metadataFilter` is not valid JSON.

**Solution:** Provide a JSON object string (for example: `{"source":"kb"}`). Invalid filters are ignored.

#### Embedding generation failed

**Cause:** Provider URL or API key is invalid.

**Solution:** Verify `ollamaUrl` or `openaiApiKey`, and ensure the service is reachable.

<!-- /SECTION: troubleshooting -->

---




