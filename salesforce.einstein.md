---
node_id: "salesforce-einstein"
title: "Salesforce - Einstein"
description: "Run Salesforce Einstein language and vision operations including prediction, training, and usage checks"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - einstein
  - ai
related_nodes:
  - salesforce-case
  - salesforce-search
---

# Salesforce - Einstein

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceEinstienNode`

Operations: `getApiUsage`, `getLanguageDatasets`, `predictSentiment`, `predictIntent`, `trainLanguageModel`, `getLanguageTrainingStatus`, `getVisionDatasets`, `predictImage`, `detectObjects`, `ocrImage`, `trainVisionModel`, `getVisionTrainingStatus`.

## Configuration

Auth:

- `accessToken` (required)
- `baseUrl` (optional, default `https://api.einstein.ai/v2`)

Key params by operation family:

- Language prediction and training params under corresponding language fields
- Vision prediction and training params under corresponding vision fields

Outputs:

- Einstein API response payloads for usage, datasets, predictions, and training status

## Example

```json
{
  "accessToken": "{{secrets.salesforceEinsteinToken}}",
  "operation": "predictSentiment",
  "sentimentData": {
    "document": "This experience was excellent.",
    "modelId": "CommunitySentiment"
  }
}
```
