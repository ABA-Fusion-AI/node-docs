---
node_id: "salesforce-flow"
title: "Salesforce - Flow"
description: "Invoke Salesforce Flows and list available flow definitions"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - flow
  - automation
related_nodes:
  - salesforce-case
  - salesforce-account
---

# Salesforce - Flow

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceFlowNode`

Operations: `invoke`, `getAll`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `invoke`: `flowApiName`
- `inputsJson` is optional; when omitted, object-shaped incoming data is used

Outputs:

- Flow invocation result payload for `invoke`
- Flow definition list for `getAll`

## Example

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "invoke",
  "flowApiName": "My_Flow_API_Name",
  "inputsJson": "{\"recordId\":\"001xx000003DHP0AAO\"}"
}
```
