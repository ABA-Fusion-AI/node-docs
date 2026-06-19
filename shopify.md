---
node_id: "shopify"
title: "Shopify Node"
description: "Manage Shopify products and orders via the Admin API"
category: "integrations"
subcategory: "shopify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-16"
author: "Fusion Team"
tags:
  - shopify
  - ecommerce
  - store
  - action
---

<!-- SECTION: header -->
# Shopify Node

> **Category:** Integrations | **Type:** Action Node

Connect to the Shopify Admin API to manage products (media, category/type, inventory) and orders with a single node.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

The **Shopify** action node lets you operate on two resources: **product** and **order**. Pick a `resourceType` and an `operation` to create, read, update, delete, or list items. For products, you can also upload local images as base64 attachments.

### Key Features

- **Products and Orders:** Manage two core Shopify resources with one node
- **Image Uploads:** Attach local images when creating or updating products
- **Simple Operations:** Standard CRUD operations plus list

### Use Cases

- Sync products from a catalog into Shopify
- Update product inventory and pricing from a workflow
- Create draft or live orders programmatically

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Connection Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `shopName` | `string` | Yes | - | Shop name (without `.myshopify.com`) |
| `accessToken` | `string` | Yes | - | Shopify access token (`shpat_...`) |

### Common Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `resourceType` | `enum` | No | `product` | `product` or `order` |
| `operation` | `enum` | No | `list` | `create`, `read`, `update`, `delete`, `list` |

### Product Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `productId` | `string` | Yes* | - | Product ID (`read`, `update`, `delete`) |
| `title` | `string` | No | - | Product title (`create`, `update`) |
| `productCategory` | `string` | No | - | Category / product type (`create`, `update`) |
| `price` | `number` | No | - | Product price (`create`, `update`) |
| `quantity` | `number` | No | - | Inventory quantity (`create`, `update`) |
| `description` | `string` | No | - | Product description (`create`, `update`) |
| `productStatus` | `enum` | No | `active` | `active`, `archived`, `draft` (`create`, `update`) |
| `imagePath` | `string` | No | - | Local image path (e.g. `/home/user/image.jpg`) (`create`, `update`) |
| `limit` | `number` | No | `50` | List limit (`list`) |

### Order Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `orderId` | `string` | Yes* | - | Order ID (`read`, `update`, `delete`) |
| `productIds` | `string[]` | No | - | Product IDs list (`create`, `update`) |
| `customerEmail` | `string` | No | - | Customer email (`create`, `update`) |
| `note` | `string` | No | - | Order note (`create`, `update`) |
| `tags` | `string` | No | - | Order tags (`create`, `update`) |

*Required only for the listed operation and resource type.

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
| `output` | `object` | Shopify API response for the executed operation |
| `error` | `Error` | Emitted if the operation or connection fails |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Example: Create a Product With Image

```json
{
  "shopName": "my-store",
  "accessToken": "shpat_...",
  "resourceType": "product",
  "operation": "create",
  "title": "Product1",
  "description": "Undated planner with hardcover.",
  "productCategory": "Stationery",
  "price": 19,
  "quantity": 50,
  "imagePath": "/home/user/images/image.jpg"
}
```

### Example: List Products

```json
{
  "shopName": "my-store",
  "accessToken": "shpat_...",
  "resourceType": "product",
  "operation": "list",
  "limit": 25
}
```

### Example: Create an Order From Product IDs

```json
{
  "shopName": "my-store",
  "accessToken": "shpat_...",
  "resourceType": "order",
  "operation": "create",
  "productIds": ["1234567890", "2345678901"],
  "customerEmail": "buyer@example.com",
  "note": "First-time customer",
  "tags": "web,automation"
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### API Client not initialized

**Cause:** Missing or invalid `shopName` or `accessToken`.

**Solution:** Verify credentials and try again.

#### File not found at path

**Cause:** `imagePath` points to a missing file.

**Solution:** Provide a valid absolute path to an existing image.

#### Shopify API Error

**Cause:** Shopify API returned an error response.

**Solution:** Check permissions, resource IDs, and required fields for the selected operation.

#### Order delete not available

**Cause:** The current implementation only lists or reads orders for non-create operations.

**Solution:** Use Shopify Admin API directly for delete, or adjust the node implementation.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-16 | Initial release |

<!-- /SECTION: changelog -->
