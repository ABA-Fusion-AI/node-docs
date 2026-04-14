---
node_id: "segment-group"
title: "Segment Group"
description: "Add a user to a group with deep nested path-based dependencies."
category: "marketing"
subcategory: "analytics"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - segment
  - group
  - analytics
  - action
---

<!-- SECTION: header -->
# Segment Group

> **Category:** Marketing | **Type:** Action Node

Connect to Segment to dynamically map users to specific accounts, companies, or organizations. The Group action handles complex nested contexts natively, allowing you to pass deep application, campaign, and device attributes gracefully.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Segment Group** node is an advanced action designed to map users to their organizational counterparts inside of Segment (e.g. an "Account" or "Company" level struct). It comes with deep UI toggle structures, enabling you to gracefully reveal and pack optional metadata like Campaign tags, App version targets, and Device telemetry into your outbound payload.

### Key Features

- **Nested Flow Menus:** Dynamically hides and cleans empty structured metadata (like App, Campaign, and Device context) so your payload remains strictly compliant with the Segment Tracking Spec.
- **Dynamic Traits Mapping:** Support assigning varying customized organizational properties directly to the group.
- **Specific Integration Routing:** Explicitly enable or disable outbound push filtering to downstream tools like Salesforce.

### Use Cases

- Track B2B accounts by adding users to structured organizations with associated traits inside Segment.
- Log specific device metrics and campaign attribution context selectively to groups when new Enterprise deals are signed.
- Sync active trial upgrades via dynamic integration filtering without pushing the noisy data to tools that don't need it.

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `writeKey` | `string` | ✅ Yes | — | Your Segment API Write Key for source authentication |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `resource` | `enum` | ✅ Yes | `Group` | The Segment resource block |
| `operation` | `enum` | ✅ Yes | `Add a user to a group`| Operation to perform |

### Audience Identifiers & Traits

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `groupId` | `string` | ✅ Yes | Internal ID mapping to the specific company or account |
| `userId` | `string` | ❌ No | ID of the specific user you are tying to this group |
| `traits` | `array` | ❌ No | Group specific traits, e.g., `[ { "key": "plan", "value": "premium" } ]` |

### Context Data (Optional)

To enable context tracking, you must toggle the **`showContext`** parameter to `Show`. 

| Parameter | Type | Description |
|-----------|------|-------------|
| `context.active` | `boolean`| Explicitly mark the context/user state as active |
| `context.ip` | `string` | IPv4 or IPv6 tracking metric |
| `context.locale` | `string` | Locale string tracking (e.g. `en-US`) |
| `context.page` | `string` | URI routing path tracking |
| `context.timezone`| `string` | Timezone string metric |

#### Nested Sub-Contexts

You can dig deeper by individually enabling sub-options under the main context tree:
* **App Visibility**: Map dynamic fields for application `name`, `version`, and `build`.
* **Campaign Visibility**: Log attribution data like `name`, `source`, `medium`, `term`, `content`.
* **Device Visibility**: Track hardware identifiers including `id`, `model`, `name`, `type`, and `version`.

### Integrations Data (Optional)

By default Segment distributes to all defined destinations. To override this, set **`showIntegrations`** to `Show`.

| Parameter | Type | Description |
|-----------|------|-------------|
| `integrations.all` | `boolean`| Explicit flag whether to funnel to natively unrestricted integrations |
| `integrations.salesforce`| `boolean`| Explicit flag routing data directly or denying route to Salesforce |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Base success response containing the mapped `userId` and `groupId` |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas

**`Add a user to a group`**
```json
{
  "operation": "Add a user to a group",
  "success": true,
  "data": {
    "userId": "user-4890",
    "groupId": "acme-corp-org"
  }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Associate a User to a Premium Company

Maps a user natively to an account with mapped organizational traits attached.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "operation": "Add a user to a group",
  "groupId": "org-id-123",
  "userId": "usr-987",
  "traits": [
    { "key": "name", "value": "Acme Corp" },
    { "key": "industry", "value": "Technology" },
    { "key": "plan", "value": "Enterprise" }
  ]
}
```

**Output:**
```json
{
  "operation": "Add a user to a group",
  "success": true,
  "data": {
    "userId": "usr-987",
    "groupId": "org-id-123"
  }
}
```

---

### Example: Track Context with Salesforce Sync Denied

Logs standard context, prevents Salesforce from capturing the sync, and passes campaign attribution.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "groupId": "org-id-123",
  "userId": "usr-987",
  "showContext": "Show",
  "context": {
    "ip": "192.168.1.1",
    "campaignVisibility": "Show",
    "campaign": {
      "source": "google",
      "medium": "cpc",
      "name": "summer-b2b"
    }
  },
  "showIntegrations": "Show",
  "integrations": {
    "all": true,
    "salesforce": false
  }
}
```

**Output:**
```json
{
  "operation": "Add a user to a group",
  "success": true,
  "data": {
    "userId": "usr-987",
    "groupId": "org-id-123"
  }
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `Setup failed` error

**Cause:** The provided Write Key is incorrect, malformed, or missing.

**Solution:** Verify your `writeKey` exists inside the Segment Source Settings page.

#### Missing fields in Segment dashboard

**Cause:** You mapped values inside nested Context objects, but forgot to enable the root visualization toggle (e.g., `showContext` was left on `Hide`).

**Solution:** Ensure all parent visibility toggles corresponding to your nested structs are intentionally toggled to `Show`.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->