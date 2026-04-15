---
node_id: "salesforce-user"
title: "Salesforce - User"
description: "Read Salesforce User records with get, list, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - user
  - crm
related_nodes:
  - salesforce-task
  - salesforce-search
---

# Salesforce - User

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceUserNode`

Operations: `get`, `getAll`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `get`: `userId`
- `getAll`: `autoFetch`, `maxFetch` are optional controls for pagination behavior

Outputs:

- User record payload for `get`
- User list payload for `getAll`
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "getAll",
  "autoFetch": true,
  "maxFetch": 200
}
```
