---
node_id: "expressions"
title: "Using Expressions"
description: "How to reference workflow data — previous node outputs, variables, and secrets — inside any node's input fields."
category: "guides"
subcategory: "core-concepts"
version: "1.0.0"
language: "en"
last_updated: "2026-06-16"
author: "Fusion Team"
tags:
  - expressions
  - variables
  - outputs
  - secrets
  - dynamic-values
  - guide
related_nodes:
  - function
  - filter
  - set-variable
  - switch
---

<!-- SECTION: header -->
# Using Expressions

> **Category:** Guides | **Applies to:** All nodes

Most input fields accept either a **fixed value** that you type in, or an **expression** — a reference to data produced elsewhere in the workflow (a previous node's output, a workflow variable, or a secret). Expressions are what make a workflow *dynamic*: instead of hard-coding an email address or an ID, you point the field at a value that is computed while the workflow runs.

<!-- /SECTION: header -->

---

<!-- SECTION: turning-on -->
## Turning a field into an expression

Any input field that supports expressions shows a small **`expression`** toggle just above it. Click it to switch the field from a literal value into expression mode, then pick (or type) the value you want to reference. Click it again to switch back to a plain value.

> Some fields hide the toggle (for example fixed enums/dropdowns). If you don't see the **`expression`** button, that field only accepts a literal value.

<!-- /SECTION: turning-on -->

---

<!-- SECTION: data-context -->
## What data you can reference

Expressions can read from three sources. They are always available by these names:

| Name | Contains | Example reference |
|------|----------|-------------------|
| `outputs` | The data emitted by **previous nodes** in this run | `outputs["HTTP Request"].success` |
| `variables` | Your **Workflow Variables** | `variables.minScore` |
| `secrets` | Your **Workflow Secrets** (only offered on text/password/URL fields) | `secrets.apiKey` |

In **code fields** (the Function node's `code` and the Filter node's `condition`) you additionally get the data flowing into the node:

| Name | Contains |
|------|----------|
| `input` | The data arriving on the node's input |
| `item`, `index`, `array` | (Filter only) the current item, its position, and the whole list |

<!-- /SECTION: data-context -->

---

<!-- SECTION: outputs-structure -->
## How `outputs` is organised

`outputs` is a nested object, keyed **first by the node's label** (the name shown on the node in the canvas) and **then by the output handle** (the port the data left from — usually `success` or `output`):

```
outputs[ <node label> ][ <output handle> ]
```

If you had two upstream nodes labelled `HTTP Request` and `Set Score`, the data available to an expression looks like this:

```json
{
  "outputs": {
    "HTTP Request": {
      "success": { "users": [ { "id": 1, "name": "Alice" } ] }
    },
    "Set Score": {
      "output": 42
    }
  }
}
```

So to reach the user list you'd reference:

```
outputs["HTTP Request"].success.users
```

> 🔎 **Two things to get right:** the **label** must match the source node's name exactly, and the **handle** must match the port the data came out of (check the source node's *Outputs* section). Most action nodes emit on `success`; the Function node emits on `output`.

<!-- /SECTION: outputs-structure -->

---

<!-- SECTION: special-labels -->
## ⚠️ Node labels with spaces or special characters

This is the **single most common cause of broken expressions**, so it's worth understanding.

Node labels are free text — you can call a node `Get Users 🚀` or `user-list (v2)`. But those names contain spaces and symbols that are **not valid in a JavaScript property name**, so you cannot reach them with dot notation:

```javascript
// ❌ Breaks — "Filter Active" has a space
outputs.Filter Active.success

// ❌ Breaks — the hyphen is read as a minus sign
outputs.user-list.success
```

Instead, wrap the label in **square brackets and quotes**:

```javascript
// ✅ Works — the label is a quoted string inside [ ]
outputs["Filter Active"].success
outputs["user-list"].success
outputs["Get Users 🚀"].success
```

The same rule applies to **any property name** that contains spaces or special characters, at any depth:

```javascript
// label has a space AND the field name has a space
outputs["My API Call"].success["full name"]
```

> 💡 **Rule of thumb:** plain dot access (`.name`) only works for names made of letters, digits, `_` and `$` that don't start with a digit. When in doubt, use `["..."]` — it always works, even for simple names (`outputs["HttpRequest"].success` is just as valid as `outputs.HttpRequest.success`).

### The easiest way to avoid this

- **Use the picker.** When you toggle a field into expression mode, you can browse the available data and select the value — the editor inserts the correct reference for you, brackets and all.
- **Keep labels simple.** If you write expressions by hand a lot, naming nodes with letters and underscores (e.g. `http_request`) lets you use plain dot notation everywhere.
- **Avoid dots and quotes inside labels.** A `.` or `"` *inside* a label is especially error-prone — rename the node if you can.

<!-- /SECTION: special-labels -->

---

<!-- SECTION: code-fields -->
## Expressions in code fields (Function & Filter)

The **Function** node's `code` and the **Filter** node's `condition` are full **JavaScript**. Here `outputs`, `variables`, `secrets`, and `input` are real JavaScript objects, so you access them with normal JavaScript syntax — including the bracket-and-quote rule above.

```javascript
// Function node: build a request from a previous node's output + a secret
return {
  url: `${variables.apiBaseUrl}/users/${input.userId}`,
  token: secrets.apiToken,
  region: outputs["Get Current Region"].success.region
};
```

```javascript
// Filter node: keep items whose category was allowed by an earlier node
outputs["Allowed Categories"].success.includes(item.category);
```

> ✅ Because these fields are JavaScript, you can use anything the language offers — optional chaining, `includes()`, comparisons, template strings, etc.

<!-- /SECTION: code-fields -->

---

<!-- SECTION: template-syntax -->
## Expression syntax in text fields

Outside of code fields, expressions are embedded into a value using delimiters. There are three forms, depending on **what kind of value you need**:

| Syntax | Result | Use it when… |
|--------|--------|--------------|
| `{{ ... }}` | The value converted to **text**, substituted into the surrounding string | You're building a string, e.g. a URL or message |
| `[[ ... ]]` | The **raw value**, with its original type preserved (number, object, array, boolean) | The whole field should *be* that value, not a string |
| `$[[ ... ]]` | The value **JSON-stringified** | You need to embed an object/array as JSON text |

### Examples

```text
Hello {{ outputs["Get User"].success.name }}, welcome back!
→ "Hello Alice, welcome back!"
```

```text
[[ outputs["Get User"].success.age ]]
→ 30          (a number, not the string "30")
```

```text
Payload: $[[ outputs["Get User"].success ]]
→ Payload: {"name":"Alice","age":30}
```

> ℹ️ Inside these delimiters you reference the path with **dots** (`outputs.<label>.<handle>.<field>`). Spaces in a label are tolerated here, but a **dot inside a label** will be misread as a path separator — another reason to keep labels simple.

<!-- /SECTION: template-syntax -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

#### My expression returns nothing / `undefined`

- **Label mismatch.** `outputs["http request"]` does **not** match a node labelled `HTTP Request`. Capitalisation and spacing must match exactly.
- **Wrong handle.** Make sure you used the right output port (`success` vs `output` vs `error`).
- **Node hasn't run yet.** If the source node runs *after* this one, its data isn't available. Reorder the workflow, or guard with optional chaining in code fields: `outputs["My Node"]?.success?.value`.

#### `SyntaxError` in a code field

Almost always a label or field name with a space/special character accessed via dot notation. Switch to bracket-and-quote notation: `outputs["My Node"].success`.

#### My expression broke after I renamed a node

Because `outputs` is keyed by **label**, renaming a node changes its key. Update every expression that referenced the old name.

#### Smart quotes don't work

Use straight quotes (`"` or `'`) inside the brackets. Curly “smart quotes” pasted from a document will cause errors.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- [Function](./function.md) – Run custom JavaScript; the primary code-field for expressions
- [Filter](./filter.md) – Filter a list with a JavaScript condition
- [Set Variable](./set-variable.md) – Store values to reference later via `variables`
- [Switch](./switch.md) – Route data based on expression conditions

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-06-16 | Initial expressions guide. |

<!-- /SECTION: changelog -->
