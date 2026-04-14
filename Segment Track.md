---
node_id: "segment-track"
title: "Segment Track"
description: "Track events and page views for a user."
category: "marketing"
subcategory: "analytics"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - segment
  - track
  - page
  - analytics
  - action
---

<!-- SECTION: header -->
# Segment Track

> **Category:** Marketing | **Type:** Action Node

Connect to Segment to comprehensively record user actions (`track()`) and website navigation (`page()`). The Track action natively routes dual API methods depending on your intention, while providing the standard deep context tracking structures you expect.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Segment Track** node is arguably the workhorse of your Segment integration. Rather than manually juggling distinct nodes for Page Tracking and Object Events, this node seamlessly groups both concepts into one cleanly packaged Action Node.

### Key Features

- **Dual Operation Routing:** Switches internal connection methods on the fly between tracking generic behavioral events and strictly typed Page loads dynamically without resetting the whole workflow.
- **Variable Injection Parsing:** Inherently toggles required fields (like isolating the `Event` name strictly to Event flows, or presenting `Name` properties purely on Page flows).
- **Extensible Contexts:** Provides the standardized deep UI toggle system (handling empty nested Campaign, App, and Device metrics) you've come to expect across other Segment modules.

### Use Cases

- Track custom behaviors like when a user pushes a distinct element button ("Played Video" or "Clicked Purchase CTA").
- Log a structured Page View explicitly into Segment when a user navigates to a sensitive dashboard or finishes a checkout URL.
- Funnel user event telemetry dynamically into Intercom or Mixpanel without touching tracking code on your actual site.

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
| `resource` | `enum` | ✅ Yes | `Track` | The Segment resource block |
| `operation` | `enum` | ✅ Yes | `Event` | Determines whether the node triggers a `track()` or `page()` operation internally |

### Tracking Settings & Data

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `userId` | `string` | ✅ Yes | Primary user identification tracking key |
| `event` | `string` | Condition | The primary name of the event being tracked (e.g. `User Signed Up`). **Required if Operation is "Event"**. |
| `name` | `string` | ❌ No | The name of the specific Page being requested (e.g. `Pricing Page`). Optional, applies only if Operation is "Page". |
| `properties` | `array` | ❌ No | Object-level arrays mapping specific tracked properties (`key`, `value`) |

### Context Data (Optional)

To enable analytic context mapping alongside the event, toggle the **`showContext`** parameter to `Show`. 

| Parameter | Type | Description |
|-----------|------|-------------|
| `context.active` | `boolean`| Explicitly mark the context/user instance as active |
| `context.ip` | `string` | IPv4 or IPv6 tracking metric |
| `context.locale` | `string` | Locale string tracking (e.g. `en-US`) |
| `context.page` | `string` | URI routing path tracking (often combined with standard Event runs) |
| `context.timezone`| `string` | User's timezone metric |

#### Nested Sub-Contexts

You can refine further by individually expanding sub-options inside the root context menu:
* **App Visibility**: Standardize metadata for mobile apps including `name`, `version`, and `build`.
* **Campaign Visibility**: Provide native ad attribution metrics like `name`, `source`, `medium`, `term`, `content`.
* **Device Visibility**: Report hardware identifiers such as `id`, `manufacturer`, `model`, `name`, `type`, and `version`.

### Integrations Data (Optional)

You can explicitly block or target designated sync platforms by enabling the **`showIntegrations`** toggle.

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
| `output` | `object` | Base success response containing the mapped identifiers |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas

**`Track Event`**
```json
{
  "operation": "Event",
  "success": true,
  "data": {
    "userId": "usr-987",
    "event": "Clicked Purchase Header"
  }
}
```

**`Track Page`**
```json
{
  "operation": "Page",
  "success": true,
  "data": {
    "userId": "usr-987",
    "name": "Checkout Flow Start"
  }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Log a Specific Behavioral Event

Log an event with customized dynamic properties natively attached.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "operation": "Event",
  "userId": "usr-987",
  "event": "Article Bookmarked",
  "properties": [
    { "key": "article_id", "value": "78789A" },
    { "key": "category", "value": "Engineering" }
  ]
}
```

**Output:**
```json
{
  "operation": "Event",
  "success": true,
  "data": {
    "userId": "usr-987",
    "event": "Article Bookmarked"
  }
}
```

---

### Example: Track a Page View explicitly

Log a page view and attach complex device context to better visualize what hardware your users are converting on natively.

**Configuration:**
```json
{
  "writeKey": "your_segment_write_key",
  "operation": "Page",
  "userId": "usr-987",
  "name": "Upgrade Confirmation View",
  "showContext": "Show",
  "context": {
    "deviceVisibility": "Show",
    "device": {
      "type": "ios",
      "model": "iPhone 15 Pro",
      "version": "17.1"
    }
  }
}
```

**Output:**
```json
{
  "operation": "Page",
  "success": true,
  "data": {
    "userId": "usr-987",
    "name": "Upgrade Confirmation View"
  }
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### `You must provide an Event Name` error

**Cause:** The workflow ran with `operation` locked to `Event`, but the `event` mapping field was left fundamentally blank or dynamic evaluation collapsed.

**Solution:** Segment exclusively blocks blank string arrays inside its Track routing. Ensure an explicit event name is provided.

#### `You must provide a User ID` error

**Cause:** You didn't map a `userId` tracking attribute to tie the event or page view structurally.

**Solution:** Populate an explicit `userId`. Anonymous events explicitly utilizing `anonymousId` are better served using raw HTTP payload tools if standard explicit user ID ties are missing.

#### `Setup failed` error

**Cause:** The provided Write Key is incorrect, malformed, or missing.

**Solution:** Double-check your `writeKey` exists inside the Segment instance configuration.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->