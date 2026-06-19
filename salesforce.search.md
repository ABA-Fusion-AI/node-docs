---
node_id: "salesforce-search"
title: "Salesforce - Search"
description: "Query Salesforce using SOQL, SOSL, or parameterized search"
category: "integrations"
subcategory: "salesforce"
version: "1.0.0"
language: "en"
last_updated: "2026-04-15"
author: "Fusion Team"
tags:
  - integration
  - salesforce
  - search
  - soql
related_nodes:
  - salesforce-account
  - salesforce-contact
---

# Salesforce - Search

> **Category:** Integrations | **Type:** Action Node

Node class: `SalesforceSearchNode`

Operations: `soql`, `sosl`, `parameterized`.

## Configuration

Auth: provide `authType` with either `accessToken` credentials or username/password credentials.

Key params:

- `soql`: `soqlQuery`
- `sosl`: `soslQuery`
- `parameterized`: `searchTerm`

Outputs:

- Salesforce query results in normalized record arrays

## Examples

### SOQL

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "soql",
  "soqlQuery": "SELECT Id, Name FROM Account LIMIT 50"
}
```

### SOSL

```json
{
  "authType": "accessToken",
  "instanceUrl": "https://your-org.my.salesforce.com",
  "accessToken": "{{secrets.salesforceToken}}",
  "operation": "sosl",
  "soslQuery": "FIND {Acme} RETURNING Account(Id, Name), Contact(Id, Name)"
}
```
