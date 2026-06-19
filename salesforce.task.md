---
node_id: "salesforce-task"
title: "Salesforce - Task"
description: "Manage Salesforce Task records with create, update, fetch, delete, and summary operations"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - task
  - crm
related_nodes:
  - salesforce-case
  - salesforce-user
---

# Salesforce - Task

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceTaskNode`

Operations: `create`, `update`, `get`, `getAll`, `delete`, `getSummary`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `create`: `createSubject`
- `update`: `taskId` and at least one updatable field
- `get`/`delete`: `taskId`

Outputs:

- Standard Salesforce REST payload for task operations
- Summary payload for `getSummary`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "create",
  "createSubject": "Follow up call",
  "status": "Not Started",
  "priority": "Normal"
}
```
