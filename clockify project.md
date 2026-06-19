---
node_id: "clockify-project"
title: "Clockify Project"
description: "Create, update, get, and delete Clockify projects."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - project
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Project

> **Category:** Productivity | **Type:** Action Node

Manage projects within a Clockify workspace. This node allows you to retrieve all projects, fetch a specific project, and create, update, or delete existing projects.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Project** node interacts with the Clockify API to manage project records in your workspace. It handles authentication via API Key and supports resolving the workspace and client by either their exact IDs or names.

### Key Features

- **Five Operations:** Get all projects, Get a project, Create a project, Update a project, Delete a project
- **Smart ID Resolution:** Provide the exact Workspace ID or Client ID, or simply provide their names, and the node will dynamically resolve them for you.
- **Full Project Management:** Configure project colors, visibility, billable status, client associations, and archived statuses alongside the project's name.

### Use Cases

- Automatically create a Clockify project when a new project is created in your task management tool
- Retrieve project lists for synchronizing timesheet data across environments
- Archive completed projects automatically once their status updates in your CRM
- Dynamically link projects to specific clients during creation using client names

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
| `operation` | `enum` | ✅ Yes | `Get all projects` | Operation to perform (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Get all projects` | Retrieve a list of all projects in the workspace |
| `Get a project` | Retrieve a specific project by ID |
| `Create a project` | Add a new project to the workspace |
| `Update a project` | Modify an existing project |
| `Delete a project` | Remove a project from the workspace |

---

### Operation: Get a project / Delete a project

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | `string` | ✅ Yes | ID of the project to retrieve or delete |

### Operation: Create a project

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectName` | `string` | ✅ Yes | The name of the new project |
| `projectColor` | `enum` | ❌ No | Visual color of the project (default "Sky Blue") |
| `projectClientId` | `string` | ❌ No | ID or name of the client associated with the project |
| `projectIsPublic` | `boolean`| ❌ No | Visibility of the project |
| `projectBillable` | `boolean`| ❌ No | Default billable status for time entries on this project |
| `projectNote` | `string` | ❌ No | Additional notes for the project |

### Operation: Update a project

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | `string` | ✅ Yes | ID of the project to update |
| `projectNameOptional`| `string` | ❌ No | The new name for the project |
| `projectColor` | `enum` | ❌ No | The new visual color of the project |
| `projectClientId` | `string` | ❌ No | The new ID or name of the client |
| `projectIsPublic` | `boolean`| ❌ No | The new visibility of the project |
| `projectBillable` | `boolean`| ❌ No | The new billable status |
| `projectNote` | `string` | ❌ No | The new notes |
| `projectArchived` | `boolean`| ❌ No | Whether the project should be archived |

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

**`Get all projects`**
```json
{
  "operation": "Get all projects",
  "success": true,
  "data": [
    {
      "id": "62e2abc1...",
      "name": "Website Redesign",
      "clientId": "62e3def...",
      "clientName": "Acme Corp",
      "color": "#0099CC",
      "billable": true,
      "archived": false,
      "note": "Design and development",
      "workspaceId": "62e1xyz..."
    }
  ]
}
```

**`Create a project` or `Update a project` or `Get a project`**
```json
{
  "operation": "Create a project",
  "success": true,
  "data": {
    "id": "62e2abc1...",
    "name": "Website Redesign",
    "clientId": "62e3def...",
    "clientName": "Acme Corp",
    "color": "#0099CC",
    "billable": true,
    "archived": false,
    "note": "Design and development",
    "workspaceId": "62e1xyz..."
  }
}
```

**`Delete a project`**
```json
{
  "operation": "Delete a project",
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

### Example: Create a New Project

Create a new billable project linked to a client.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace",
  "operation": "Create a project",
  "projectName": "Social Media Campaign",
  "projectColor": "Pink",
  "projectClientId": "Acme Corp",
  "projectBillable": true
}
```

**Output:**
```json
{
  "operation": "Create a project",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Social Media Campaign",
    "clientId": "62e3def...",
    "clientName": "Acme Corp",
    "color": "#E91E63",
    "billable": true,
    "archived": false,
    "note": "",
    "workspaceId": "62e1xyz..."
  }
}
```

---

### Example: Archive an Existing Project

Update an existing project to mark it as archived.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "62e1xyz...",
  "operation": "Update a project",
  "projectId": "63f4b...",
  "projectArchived": true
}
```

**Output:**
```json
{
  "operation": "Update a project",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Social Media Campaign",
    "clientId": "62e3def...",
    "clientName": "Acme Corp",
    "color": "#E91E63",
    "billable": true,
    "archived": true,
    "note": "",
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

#### `Client with name "{name}" not found in workspace` error

**Cause:** You mapped a client by name, but it doesn't exist in the current workspace.

**Solution:** Verify the client name exactly as it appears in Clockify, or use the exact Client ID.

#### `Project ID is required` error

**Cause:** An operation like Get, Update, or Delete was selected but the `projectId` was not provided.

**Solution:** Ensure the `projectId` field is filled out correctly.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->