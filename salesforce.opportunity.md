---
node_id: "salesforce-opportunity"
title: "Salesforce - Opportunity"
description: "Manage Salesforce Opportunity records with create, update, upsert, fetch, delete, note, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - opportunity
  - crm
related_nodes:
  - salesforce-account
  - salesforce-lead
---

# Salesforce - Opportunity

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceOpportunityNode`

Operations: `create`, `update`, `upsert`, `get`, `getAll`, `delete`, `addNote`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createName`, `createStageName`, `createCloseDate`
- `update`: `opportunityId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `opportunityId`
- `addNote`: `opportunityId`, `noteTitle`

Outputs:

- Standard Salesforce REST payload for opportunity operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createName": "Q3 Renewal",
  "createStageName": "Prospecting",
  "createCloseDate": "2026-07-15",
  "amount": 25000
}
```
