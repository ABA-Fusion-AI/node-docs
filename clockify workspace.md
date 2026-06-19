---
node_id: "clockify-workspace"
title: "Clockify Workspace"
description: "Get all Clockify workspaces."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - workspace
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Workspace

> **Category:** Productivity | **Type:** Action Node

Connect to the Clockify API to retrieve a list of all workspaces associated with your API key account.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Workspace** node is the simplest integration point for the Clockify platform. Instead of resolving data within a predefined workspace context, this node provides you with the foundational list of all connected Workspaces linked to the authenticated user.

### Key Features

- **Global Context Retrieval:** Pulls data at the highest level of the Clockify hierarchy.
- **Single Operation Simplicity:** Directly fetches all workspace objects with a clean array output.
- **No Dependencies:** Only requires an API Key — no secondary IDs or queries necessary.

### Use Cases

- Use this node at the start of a dynamic routing flow to find a Workspace by name before passing the `Workspace ID` to subsequent Time Entry or Task nodes
- Retrieve and verify active Feature Subscription Types across the portfolio of Workspaces you manage

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `apiKey` | `string` | ✅ Yes | — | Your Clockify API Key for authentication |

### Operations

This node natively runs entirely on its own global context and performs a single operation automatically: **`Get all workspaces`**.

---

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data (usable via expressions) |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Result of the executed operation |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas

**`Get all workspaces`**
```json
{
  "operation": "Get all workspaces",
  "success": true,
  "data": [
    {
      "id": "62e1xyz1...",
      "name": "Design Department",
      "imageUrl": "https://clockify.me/assets/images/workspace-logo.png",
      "featureSubscriptionType": "PREMIUM"
    },
    {
      "id": "62e1xyz2...",
      "name": "Marketing Team",
      "imageUrl": "https://clockify.me/assets/images/workspace-default.png",
      "featureSubscriptionType": "FREE"
    }
  ]
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Retrieve All Workspaces

Fetch the active list of workspaces linked to your API token.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere"
}
```

**Output:**
```json
{
  "operation": "Get all workspaces",
  "success": true,
  "data": [
    {
      "id": "63f4b...",
      "name": "Acme Inc Workspace",
      "imageUrl": "",
      "featureSubscriptionType": "BASIC"
    }
  ]
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `Invalid credentials` error

**Cause:** The provided API Key is incorrect or missing.

**Solution:** Verify your `apiKey` and ensure it is valid for your Clockify account. Once validated, you should be able to fetch the workspaces cleanly.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->