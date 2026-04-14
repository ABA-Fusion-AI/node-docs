---
node_id: "clockify-time-entry"
title: "Clockify Time Entry"
description: "Create, update, get, and delete Clockify time entries."
category: "productivity"
subcategory: "clockify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - clockify
  - time entry
  - time tracking
  - action
---

<!-- SECTION: header -->
# Clockify Time Entry

> **Category:** Productivity | **Type:** Action Node

Manage tracked time within a Clockify workspace. This node allows you to selectively fetch specific time entries, create new ones, update ongoing or past entries, or cleanly delete them.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Clockify Time Entry** node interacts with the Clockify API to programmatically log and manage tracked time records. It inherently resolves the Workspace by ID or Name, and handles the complicated mappings between standard UI Datetime inputs and Clockify's required ISO 8601 formatting context.

### Key Features

- **Four Operations:** Get a time entry, Create a time entry, Update a time entry, Delete a time entry.
- **Smart Date Parsing:** Natively handles `datetime-local` output format (YYYY-MM-DDTHH:MM) internally appending proper Seconds and Z offsets required by Clockify API.
- **Granular Connections:** Supports linking Time Entries natively to specific Projects and Tasks.

### Use Cases

- Stop an active timer dynamically by providing an `End Time` via a webhook or push of a button
- Sync time logs from an external proprietary time scanner directly into Clockify
- Create billable hour records instantaneously when a meeting finishes on a linked calendar
- Retroactively modify standard descriptions across historical time entries

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
| `operation` | `enum` | ✅ Yes | `Create a time entry` | Operation to perform (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Get a time entry` | Retrieve a specific time entry by ID |
| `Create a time entry` | Add a new time entry to the workspace |
| `Update a time entry` | Modify an existing time entry (like stopping an active timer) |
| `Delete a time entry` | Remove a time entry from the workspace |

---

### Operation: Get a time entry / Delete a time entry

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `timeEntryId` | `string` | ✅ Yes | ID of the time entry to retrieve or delete |

### Operation: Create a time entry

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `timeEntryStart` | `datetime`| ✅ Yes | The start date/time of the entry |
| `timeEntryEnd` | `datetime`| ❌ No | The end date/time. If omitted, timer remains actively running |
| `timeEntryDescription` | `string`| ❌ No | Description of what you are working on |
| `timeEntryProjectId` | `string`| ❌ No | ID of the project the time entry belongs to |
| `timeEntryTaskId` | `string`| ❌ No | ID of the task the time entry belongs to |
| `timeEntryBillable` | `boolean`| ❌ No | Indicates whether the entry is billable |

### Operation: Update a time entry

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `timeEntryId` | `string` | ✅ Yes | ID of the entry to update |
| `timeEntryStartOptional`| `datetime`| ❌ No | The new start date/time |
| `timeEntryEnd` | `datetime`| ❌ No | The new end date/time (providing this will stop an active timer) |
| `timeEntryDescription` | `string`| ❌ No | The new description |
| `timeEntryProjectId` | `string`| ❌ No | New project ID |
| `timeEntryTaskId` | `string`| ❌ No | New task ID |
| `timeEntryBillable` | `boolean`| ❌ No | Status update of billability |

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

**`Create a time entry` or `Update a time entry` or `Get a time entry`**
```json
{
  "operation": "Create a time entry",
  "success": true,
  "data": {
    "id": "62e2abc1...",
    "description": "Fixing bug 104",
    "billable": true,
    "projectId": "62e3def...",
    "taskId": null,
    "start": "2026-04-14T08:30:00Z",
    "end": "2026-04-14T10:00:00Z",
    "duration": "PT1H30M",
    "workspaceId": "62e1xyz..."
  }
}
```

**`Delete a time entry`**
```json
{
  "operation": "Delete a time entry",
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

### Example: Start a New Timer

Create an active time entry by only providing the start time and a description.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "My Workspace",
  "operation": "Create a time entry",
  "timeEntryStart": "2026-04-14T09:00",
  "timeEntryDescription": "Brainstorming session",
  "timeEntryBillable": true
}
```

**Output:**
```json
{
  "operation": "Create a time entry",
  "success": true,
  "data": {
    "id": "63f4b...",
    "description": "Brainstorming session",
    "billable": true,
    "projectId": null,
    "taskId": null,
    "start": "2026-04-14T09:00:00Z",
    "end": null,
    "duration": null,
    "workspaceId": "62e1xyz..."
  }
}
```

---

### Example: Stop an Active Timer

Update an active time entry by providing an End Time to successfully halt the timer count.

**Configuration:**
```json
{
  "apiKey": "yourapikeyhere",
  "workspaceId": "62e1xyz...",
  "operation": "Update a time entry",
  "timeEntryId": "63f4b...",
  "timeEntryEnd": "2026-04-14T10:45"
}
```

**Output:**
```json
{
  "operation": "Update a time entry",
  "success": true,
  "data": {
    "id": "63f4b...",
    "description": "Brainstorming session",
    "billable": true,
    "projectId": null,
    "taskId": null,
    "start": "2026-04-14T09:00:00Z",
    "end": "2026-04-14T10:45:00Z",
    "duration": "PT1H45M",
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

#### `Time Entry ID is required` error

**Cause:** An operation like Update, Get, or Delete was selected but the `timeEntryId` was not provided.

**Solution:** Ensure the `timeEntryId` field is filled out correctly.

#### `Start Time is required` error

**Cause:** You chose the `Create a time entry` operation but left the start date tracking field empty.

**Solution:** A start time is strictly required. Provide a valid static date/time mapping or a dynamic flow payload.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->