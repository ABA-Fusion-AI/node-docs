---
node_id: "vero-event"
title: "Vero Event"
description: "Track custom events for specific customers in Vero"
category: "integrations"
subcategory: "vero"
version: "1.0.0"
language: "en"
last_updated: "2026-04-16"
author: "Fusion Team"
tags:
  - vero
  - event
  - tracking
  - action
related_nodes:
  - vero-user
---

<!-- SECTION: header -->
# Vero Event

> **Category:** Integrations | **Type:** Action Node

Track custom events for a customer in Vero to power segmentation and personalized campaigns.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Vero Event** node sends a single event to Vero with optional data and extra attributes. It validates the auth token during setup and supports column-style key/value inputs for event properties.

### Key Features

- **Event Tracking:** Send custom events tied to a user ID
- **Structured Attributes:** Attach `eventData` and `extras` as key/value pairs
- **Safe JSON Parsing:** Accepts JSON-like values for columns

### Use Cases

- Track product views or cart actions
- Attach metadata like price, category, or campaign
- Power downstream automation based on event attributes

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `authToken` | `string` | Yes | - | Vero Auth Token |

### Event Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `id` | `string` | Yes | - | User ID that triggered the event |
| `eventName` | `string` | Yes | - | Event name (e.g. "Viewed product") |
| `eventData` | `array` | No | - | Key/value attributes for the event |
| `extras` | `array` | No | - | Extra metadata for personalization |

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
| `output` | `object` | Vero API response for the event |
| `error` | `Error` | Emitted if validation or API calls fail |

### Output Schema

```json
{
  "status": "success",
  "event": "Viewed product",
  "id": "user-123"
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Track a Product View

```json
{
  "authToken": "vero_token",
  "id": "user-123",
  "eventName": "Viewed product",
  "eventData": [
    { "name": "product_id", "value": "sku-88" },
    { "name": "price", "value": "49.99" }
  ],
  "extras": [
    { "name": "campaign", "value": "spring-launch" }
  ]
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

**Solution:** Provide a non-empty `id`.

#### "Event Name" is required

**Cause:** `eventName` was empty or missing.

**Solution:** Provide a non-empty `eventName`.

#### Invalid JSON for field

**Cause:** A column value looks like JSON but is invalid.

**Solution:** Provide valid JSON or plain strings.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- Vero User

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-16 | Initial release |

<!-- /SECTION: changelog -->
