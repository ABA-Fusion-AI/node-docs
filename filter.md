---
node_id: "filter"
title: "Filter"
description: "Keep only the items in a list that satisfy a JavaScript condition. The list can come from the node input or a config-defined array."
category: "utilities"
subcategory: "data"
version: "2.1.0"
language: "en"
last_updated: "2026-06-16"
author: "Fusion Team"
tags:
  - utility
  - filter
  - javascript
  - array
  - list
  - condition
related_nodes:
  - function
  - iterate
  - switch
  - merge
---

<!-- SECTION: header -->
# Filter

> **Category:** Utilities | **Type:** Action Node

Keep only the items in a list that satisfy a JavaScript condition. The Filter node runs your condition once for every item in the incoming array — exactly like JavaScript's built-in `Array.filter()` — and outputs a new array containing only the items that passed.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Filter** node evaluates a small JavaScript **condition** against each item in a **list**. Items for which the condition is *truthy* are kept; the rest are dropped. The result is a new, filtered array emitted on the `success` output.

The list to filter can come from two places, controlled by the **Input Source** setting:

- **Node Input** (default): the array arriving from the previous node.
- **Config List**: a fixed array you define directly in the node's config.

> ⚠️ **The Filter node only works on arrays/lists.** If the selected source is not an array (an object, string, number, `null`, etc.), the node emits a descriptive error on the `error` output instead of filtering. Use the [Function](./function.md) node if you need to transform or gate a single value.

### Key Features

- **Two input sources:** Filter the incoming data, or a list defined right in the config.
- **Per-item evaluation:** Your condition runs once per item, just like `Array.prototype.filter`.
- **Familiar callback variables:** Access `item`, `index`, and `array` inside the condition.
- **Sandboxed execution:** Conditions run in an isolated VM (2-second timeout, 128 MB limit).
- **Full workflow context:** Read `variables`, `secrets`, and previous `outputs`.
- **Safe by default:** Non-array input produces a clear error rather than silently passing through.

### Use Cases

- Keep only active users from a list of accounts
- Drop records with missing or invalid fields
- Filter products above/below a price threshold
- Remove blocked items or items outside a date range
- Narrow down API results before sending them downstream

<!-- /SECTION: overview -->

---

<!-- SECTION: how-it-works -->
## How the Condition Works

The **condition** is a snippet of JavaScript that is evaluated **once for each item** in the input array. The node keeps an item when the condition evaluates to a **truthy** value and discards it when the value is **falsy**.

Think of it as the body of a standard `Array.filter` callback:

```javascript
// What you write in the condition field:
item.value > 0;

// Is effectively run as:
inputArray.filter((item, index, array) => {
  return item.value > 0;
});
```

### Writing the condition (expression style)

Write the condition as an **expression** — the **last expression is the result**. You do **not** write `return` (unlike the Function node). A trailing semicolon is fine.

```javascript
item.price > 100;          // keep items costing more than 100
item.status === "active";  // keep active items
item.tags.includes("vip"); // keep items tagged "vip"
```

Multi-statement conditions also work; the value of the final expression decides the outcome:

```javascript
const threshold = variables.minScore;
item.score >= threshold;   // this final expression is the result
```

### Variables available inside the condition

| Variable | Type | Description |
|----------|------|-------------|
| `item` | `any` | The current item being tested. |
| `input` | `any` | Alias for `item` (the current item). |
| `index` | `number` | The zero-based position of the current item in the array. |
| `array` | `any[]` | The full input array (handy for look-ahead/look-behind comparisons). |
| `variables` | `object` | Workflow variables. |
| `secrets` | `object` | Workflow secrets. |
| `outputs` | `object` | Outputs from previous nodes. |

> 💡 **`item` vs `input`:** Both refer to the *current item* being evaluated. `item` reads more naturally for list filtering; `input` is kept as an alias.

### Truthy vs falsy

Any JavaScript truthy value keeps the item; any falsy value (`false`, `0`, `""`, `null`, `undefined`, `NaN`) drops it. Conditions don't have to be strict booleans — for example, `item.email` keeps items that have a non-empty email.

### Referencing other nodes, variables, and secrets

Alongside `item`/`index`/`array`, the condition can read previous node outputs, workflow variables, and secrets via `outputs`, `variables`, and `secrets`.

> 📘 For the full rules — how `outputs` is keyed by node label, and why labels with spaces or special characters must use bracket-and-quote notation like `outputs["some node label with space in the name"]` — see the **[Using Expressions](./expressions.md)** guide.

<!-- /SECTION: how-it-works -->

---

<!-- SECTION: configuration -->
## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `inputSource` | `enum` | ❌ No | `Node Input` | Where to read the list from: `Node Input` (the data arriving from the previous node) or `Config List` (the `items` array defined below). |
| `items` | `any[]` | ❌ No | `[]` | The list of items to filter. **Only shown/used when `inputSource` is `Config List`.** |
| `condition` | `string` | ❌ No | `item.value > 0;` | JavaScript expression evaluated per item. Truthy keeps the item, falsy drops it. |

> 💡 The **`items`** field only appears in the editor when **Input Source** is set to **`Config List`**. When **Input Source** is **`Node Input`**, the field is hidden and the node filters whatever array arrives on the `input` port.

### Condition Guidelines

```javascript
// Available inside the condition:
// - item / input: the current item
// - index: position of the item in the array
// - array: the full input array
// - variables, secrets, outputs: workflow context

// Return the *last expression* as the test (no `return` keyword):
item.active === true;
```

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any[]` | The array of items to filter when `inputSource` is `Node Input`. **Must be an array.** |

> ℹ️ **The node always fires on an incoming `input` event**, regardless of the source setting. In `Config List` mode the incoming payload is ignored (the configured `items` are filtered instead), but an upstream signal is still what triggers the node. Connect a trigger/previous node to the `input` port even when filtering a config list.

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `success` | `any[]` | A new array containing only the items that passed the condition. May be an **empty array** if nothing matched. |
| `error` | `Error` | Emitted when the input is not an array or when the condition throws / times out. |

> ℹ️ **Empty results still emit.** Filtering a list always yields a list, so if no item matches, `success` emits `[]`. Downstream nodes should be ready to handle an empty array.

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Basic Example: Keep Items Above a Threshold

**Configuration:**
```javascript
item.value > 0;
```

**Input:**
```json
[
  { "value": 5 },
  { "value": -3 },
  { "value": 0 },
  { "value": 12 }
]
```

**Output (success):**
```json
[
  { "value": 5 },
  { "value": 12 }
]
```

---

### Example: Filter by String Field

Keep only active users.

**Configuration:**
```javascript
item.status === "active";
```

**Input:**
```json
[
  { "id": 1, "status": "active" },
  { "id": 2, "status": "disabled" },
  { "id": 3, "status": "active" }
]
```

**Output (success):**
```json
[
  { "id": 1, "status": "active" },
  { "id": 3, "status": "active" }
]
```

---

### Example: Using `index` (Keep Every Other Item)

The `index` variable lets you filter by position.

**Configuration:**
```javascript
index % 2 === 0;   // keep items at even positions
```

**Input:** `["a", "b", "c", "d", "e"]`

**Output (success):** `["a", "c", "e"]`

---

### Example: Using Workflow Variables

Use a workflow variable as the cutoff.

**Configuration:**
```javascript
item.score >= variables.minScore;
```

**Input (with `variables.minScore = 50`):**
```json
[
  { "name": "Alice", "score": 80 },
  { "name": "Bob", "score": 40 },
  { "name": "Carol", "score": 55 }
]
```

**Output (success):**
```json
[
  { "name": "Alice", "score": 80 },
  { "name": "Carol", "score": 55 }
]
```

---

### Example: Truthy Field Check (Drop Incomplete Records)

Keep only items that have a non-empty `email`.

**Configuration:**
```javascript
item.email;
```

**Input:**
```json
[
  { "id": 1, "email": "a@example.com" },
  { "id": 2, "email": "" },
  { "id": 3 }
]
```

**Output (success):**
```json
[
  { "id": 1, "email": "a@example.com" }
]
```

---

### Example: Filtering Primitive Values

The items don't have to be objects.

**Configuration:**
```javascript
item > 10;
```

**Input:** `[4, 11, 7, 25, 10]`

**Output (success):** `[11, 25]`

---

### Example: Filtering a Config List

Filter a fixed list defined in the node itself, ignoring the incoming payload. Set **Input Source** to `Config List` and fill in the `items` array.

**Configuration:**
```json
{
  "inputSource": "Config List",
  "items": [
    { "name": "Alice", "active": true },
    { "name": "Bob", "active": false },
    { "name": "Carol", "active": true }
  ],
  "condition": "item.active === true;"
}
```

Any incoming `input` event triggers the node; its payload is ignored.

**Output (success):**
```json
[
  { "name": "Alice", "active": true },
  { "name": "Carol", "active": true }
]
```

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Notify Only High-Value Orders

Fetch a list of orders, keep only the large ones, then iterate over the result to send notifications.

```json
{
  "nodes": [
    {
      "id": "fetch-orders",
      "type": "http-request",
      "config": {
        "url": "https://api.example.com/orders",
        "method": "GET"
      }
    },
    {
      "id": "filter-large",
      "type": "filter",
      "config": {
        "condition": "item.total >= 1000;"
      }
    },
    {
      "id": "iterate",
      "type": "iterate",
      "config": {
        "stagger": 500
      }
    }
  ]
}
```

**How it flows:**
1. The HTTP node returns an array of orders.
2. The Filter node keeps only orders where `total >= 1000`.
3. The Iterate node walks the filtered array one item at a time for downstream processing.

### Common Patterns

- **Pre-filter before iterating:** Filter → Iterate to process only relevant items.
- **Clean a data set:** Filter out records with missing/invalid fields before storing.
- **Threshold gating:** Keep only items above/below a numeric or date threshold.
- **Combine with Function:** Use [Function](./function.md) to transform items, then Filter to drop unwanted results (or vice versa).

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### "Filter node expects an array of items as input/in the config, but received object."

**Cause:** The selected source was not an array (it was an object, string, number, or `null`). The message says `as input` for `Node Input` mode and `in the config` for `Config List` mode.

**Solution (Node Input):** Make sure the upstream node outputs an array. If your data is nested (e.g. `{ "data": [ ... ] }`), use a [Function](./function.md) node first to return the inner array (`return input.data;`), then feed it into the Filter node.

**Solution (Config List):** Ensure the `items` field is a proper array. (It defaults to `[]`, so this normally only happens if it was overridden with a non-array value.)

#### The output is an empty array

**Cause:** No item satisfied the condition, or the field names in the condition don't match the data.

**Solution:** Double-check field names and value types. Remember `"10"` (string) is not `> 10` the way `10` (number) is. Inspect a sample item to confirm its shape.

#### Timeout error

**Cause:** The condition took longer than the 2-second execution limit across all items (e.g. a very large array with heavy per-item logic).

**Solution:** Simplify the condition, reduce the array size upstream, or move heavy processing to a [Function](./function.md) node.

#### "ReferenceError: X is not defined"

**Cause:** The condition references a variable that doesn't exist in the sandbox.

**Solution:** Only `item`, `input`, `index`, `array`, `variables`, `secrets`, and `outputs` are available. Access workflow data through `variables`/`secrets`/`outputs`.

### Error Codes

| Error | Cause | Solution |
|-------|-------|----------|
| `Error` (non-array input) | Input was not an array | Ensure the previous node emits an array |
| `SyntaxError` | Invalid JavaScript in the condition | Fix the condition syntax |
| `ReferenceError` | Unknown variable in the condition | Use only the available context variables |
| `TypeError` | Accessing a field on `null`/`undefined` | Add optional chaining, e.g. `item?.value > 0` |
| `TimeoutError` | Evaluation exceeded 2 seconds | Simplify the condition or reduce input size |

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- [Function](./function.md) – Transform data or apply custom logic to a single value
- [Iterate](./iterate.md) – Process each item of a (filtered) list one at a time
- [Switch](./switch.md) – Route data down different branches based on conditions
- [Merge](./merge.md) – Combine multiple streams before or after filtering

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.1.0 | 2026-06-16 | Added `inputSource` (`Node Input` / `Config List`) and a config-defined `items` list. The `items` field is shown only when `inputSource` is `Config List`. |
| 2.0.0 | 2026-06-16 | Filter now operates per-item over arrays (like `Array.filter`); non-array input emits an error. Added `item`, `index`, and `array` to the condition context. |
| 1.0.0 | 2026-01-31 | Initial release (single-value condition gate). |

<!-- /SECTION: changelog -->
