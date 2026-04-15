---
node_id: "salesforce-contact"
title: "Salesforce - Contact"
description: "Manage Salesforce Contact records with create, update, upsert, fetch, delete, note, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - contact
  - crm
related_nodes:
  - salesforce-account
  - salesforce-lead
---

# Salesforce - Contact

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceContactNode`

Operations: `create`, `update`, `upsert`, `get`, `getAll`, `delete`, `addNote`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createLastName`
- `update`: `contactId` and at least one updatable field
- `upsert`: `externalIdField`, `externalIdValue`
- `get`/`delete`: `contactId`
- `addNote`: `contactId`, `noteTitle`

Outputs:

- Standard Salesforce REST payload for contact operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "upsert",
  "externalIdField": "External_Id__c",
  "externalIdValue": "contact-123",
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com"
}
```
