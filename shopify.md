---
node_id: "shopify"
title: "Shopify Node"
description: "Interact with Shopify API to manage your store"
category: "actions"
subcategory: "shopify"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - shopify
  - ecommerce
  - store
  - action
related_nodes: []
---

<!-- SECTION: header -->

# Shopify Node

> **Category:** Actions | **Type:** Action Node

Resource operations for `product` and `order` with `create`, `read`, `update`, `delete`, `list`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `shopName` | `string` | Yes | - | Shop name (without `.myshopify.com`) |
| `accessToken` | `string` | Yes | - | Shopify access token (`shpat_...`) |
| `resourceType` | `enum` | No | `product` | `product` or `order` |
| `operation` | `enum` | No | `list` | `create`, `read`, `update`, `delete`, `list` |
| `productId` | `string` | Yes* | - | Product ID (`read`, `update`, `delete`) |
| `title` | `string` | No | - | Product title (`create`, `update`) |
| `productCategory` | `string` | No | - | Category / Product type (`create`, `update`) |
| `price` | `number` | No | - | Product price (`create`, `update`) |
| `quantity` | `number` | No | - | Inventory quantity (`create`, `update`) |
| `description` | `string` | No | - | Product description (`create`, `update`) |
| `productStatus` | `enum` | No | `active` | `active`, `archived`, `draft` (`create`, `update`) |
| `imagePath` | `string` | No | - | Local image path for product media (`create`, `update`) |
| `limit` | `number` | No | `50` | List limit (`list` products) |
| `orderId` | `string` | Yes* | - | Order ID (`read`, `update`, `delete`) |
| `productIds` | `string[]` | No | - | Product IDs list (`create`, `update` orders) |
| `customerEmail` | `string` | No | - | Customer email (`create`, `update` orders) |
| `note` | `string` | No | - | Order note (`create`, `update` orders) |
| `tags` | `string` | No | - | Order tags (`create`, `update` orders) |

*Required only for the listed operation and resource type.

### Outputs

| Operation | Output |
| --- | --- |
| `create` | Created resource response |
| `read` | Resource response |
| `update` | Updated resource response |
| `delete` | Delete response |
| `list` | List response |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `none` | Uses configuration only; incoming input is ignored |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `object` | Shopify API response for the operation |
| `error` | `Error` | Thrown when validation or API calls fail |

<!-- /SECTION: inputs-outputs -->

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

<!-- /SECTION: troubleshooting -->

---
