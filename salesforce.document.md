---
node_id: "salesforce-document"
title: "Salesforce - Document"
description: "Manage Salesforce Document records with create, fetch, delete, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - document
  - files
related_nodes:
  - salesforce-attachment
  - salesforce-case
---

# Salesforce - Document

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceDocumentNode`

Operations: `create`, `get`, `getAll`, `delete`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createName`, `createFolderId`
- `get`/`delete`: `documentId`

Outputs:

- Standard Salesforce REST payload for document operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Release Notes",
  "createFolderId": "00lxx000000XYZAA2",
  "body": "<base64-content>",
  "contentType": "text/plain"
}
```
