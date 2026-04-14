---
node_id: "segment-identify"
title: "Segment Identify"
description: "Identify a user and record their traits in Segment."
category: "marketing"
subcategory: "analytics"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - segment
  - identify
  - analytics
  - action
---

<!-- SECTION: header -->
# Segment Identify

> **Category:** Marketing | **Type:** Action Node

Connect to Segment to tie users to their actions and record their traits. The Identify action natively supports deep, nested context routing — empowering you to confidently log complex device telemetry and attribution variables synchronously with standard user details.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Segment Identify** node allows you to push identifiable metadata about your users natively into the Segment pipeline. Unlike the Group node, this centers exclusively on standard individual `User ID` mapping, letting downstream destinations seamlessly link page views, track events, and custom traits to a specific verified individual.

### Key Features

- **Standard Individual Alignment:** Centralizes and propagates user identity to hundreds of marketing and data warehousing tools simultaneously.
- **Deep Nested Flow Menus:** Dynamically hides and cleans empty nested parameters (like specific mobile device telemetry or customized campaign attribution) ensuring payloads match the spec exactly without triggering downstream validation warnings.
- **Direct Integration Management:** Choose which tools successfully receive this particular user identifier by explicitly routing outputs via the `integrations` toggle matrix.

### Use Cases

- Funnel new user sign-up details (like `email`, `first_name`, and `plan_type`) directly from your database into tools like Intercom, Mixpanel, and Salesforce.
- Synchronize anonymous IDs into strict identifiable User IDs post-checkout.
- Automatically enrich existing users with new tracking context (such as when they switch to a new Device ID or change physical `locale` mappings).

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
| `resource` | `enum` | ✅ Yes | `Identify` | The Segment resource block |
| `operation` | `enum` | ✅ Yes | `Create`   | Operation to perform |

### Audience Identifiers & Traits

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | `string` | ✅ Yes | The overarching database User ID tracking mapping |
| `traits` | `array` | ❌ No | User specific behavioral or defining traits (e.g., `email`, `role`, `tier`) |

### Context Data (Optional)

To enable deep analytic context tracking, toggle the **`showContext`** parameter to `Show`. 

| Parameter | Type | Description |
|-----------|------|-------------|
| `context.active` | `boolean`| Explicitly mark the context/user background session as active |
| `context.ip` | `string` | IPv4 or IPv6 tracking metric |
| `context.locale` | `string` | Locale string tracking (e.g. `en-US`) |
| `context.page` | `string` | URI routing path tracking |
| `context.timezone`| `string` | User's timezone metric |

#### Nested Sub-Contexts

You can refine further by individually expanding sub-options inside the root context menu:
* **App Visibility**: Standardize metadata for mobile apps including `name`, `version`, and `build`.
* **Campaign Visibility**: Provide native ad attribution metrics like `name`, `source`, `medium`, `term`, `content`.
* **Device Visibility**: Report hardware identifiers such as `id`, `manufacturer`, `model`, `name`, `type`, and `version`.

### Integrations Data (Optional)

Segment automatically fans your events out to all mapped destinations. You can explicitly block or target destinations by enabling **`showIntegrations`**.

| Parameter | Type | Description |
|-----------|------|-------------|
| `integrations.all` | `boolean`| Master flag: should this event go to natively unrestricted destinations? |
| `integrations.salesforce`| `boolean`| Explicitly push or deny this sync payload from touching Salesforce |

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
| `output` | `object` | Base success response containing the mapped `userId` |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas

**`Create (Identify)`**
```json
{
  "operation": "Create",
  "success": true,
  "data": {
    "userId": "usr-987"
  }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Identify a User with Upgraded Traits

Log a newly registered user inside Segment alongside standard database traits.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "operation": "Create",
  "userId": "usr-987",
  "traits": [
    { "key": "email", "value": "demo@example.com" },
    { "key": "first_name", "value": "Jane" },
    { "key": "plan", "value": "Premium" }
  ]
}
```

**Output:**
```json
{
  "operation": "Create",
  "success": true,
  "data": {
    "userId": "usr-987"
  }
}
```

---

### Example: Nested Device Telemety Handshake

Identify an existing user connecting from a new Apple device while explicitly hiding the event from your standard Salesforce pipeline.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "userId": "usr-987",
  "showContext": "Show",
  "context": {
    "locale": "en-US",
    "timezone": "America/New_York",
    "deviceVisibility": "Show",
    "device": {
      "manufacturer": "Apple",
      "model": "iPhone 14 Pro Max",
      "type": "ios"
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
  "operation": "Create",
  "success": true,
  "data": {
    "userId": "usr-987"
  }
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `You must provide a User ID` error

**Cause:** The `userId` field was unexpectedly left blank or the mapped dynamic variable was missing.

**Solution:** Unlike previous Segment nodes, `Identify` requires a mapped `userId` constraint safely. If tracking an anonymous visitor, use the Segment *Track* resource instead (or pass the `anonymousId` inside native traits if your downstream destinations approve it).

#### `Setup failed` error

**Cause:** The provided Write Key is incorrect, malformed, or missing.

**Solution:** Double-check your `writeKey` exists inside the Segment instance configuration.

#### Blank metadata inside Mixpanel/Amplitude

**Cause:** Your `appVisibility` or `deviceVisibility` toggles were left as `Hide`, preventing the nested child data from resolving upwards into the final API payload context.

**Solution:** Toggle the appropriate visibility block to `Show`.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->