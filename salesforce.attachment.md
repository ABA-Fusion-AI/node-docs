---
node_id: "salesforce-attachment"
title: "Salesforce - Attachment"
description: "Manage Salesforce Attachment records for create, update, fetch, delete, and summary workflows"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - attachment
  - files
related_nodes:
  - salesforce-document
  - salesforce-case
---

# Salesforce - Attachment

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceAttachmentNode`

Operations: `create`, `update`, `get`, `getAll`, `delete`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createParentId`, `createName`
- `update`: `attachmentId` and at least one updatable field
- `get`/`delete`: `attachmentId`

Outputs:

- Standard Salesforce REST payload for attachment operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createParentId": "001xx000003DHP0AAO",
  "createName": "invoice.pdf",
  "body": "<base64-content>",
  "contentType": "application/pdf"
}
```
