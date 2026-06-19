---
node_id: "clockify-client"
title: "Clockify Client"
description: "Create, update, get, and delete Clockify clients."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - client
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Client

> **Category:** Productivity | **Type:** Action Node

Manage clients within a Clockify workspace. This node allows you to retrieve all clients, fetch a specific client, and create, update, or delete existing clients.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Client** node interacts with the Clockify API to manage client records in your workspace. It handles authentication via API Key and supports resolving the workspace by either its exact ID or its name.

### Key Features

- **Five Operations:** Get all clients, Get a client, Create a client, Update a client, Delete a client
- **Smart Workspace Resolution:** Provide the exact Workspace ID or just the Workspace Name, and the node will dynamically resolve it
- **Full Client Management:** Configure addresses, notes, and archived statuses alongside the client's name

### Use Cases

- Automatically create a Clockify client when a new customer signs up in your CRM
- Retrieve client lists to sync with an external database
- Archive inactive clients through an automated scheduled workflow
- Update client contact details when they change in another system

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `apiKey` | `string` | ✅ Yes | — | Your Clockify API Key for authentication |
| `workspaceId` | `string` | ✅ Yes | — | ID or name of the Clockify workspace |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | ✅ Yes | `Get all clients` | Operation to perform (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Get all clients` | Retrieve a list of all clients in the workspace |
| `Get a client` | Retrieve a specific client by ID |
| `Create a client` | Add a new client to the workspace |
| `Update a client` | Modify an existing client |
| `Delete a client` | Remove a client from the workspace |

---

### Operation: Get a client / Delete a client

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clientId` | `string` | ✅ Yes | ID of the client to retrieve or delete |

### Operation: Create a client

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clientName` | `string` | ✅ Yes | The name of the new client |
| `clientAddress` | `string` | ❌ No | Address of the client |
| `clientNote` | `string` | ❌ No | Additional notes for the client |

### Operation: Update a client

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `clientId` | `string` | ✅ Yes | ID of the client to update |
| `clientNameOptional` | `string` | ❌ No | The new name for the client |
| `clientAddress` | `string` | ❌ No | The new address for the client |
| `clientNote` | `string` | ❌ No | The new notes for the client |
| `clientArchived`| `boolean`| ❌ No | Whether the client should be archived |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data (usable via expressions) |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Result of the executed operation |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas by Operation

**`Get all clients`**
```json
{
  "operation": "Get all clients",
  "success": true,
  "data": [
    {
      "id": "62e2abc1...",
      "name": "Acme Corp",
      "archived": false,
      "address": "123 Main St",
      "note": "Premium customer",
      "workspaceId": "62e1xyz...",
      "currencyCode": "USD"
    }
  ]
}
```

**`Create a client` or `Update a client` or `Get a client`**
```json
{
  "operation": "Create a client",
  "success": true,
  "data": {
    "id": "62e2abc1...",
    "name": "Acme Corp",
    "archived": false,
    "address": "123 Main St",
    "note": "Premium customer",
    "workspaceId": "62e1xyz...",
    "currencyCode": "USD"
  }
}
```

**`Delete a client`**
```json
{
  "operation": "Delete a client",
  "success": true,
  "data": {
    "id": "62e2abc1..."
  }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Create a New Client

Create a new client with an address and a note.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace",
  "operation": "Create a client",
  "clientName": "Globex Corporation",
  "clientAddress": "456 Corporate Blvd",
  "clientNote": "New prospect from website"
}
```

**Output:**
```json
{
  "operation": "Create a client",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Globex Corporation",
    "archived": false,
    "address": "456 Corporate Blvd",
    "note": "New prospect from website",
    "workspaceId": "62e1xyz...",
    "currencyCode": "USD"
  }
}
```

---

### Example: Archive an Existing Client

Update an existing client to mark them as archived.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "62e1xyz...",
  "operation": "Update a client",
  "clientId": "63f4b...",
  "clientArchived": true
}
```

**Output:**
```json
{
  "operation": "Update a client",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Globex Corporation",
    "archived": true,
    "address": "456 Corporate Blvd",
    "note": "New prospect from website",
    "workspaceId": "62e1xyz...",
    "currencyCode": "USD"
  }
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `Invalid credentials` error

**Cause:** The provided API Key is incorrect or missing.

**Solution:** Verify your `apiKey` and ensure it is valid for your Clockify account.

#### `Workspace with name "{name}" not found` error

**Cause:** The `workspaceId` configuration was provided as a name, but no matching workspace was found in your Clockify account.

**Solution:** Check the spelling of the workspace name. It is case-insensitive, but must match exactly. Alternatively, provide the 24-character Workspace ID directly.

#### `Client ID is required` error

**Cause:** An operation like Get, Update, or Delete was selected but the `clientId` was not provided.

**Solution:** Ensure the `clientId` field is filled out correctly.

#### `Client Name is required` error

**Cause:** You chose the `Create a client` operation without specifying a name.

**Solution:** Provide a value for `clientName`.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->