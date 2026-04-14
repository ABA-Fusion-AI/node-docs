---
node_id: "supabase.insert-documents"
title: "Supabase File Converter"
description: "Insert, update, or upload documents to Supabase with optional embeddings"
category: "actions"
subcategory: "supabase"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - supabase
  - insert-documents
  - upload
  - embeddings
related_nodes:
  - supabase.get-many
  - supabase.vector-store-chain
  - supabase.vector-store-tool
---

<!-- SECTION: header -->

# Supabase File Converter

> **Category:** Actions | **Type:** Action Node

File operations: `insertDocuments`, `updateDocuments`, `uploadFile`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `supabaseUrl` | `string` | Yes | - | Supabase project URL |
| `supabaseKey` | `string` | Yes | - | Supabase API key |
| `operation` | `enum` | Yes | - | `insertDocuments`, `updateDocuments`, `uploadFile` |
| `table` | `string` | Yes* | `documents` | Target table (`insertDocuments`, `updateDocuments`) |
| `storageBucket` | `string` | Yes* | `files` | Storage bucket (`uploadFile`) |
| `storageFolder` | `string` | No | - | Optional folder path in the bucket |
| `id` | `string` | Yes* | - | Document ID (`updateDocuments`) |
| `embeddingProvider` | `enum` | No | `none` | `none`, `ollama`, `openai` |
| `ollamaUrl` | `string` | Yes* | - | Ollama base URL (required for `ollama`) |
| `ollamaModel` | `string` | Yes* | `nomic-embed-text` | Ollama model name (required for `ollama`) |
| `openaiApiKey` | `string` | Yes* | - | OpenAI API key (required for `openai`) |
| `openaiModel` | `string` | Yes* | `text-embedding-3-small` | OpenAI model name (required for `openai`) |

*Required only for the listed operation or provider.

### Outputs

| Operation | Output |
| --- | --- |
| `insertDocuments` | `{ success: true, contentLength, hasMetadata, hasEmbedding }` |
| `updateDocuments` | `{ success: true, id, contentLength, hasMetadata, hasEmbedding }` |
| `uploadFile` | `{ success: true, fileName, url }` |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `string` or `object` | Base64 content or object containing `base64`, `contentBase64`, or `fileBase64` |
| `metadata` | `object` | Optional metadata; also accepts `fileMetadata`, `folder`, `meta`, `data.metadata`, or `file.metadata` |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `object` | Operation result (see outputs in Configuration) |
| `error` | `Error` | Thrown on validation or API failures |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### no data detected

**Cause:** No base64 content provided in the incoming input.

**Solution:** Pass a base64 string or one of the supported base64 fields.

#### ID is required for updateDocuments

**Cause:** `operation=updateDocuments` without `id`.

**Solution:** Provide the document `id` to update.

#### PDF text extraction failed

**Cause:** `pdf-parse` is not installed or the PDF is not readable.

**Solution:** Install `pdf-parse` in the runtime or provide plain-text content.

<!-- /SECTION: troubleshooting -->

