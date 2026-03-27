---
node_id: "mongodb-action"
title: "MongoDB Action"
description: "Execute MongoDB operations — find, insert, update, delete, aggregate, and count documents"
category: "data"
subcategory: "databases"
version: "1.1.0"
language: "en"
last_updated: "2026-03-27"
author: "Fusion Team"
tags:
  - mongodb
  - database
  - nosql
  - document
  - action
related_nodes:
  - mysql-action
  - redis-action
  - function
---

<!-- SECTION: header -->
# MongoDB Action

> **Category:** Data | **Type:** Action Node

Connect to a MongoDB database and execute document operations — query, insert, update, delete, aggregate, or count documents in a specified collection.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **MongoDB Action** node connects to a MongoDB instance using a connection URI and performs one of ten available operations on a target collection. It supports MongoDB's native filter and projection syntax via JSON, making it suitable for flexible document management workflows.

### Key Features

- **Ten Operations:** Find, Find One, Insert One, Insert Many, Update One, Update Many, Delete One, Delete Many, Aggregate, Count
- **Native Filter Syntax:** Use standard MongoDB query operators (`$eq`, `$gt`, `$in`, etc.) in JSON format
- **Projection Support:** Control which fields are returned in query results
- **Upsert Mode:** Insert or update in a single step with `upsert: true`
- **Aggregation Pipelines:** Run multi-stage transformations directly on the collection
- **Key-Value Record Input:** Insert and update data are defined as key-value objects (`Record<string, unknown>`), with full expression support for dynamic variables from previous nodes
- **Expression Support:** All parameter fields support dynamic expressions (`metadata: { expression: true }`), enabling data to flow from upstream nodes

### Use Cases

- Store structured or semi-structured data from webhooks or APIs
- Query product catalogs, user profiles, or event logs
- Update user settings based on workflow inputs
- Remove expired sessions or outdated records
- Run analytics aggregations without leaving the workflow
- Count documents matching a condition as a workflow decision point

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `uri` | `string` | ✅ Yes | — | MongoDB connection URI (e.g. `mongodb://user:pass@host:27017`) |
| `database` | `string` | ✅ Yes | — | Target database name |
| `collection` | `string` | ✅ Yes | — | Target collection name |

### Common Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `operation` | `enum` | ✅ Yes | Operation to execute (see below) |

### Available Operations

| Operation | Description |
|-----------|-------------|
| `Find` | Query multiple documents |
| `Find One` | Retrieve a single matching document |
| `Insert One` | Insert a single document |
| `Insert Many` | Insert multiple documents in one call |
| `Update One` | Update the first matching document |
| `Update Many` | Update all matching documents |
| `Delete One` | Delete the first matching document |
| `Delete Many` | Delete all matching documents |
| `Aggregate` | Execute an aggregation pipeline |
| `Count` | Count documents matching a filter |

---

### Operation: Find

> UI label: **Parameters**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `findParams.filter` | `JSON object` | ❌ No | MongoDB filter (defaults to `{}` — all documents). Supports expressions |
| `findParams.projection` | `JSON object` | ❌ No | Fields to include or exclude. Supports expressions |
| `findParams.sort` | `JSON object` | ❌ No | Sort order (e.g. `{ "createdAt": -1 }`). Supports expressions |
| `findParams.limit` | `number` | ❌ No | Maximum number of documents to return. Supports expressions |
| `findParams.skip` | `number` | ❌ No | Number of documents to skip (pagination). Supports expressions |

### Operation: Find One

> UI label: **Parameters**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `findOneParams.filter` | `JSON object` | ✅ Yes | MongoDB filter to identify the document. Supports expressions |
| `findOneParams.projection` | `JSON object` | ❌ No | Fields to include or exclude. Supports expressions |

### Operation: Insert One

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `insertOneData` | `Record<string, unknown>` | ✅ Yes | Document as a key-value object (e.g. `{ "name": "Alice", "age": 30 }`). Supports expressions for dynamic values from previous nodes. UI label: **Data** |

### Operation: Insert Many

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `insertManyData` | `Array<Record<string, unknown>>` | ✅ Yes | Array of documents, each as a key-value object (e.g. `[{ "name": "Alice" }, { "name": "Bob" }]`). Supports expressions. UI label: **Data** |

### Operation: Update One / Update Many

> UI label: **Filter & Data**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `updateOneParams.filter` | `JSON object` | ✅ Yes | Filter to identify document(s). Supports expressions |
| `updateOneParams.update` | `Record<string, unknown>` | ✅ Yes | Fields to update as a key-value object (e.g. `{ "status": "active" }`). Applied via `$set`. Supports expressions. UI label: **Data** |
| `updateOneParams.upsert` | `boolean` | ❌ No | Insert document if no match is found (default: `false`) |

> `updateManyParams` uses the same structure as `updateOneParams`.

### Operation: Delete One / Delete Many

> UI label: **Delete Parameters**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `deleteOneParams.filter` | `JSON object` | ✅ Yes | Filter to identify document(s) to delete. Supports expressions |

> `deleteManyParams` uses the same structure as `deleteOneParams`.

### Operation: Aggregate

> UI label: **Aggregate Parameters**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `aggregateParams.pipeline` | `JSON array` | ✅ Yes | Aggregation pipeline stages (e.g. `[{ "$match": {...} }, { "$group": {...} }]`). Supports expressions |

### Operation: Count

> UI label: **Count Parameters**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `countParams.filter` | `JSON object` | ❌ No | Filter to count matching documents (defaults to `{}`). Supports expressions |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

| Input | Type | Description |
|-------|------|-------------|
| `input` | `any` | Incoming workflow data (usable via expressions in field values) |

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `output` | `object` | Result of the executed operation |
| `error` | `Error` | Emitted if the operation or connection fails |

### Output Schemas by Operation

**`Find`**
```json
{
  "count": 2,
  "documents": [
    { "_id": "664a1f...", "name": "Alice", "status": "active" },
    { "_id": "664a20...", "name": "Bob", "status": "active" }
  ]
}
```

**`Find One`**
```json
{
  "document": { "_id": "664a1f...", "name": "Alice", "email": "alice@example.com" }
}
```

> Returns `{ "document": null }` if no document matches the filter.

**`Insert One`**
```json
{
  "operation": "inserted",
  "insertedId": "664b3c...",
  "acknowledged": true
}
```

**`Insert Many`**
```json
{
  "operation": "inserted",
  "insertedCount": 3,
  "insertedIds": ["664b3c...", "664b3d...", "664b3e..."],
  "acknowledged": true
}
```

**`Update One` / `Update Many`**
```json
{
  "operation": "updated",
  "matchedCount": 1,
  "modifiedCount": 1,
  "upsertedId": null,
  "acknowledged": true
}
```

**`Delete One` / `Delete Many`**
```json
{
  "operation": "deleted",
  "deletedCount": 1,
  "acknowledged": true
}
```

**`Aggregate`**
```json
{
  "count": 3,
  "documents": [
    { "_id": "active", "total": 42 },
    { "_id": "inactive", "total": 8 },
    { "_id": "pending", "total": 5 }
  ]
}
```

**`Count`**
```json
{
  "count": 47
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Find Documents with a Filter

Query all active users, sorted by creation date, limited to 50.

**Configuration:**
```json
{
  "uri": "mongodb://user:pass@localhost:27017",
  "database": "myapp",
  "collection": "users",
  "operation": "Find",
  "findParams": {
    "filter": { "status": "active" },
    "projection": { "name": 1, "email": 1, "_id": 0 },
    "sort": { "createdAt": -1 },
    "limit": 50
  }
}
```

**Output:**
```json
{
  "count": 2,
  "documents": [
    { "name": "Carol", "email": "carol@example.com" },
    { "name": "Alice", "email": "alice@example.com" }
  ]
}
```

---

### Example: Find One by ID

Retrieve a single document by its `_id`.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "myapp",
  "collection": "orders",
  "operation": "Find One",
  "findOneParams": {
    "filter": { "_id": "objectid(664a1f2b3c4d5e6f7a8b9c0d)" }
  }
}
```

---

### Example: Insert One Document

Insert a new event log entry.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "logs",
  "collection": "events",
  "operation": "Insert One",
  "insertOneData": {
    "type": "user_login",
    "userId": "{{outputs.Function.success.id}}",
    "ip": "{{outputs.Function.success.ip}}",
    "timestamp": "{{new Date().toISOString()}}"
  }
}
```

**Output:**
```json
{
  "operation": "inserted",
  "insertedId": "664b3c9a...",
  "acknowledged": true
}
```

---

### Example: Update One with Upsert

Update a user's last seen timestamp, creating the document if it does not exist.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "myapp",
  "collection": "sessions",
  "operation": "Update One",
  "updateOneParams": {
    "filter": { "userId": "{{outputs.Function.success.userId}}" },
    "update": {
      "lastSeen": "{{new Date().toISOString()}}",
      "active": true
    },
    "upsert": true
  }
}
```

---

### Example: Delete Many Expired Documents

Delete all expired sessions.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "myapp",
  "collection": "sessions",
  "operation": "Delete Many",
  "deleteManyParams": {
    "filter": { "expiresAt": { "$lt": "{{new Date().toISOString()}}" } }
  }
}
```

---

### Example: Aggregation Pipeline

Group orders by status and compute the total amount per status.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "myapp",
  "collection": "orders",
  "operation": "Aggregate",
  "aggregateParams": {
    "pipeline": [
      { "$match": { "createdAt": { "$gte": "2026-01-01" } } },
      { "$group": { "_id": "$status", "total": { "$sum": "$amount" }, "count": { "$sum": 1 } } },
      { "$sort": { "total": -1 } }
    ]
  }
}
```

**Output:**
```json
{
  "count": 3,
  "documents": [
    { "_id": "shipped", "total": 14520.50, "count": 48 },
    { "_id": "pending", "total": 3200.00, "count": 12 },
    { "_id": "cancelled", "total": 800.00, "count": 5 }
  ]
}
```

---

### Example: Count Documents

Count the number of pending orders.

**Configuration:**
```json
{
  "uri": "mongodb://localhost:27017",
  "database": "myapp",
  "collection": "orders",
  "operation": "Count",
  "countParams": {
    "filter": { "status": "pending" }
  }
}
```

**Output:**
```json
{
  "count": 12
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Ingest and Store API Data

Fetch data from an external API and store each item as a MongoDB document.

```json
{
  "nodes": [
    {
      "id": "fetch-api",
      "type": "http-request",
      "position": { "x": 100, "y": 100 },
      "config": {
        "url": "https://api.example.com/products",
        "method": "GET"
      }
    },
    {
      "id": "transform",
      "type": "function",
      "position": { "x": 300, "y": 100 },
      "config": {
        "code": "return input.data.map(p => ({ sku: p.sku, name: p.name, price: p.price }));"
      }
    },
    {
      "id": "store",
      "type": "mongodb-action",
      "position": { "x": 500, "y": 100 },
      "config": {
        "uri": "mongodb://localhost:27017",
        "database": "catalog",
        "collection": "products",
        "operation": "Insert Many",
        "insertManyData": "{{input}}"
      }
    }
  ]
}
```

### Common Patterns

- **Event Logging:** Trigger → MongoDB Insert One (append event log)
- **Data Sync:** Cron → HTTP Fetch → MongoDB Insert Many (bulk ingest)
- **Analytics:** Cron → MongoDB Aggregate → Send Report
- **Cleanup:** Cron → MongoDB Delete Many (WHERE expiry condition)
- **Cache Population:** MongoDB Find → Redis Action setKey (warm cache)

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### Connection timeout

**Cause:** The MongoDB URI is incorrect, the server is unreachable, or the network blocks the connection.

**Solution:** Verify the `uri` format (`mongodb://user:pass@host:port`). Check that the host is reachable from the workflow engine and that port `27017` is open in the firewall. The node has a 10-second connection timeout by default.

#### `filter is required` error

**Cause:** A required `filter` field was left empty for operations that mandate it (`Find One`, `Update One`, `Update Many`, `Delete One`, `Delete Many`).

**Solution:** Provide a valid JSON filter object. Use `{}` to match all documents (use with care on delete operations).

#### `pipeline must be a JSON array` error

**Cause:** The `aggregateParams.pipeline` value is not a valid JSON array.

**Solution:** Ensure the pipeline is a properly formatted JSON array of stage objects, e.g. `[{ "$match": { ... } }]`.

#### `document` is `null` in Find One output

**Cause:** No document matched the provided filter.

**Solution:** Check your filter conditions. Use a Function node after Find One to handle the null case gracefully.

#### `insertOneData must be a record` error

**Cause:** The `insertOneData` field was not provided as a valid key-value object.

**Solution:** Provide `insertOneData` as a key-value object: `{ "name": "Alice", "email": "alice@example.com" }`.

#### Dynamic variables insert `undefined` values

**Cause:** The field does not have expression mode enabled, preventing dynamic data from previous nodes from being resolved.

**Solution:** Ensure that the field has `expression: true` in its metadata configuration. In the UI, verify that the expression toggle is enabled for fields that receive data from upstream nodes. All Insert and Update data fields now use `Record<string, unknown>` format with expression support enabled by default.

### Error Codes

| Code | Message | Solution |
|------|---------|----------|
| `MongoServerError` | Authentication failed | Check credentials in the `uri` |
| `MongoNetworkError` | Connection refused / timeout | Verify host, port, and firewall |
| `MongoParseError` | Invalid connection string | Check `uri` format |
| `BSONError` | Invalid BSON type | Verify filter / pipeline JSON structure |

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- [MySQL Action](./mysql.md) - Relational SQL database operations
- [Redis Action](./redis-action.md) - Key-value cache operations
- [Function](./function.md) - Transform data before inserting into MongoDB

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.1.0 | 2026-03-27 | **Breaking:** Renamed `insertOneParams` → `insertOneData` (key-value object instead of fieldName/fieldValue array). Renamed `insertManyParams` → `insertManyData` (array of key-value objects). Changed `updateOneParams.update` and `updateManyParams.update` from fieldName/fieldValue arrays to key-value objects (`Record<string, unknown>`). Added `expression: true` metadata on all parameter fields to support dynamic variables from upstream nodes. Fixed bug where dynamic variables resolved to `undefined` in Update One operations. |
| 1.0.0 | 2026-03-10 | Initial release |

<!-- /SECTION: changelog -->
