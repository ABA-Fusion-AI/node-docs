---
node_id: "clockify-tag"
title: "Clockify Tag"
description: "Create, update, get, and delete Clockify tags."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - tag
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Tag

> **Category:** Productivity | **Type:** Action Node

Manage tags within a Clockify workspace. This node allows you to retrieve all tags and create, update, or delete existing tags to help categorize your time entries.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Tag** node interacts with the Clockify API to manage tag records in your workspace. It handles authentication via API Key and supports resolving the workspace by either its exact ID or its name.

### Key Features

- **Four Operations:** Get all tags, Create a tag, Update a tag, Delete a tag
- **Smart ID Resolution:** Provide the exact Workspace ID or simply provide its name, and the node will dynamically resolve it for you.
- **Easy Categorization:** Readily create tags dynamically from incoming data (e.g. from GitHub, Jira, or a CRM) to match your workflow labels.

### Use Cases

- Automatically create a new Clockify tag when a new label is created in your issue tracking system
- Sync tag lists with an external database for reporting consistency
- Clean up unused tags by automatically archiving them
- Rename tags programmatically when categories in your organization change

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
| `operation` | `enum` | ✅ Yes | `Get all tags` | Operation to perform (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Get all tags` | Retrieve a list of all tags in the workspace |
| `Create a tag` | Add a new tag to the workspace |
| `Update a tag` | Modify an existing tag |
| `Delete a tag` | Remove a tag from the workspace |

---

### Operation: Delete a tag

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tagId` | `string` | ✅ Yes | ID of the tag to delete |

### Operation: Create a tag

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tagName` | `string` | ✅ Yes | The name of the new tag |

### Operation: Update a tag

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tagId` | `string` | ✅ Yes | ID of the tag to update |
| `tagNameOptional`| `string` | ❌ No | The new name for the tag |
| `tagArchived` | `boolean`| ❌ No | Whether the tag should be archived |

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

**`Get all tags`**
```json
{
  "operation": "Get all tags",
  "success": true,
  "data": [
    {
      "id": "62e2abc1...",
      "name": "Urgent",
      "archived": false,
      "workspaceId": "62e1xyz..."
    }
  ]
}
```

**`Create a tag` or `Update a tag`**
```json
{
  "operation": "Create a tag",
  "success": true,
  "data": {
    "id": "62e2abc1...",
    "name": "Urgent",
    "archived": false,
    "workspaceId": "62e1xyz..."
  }
}
```

**`Delete a tag`**
```json
{
  "operation": "Delete a tag",
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

### Example: Create a New Tag

Create a new workflow tag automatically based on an incoming event.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace",
  "operation": "Create a tag",
  "tagName": "Bugfix"
}
```

**Output:**
```json
{
  "operation": "Create a tag",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Bugfix",
    "archived": false,
    "workspaceId": "62e1xyz..."
  }
}
```

---

### Example: Rename a Tag

Update an existing tag's name.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "62e1xyz...",
  "operation": "Update a tag",
  "tagId": "63f4b...",
  "tagNameOptional": "Feature Request"
}
```

**Output:**
```json
{
  "operation": "Update a tag",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Feature Request",
    "archived": false,
    "workspaceId": "62e1xyz..."
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

#### `Tag ID is required` error

**Cause:** An operation like Update or Delete was selected but the `tagId` was not provided.

**Solution:** Ensure the `tagId` field is filled out correctly.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->