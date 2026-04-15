---
node_id: "salesforce-case"
title: "Salesforce - Case"
description: "Manage Salesforce Case records and comments with create, update, fetch, delete, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - case
  - support
related_nodes:
  - salesforce-contact
  - salesforce-task
---

# Salesforce - Case

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceCaseNode`

Operations: `create`, `update`, `get`, `getAll`, `delete`, `addComment`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createSubject`
- `update`: `caseId` and at least one updatable field
- `get`/`delete`: `caseId`
- `addComment`: `caseId`, `commentBody`

Outputs:

- Standard Salesforce REST payload for case operations
- Comment payload for `addComment`
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "usernamePassword",
  "username": "integration.user@company.com",
  "password": "{{secrets.salesforcePassword}}",
  "securityToken": "{{secrets.salesforceSecurityToken}}",
  "operation": "addComment",
  "caseId": "500xx000001ABCdAAO",
  "commentBody": "Customer contacted and issue reproduced.",
  "commentIsPublished": true
}
```
