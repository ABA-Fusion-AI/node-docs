---
node_id: "clockify-task"
title: "Clockify Task"
description: "Create, update, get, and delete Clockify tasks."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - task
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Task

> **Category:** Productivity | **Type:** Action Node

Manage tasks within a Clockify project. This node allows you to retrieve all tasks, fetch a specific task, and create, update, or delete existing tasks with detailed estimates and assignments.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Task** node interacts with the Clockify API to manage task records under specific projects in your workspace. It handles authentication and gracefully resolves nested references, linking assignees by ID or Name alongside proper estimate tracking.

### Key Features

- **Five Operations:** Get all tasks, Get a task, Create a task, Update a task, Delete a task
- **Smart Id Resolution:** Provide Assignee IDs or direct names and emails — the node will instantly resolve the matching user IDs behind the scenes.
- **Granular Estimations:** Automatically translates simple inputs (Days, Hours, Minutes) into properly formatted ISO 8601 task estimation values (e.g. `PT3H45M`).

### Use Cases

- Automatically create specific sub-tasks when an initial design project is provisioned in the workspace
- Pull a list of all active tasks to verify that total hours haven't exceeded estimates
- Mark old tasks as `DONE` from external triggers
- Allocate assignees automatically to a task based on Jira ticket reporters

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `apiKey` | `string` | ✅ Yes | — | Your Clockify API Key for authentication |
| `workspaceId` | `string` | ✅ Yes | — | ID or name of the Clockify workspace |
| `projectId` | `string` | ✅ Yes | — | ID of the project the task belongs to |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | ✅ Yes | `Get all tasks` | Operation to perform (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Get all tasks` | Retrieve a list of all tasks in the project |
| `Get a task` | Retrieve a specific task by ID |
| `Create a task` | Add a new task to the project |
| `Update a task` | Modify an existing task |
| `Delete a task` | Remove a task from the project |

---

### Operation: Get a task / Delete a task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | `string` | ✅ Yes | ID of the task to retrieve or delete |

### Operation: Create a task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskName` | `string` | ✅ Yes | The name of the new task |
| `taskStatus` | `enum` | ❌ No | Status of the task (`ACTIVE` or `DONE`) |
| `taskAssigneeIds` | `string[]`| ❌ No | Array of user IDs or names to assign to the task |
| `estimateDays` | `number` | ❌ No | Days estimate |
| `estimateHours` | `number` | ❌ No | Hours estimate (0-23) |
| `estimateMinutes`| `number` | ❌ No | Minutes estimate (0-59) |

### Operation: Update a task

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `taskId` | `string` | ✅ Yes | ID of the task to update |
| `taskNameOptional`| `string` | ❌ No | The new name for the task |
| `taskStatus` | `enum` | ❌ No | The new status (`ACTIVE` or `DONE`) |
| `taskAssigneeIds` | `string[]`| ❌ No | Array of user IDs or names |
| `estimateDays` | `number` | ❌ No | New days estimate |
| `estimateHours` | `number` | ❌ No | New hours estimate (0-23) |
| `estimateMinutes`| `number` | ❌ No | New minutes estimate (0-59) |

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

**`Get all tasks`**
```json
{
  "operation": "Get all tasks",
  "success": true,
  "data": [
    {
      "id": "62e2abc1...",
      "name": "Design Mockups",
      "status": "ACTIVE",
      "assigneeIds": ["62e4ghi..."],
      "estimate": "PT4H30M",
      "projectId": "62e3def...",
      "workspaceId": "62e1xyz..."
    }
  ]
}
```

**`Create a task` or `Update a task` or `Get a task`**
```json
{
  "operation": "Create a task",
  "success": true,
  "data": {
    "id": "62e2abc1...",
    "name": "Design Mockups",
    "status": "ACTIVE",
    "assigneeIds": ["62e4ghi..."],
    "estimate": "PT4H30M",
    "projectId": "62e3def...",
    "workspaceId": "62e1xyz..."
  }
}
```

**`Delete a task`**
```json
{
  "operation": "Delete a task",
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

### Example: Create an Estimated Task

Create a new task with a specific 5-hour and 30-minute estimate, assigned strictly to one employee.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace",
  "projectId": "62e3def...",
  "operation": "Create a task",
  "taskName": "Initial Testing",
  "taskStatus": "ACTIVE",
  "taskAssigneeIds": ["Alice Smith", "bob@example.com"],
  "estimateHours": 5,
  "estimateMinutes": 30
}
```

**Output:**
```json
{
  "operation": "Create a task",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Initial Testing",
    "status": "ACTIVE",
    "assigneeIds": ["alice123...", "bob456..."],
    "estimate": "PT5H30M",
    "projectId": "62e3def...",
    "workspaceId": "62e1xyz..."
  }
}
```

---

### Example: Mark a Task as Done

Update an existing task status to reflect completion.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "62e1xyz...",
  "projectId": "62e3def...",
  "operation": "Update a task",
  "taskId": "63f4b...",
  "taskStatus": "DONE"
}
```

**Output:**
```json
{
  "operation": "Update a task",
  "success": true,
  "data": {
    "id": "63f4b...",
    "name": "Initial Testing",
    "status": "DONE",
    "assigneeIds": ["alice123...", "bob456..."],
    "estimate": "PT5H30M",
    "projectId": "62e3def...",
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

#### `User with name or email "{name}" not found in workspace` error

**Cause:** The user mapping using a display name or email failed.

**Solution:** Ensure that the assignee is invited to and exists inside the active `workspaceId`. Alternatively, provide their ID exactly.

#### `Task ID is required` error

**Cause:** An operation like Update, Get, or Delete was selected but the `taskId` was not provided.

**Solution:** Ensure the `taskId` field is filled out correctly.

#### `Days/Hours/Minutes must be between...` error

**Cause:** Your `estimateHours` was larger than 23, or your `estimateMinutes` was larger than 59.

**Solution:** Distribute overflow to the higher unit (e.g. 90 minutes should be 1 hour and 30 minutes).

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->