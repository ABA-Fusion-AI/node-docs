---
node_id: "vero-user"
title: "Vero User"
description: "Manage Vero users, subscriptions, identifiers, and tags"
category: "integrations"
subcategory: "vero"
version: "1.0.0"
language: "en"
last_updated: "2026-04-16"
author: "Fusion Team"
tags:
  - vero
  - user
  - tags
  - action
related_nodes:
  - vero-event
---

<!-- SECTION: header -->
# Vero User

> **Category:** Integrations | **Type:** Action Node

Create, update, identify, subscribe, unsubscribe, delete users, and manage tags in Vero.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Vero User** node provides multiple user operations. Select an `operation` and supply the required fields. You can attach custom data as key/value pairs to extend user profiles.

### Key Features

- **User Lifecycle:** Create, update, resubscribe, unsubscribe, delete
- **Identifier Management:** Change user ID
- **Tag Management:** Add or remove tags

### Use Cases

- Maintain user profiles with custom attributes
- Manage subscriptions and suppression
- Assign tags for segmentation

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `authToken` | `string` | Yes | - | Vero Auth Token |

### Operation Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | - | `Create or update a user`, `Change a user identifier`, `Unsubscribe a user`, `Resubscribe a user`, `Delete a user`, `Add a tag`, `Remove a tag` |
| `id` | `string` | Yes* | - | User ID (most operations) |
| `email` | `string` | No | - | Email address (`Create or update a user`) |
| `customData` | `array` | No | - | Key/value user attributes (`Create or update a user`) |
| `newId` | `string` | Yes* | - | New User ID (`Change a user identifier`) |
| `tag` | `string` | Yes* | - | Tag name (`Add a tag`, `Remove a tag`) |

*Required only for the listed operation.

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
| `output` | `object` | Vero API response for the operation |
| `error` | `Error` | Emitted if validation or API calls fail |

### Output Schema

```json
{
  "status": "success",
  "id": "user-123"
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Create or Update a User

```json
{
  "authToken": "vero_token",
  "operation": "Create or update a user",
  "id": "user-123",
  "email": "user@example.com",
  "customData": [
    { "name": "plan", "value": "pro" },
    { "name": "age", "value": "32" }
  ]
}
```

### Example: Add a Tag

```json
{
  "authToken": "vero_token",
  "operation": "Add a tag",
  "id": "user-123",
  "tag": "vip"
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### Vero connection failed: invalid auth token

**Cause:** The auth token is missing or invalid.

**Solution:** Verify the `authToken` from your Vero account.

#### "User ID" is required

**Cause:** `id` was empty or missing.

**Solution:** Provide a non-empty `id` for the selected operation.

#### "New User ID" is required

**Cause:** `newId` was empty or missing for change identifier.

**Solution:** Provide a non-empty `newId`.

#### "Tag" is required

**Cause:** `tag` was empty or missing for tag operations.

**Solution:** Provide a non-empty tag.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- Vero Event

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-16 | Initial release |

<!-- /SECTION: changelog -->
