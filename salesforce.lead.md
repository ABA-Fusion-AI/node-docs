---
node_id: "salesforce-lead"
title: "Salesforce - Lead"
description: "Manage Salesforce Lead records with create, update, upsert, fetch, delete, note, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - lead
  - crm
related_nodes:
  - salesforce-contact
  - salesforce-opportunity
---

# Salesforce - Lead

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceLeadNode`

Operations: `create`, `update`, `upsert`, `get`, `getAll`, `delete`, `addNote`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createLastName`, `createCompany`
- `update`: `leadId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `leadId`
- `addNote`: `leadId`, `noteTitle`

Outputs:

- Standard Salesforce REST payload for lead operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createLastName": "Smith",
  "createCompany": "Globex",
  "email": "s.smith@globex.com",
  "status": "Open - Not Contacted"
}
```
