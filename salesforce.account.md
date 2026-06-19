---
node_id: "salesforce-account"
title: "Salesforce - Account"
description: "Manage Salesforce Account records with create, update, upsert, fetch, delete, note, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - crm
  - account
related_nodes:
  - salesforce-contact
  - salesforce-opportunity
---

# Salesforce - Account

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceAccountNode`

Operations: `create`, `update`, `upsert`, `get`, `getAll`, `delete`, `addNote`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createName`
- `update`: `accountId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `accountId`
- `addNote`: `accountId`, `noteTitle`

Outputs:

- Standard Salesforce REST payload for record operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Acme Corp",
  "industry": "Technology"
}
```
