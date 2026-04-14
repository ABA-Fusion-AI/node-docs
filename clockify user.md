---
node_id: "clockify-user"
title: "Clockify User"
description: "Get all users in a Clockify workspace."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - user
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify User

> **Category:** Productivity | **Type:** Action Node

Connect to a Clockify workspace to seamlessly retrieve a complete list of all active or invited users.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify User** node allows you to pull all user records dynamically from a workspace. This is incredibly helpful when verifying assigned limits, syncing active employment records remotely, or matching Clockify User IDs to your company's CRM or Directory system automatically.

### Key Features

- **Single Operation Simplicity:** Directly fetches all workspace users with a clean array output.
- **Dynamic Identification:** Output includes their Name, strictly verified Email, Status, and Profile Picture mapping context.
- **Smart Workspace Resolution:** Provide the exact Workspace ID or just the Workspace Name, and the node will dynamically resolve it.

### Use Cases

- Synchronize Clockify User IDs against external HR databases for dynamic Task and Project creation flows
- Ensure users who have formally left the company are manually deactivated 
- Map employee email addresses to specific `Time Entry` nodes when responding to generic Calendar invites

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `apiKey` | `string` | ✅ Yes | — | Your Clockify API Key for authentication |
| `workspaceId` | `string` | ✅ Yes | — | ID or name of the Clockify workspace |

### Operations

Unlike other Clockify nodes, the User Action only performs a single read operation by default: **`Get all users`**.

---

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

### Output Schemas

**`Get all users`**
```json
{
  "operation": "Get all users",
  "success": true,
  "data": [
    {
      "id": "62e2abc1...",
      "name": "Jane Doe",
      "email": "jane.doe@example.com",
      "status": "ACTIVE",
      "profilePicture": "https://clockify.me/assets/images/default-avatar.png"
    },
    {
      "id": "62e2abc2...",
      "name": "John Smith",
      "email": "john.smith@example.com",
      "status": "PENDING",
      "profilePicture": "https://clockify.me/assets/images/default-avatar-2.png"
    }
  ]
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Retrieve All Registered Users

Fetch the active list of user models directly to log their IDs and build assignment logic for an impending `Create a task` action sequence.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace"
}
```

**Output:**
```json
{
  "operation": "Get all users",
  "success": true,
  "data": [
    {
      "id": "63f4b...",
      "name": "Alex Johnson",
      "email": "alex.j@company.com",
      "status": "ACTIVE",
      "profilePicture": "https://url-to-avatar"
    }
  ]
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

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->