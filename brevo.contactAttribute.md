---
node_id: "brevo-contact-attribute"
title: "Brevo - Contact Attribute"
description: "Manage Brevo contact attributes"
category: "integrations"
subcategory: "brevo"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - brevo
  - contact-attribute
  - crm
related_nodes:
  - brevo-contact
  - brevo-email
  - brevo-sender
---

<!-- SECTION: header -->

# Brevo - Contact Attribute

> **Category:** Integrations | **Type:** Action Node

Manage Brevo contact attributes with operations: `Create`, `Get`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `apiKey` | `string` | Yes | - | Brevo API key |
| `partnerKey` | `string` | No | - | Optional Brevo partner key |
| `operation` | `enum` | No | `Get Many` | One of: `Create`, `Delete`, `Get`, `Get Many`, `Update` |
| `attributeCategory` | `enum` | Yes* | - | One of: `normal`, `transactional`, `category`, `calculated`, `global` (`Create`, `Delete`, `Get`, `Update`) |
| `attributeName` | `string` | Yes* | - | Attribute name such as `FIRSTNAME` (`Create`, `Delete`, `Get`, `Update`) |
| `attributeType` | `enum` | Yes* | - | One of: `text`, `date`, `float`, `boolean`, `id`, `category`, `user`, `multiple-choice` (`Create`) |
| `attributeValue` | `string` | Yes* | - | Value used for `calculated` and `global` categories (`Create`, `Update`) |
| `multiCategoryOptions` | `string[]` | No | `[]` | Options for `normal` + `multiple-choice` attributes |
| `isRecurring` | `boolean` | No | - | Recurring flag for `calculated` or `global` (`Create`) |
| `enumeration` | `array<object>` | No | `[]` | Enumeration values as `{ value, label }`, mainly for `category` |
| `payloadJson` | `string` | No | - | Raw JSON object merged into API payload (`Create`, `Update`) |
| `debug` | `boolean` | No | `false` | Enable debug logs |

* Required only for listed operations/categories.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created attribute response |
| `Get` | Matching attribute object |
| `Get Many` | Attribute groups/list response |
| `Update` | Updated attribute response |
| `Delete` | Delete response |

### Category Rules

| Category | Create Requirements | Update Fields |
| -------- | ------------------- | ------------- |
| `normal` | `attributeType` required | `multiCategoryOptions` |
| `transactional` | `attributeType` required | No dedicated update fields |
| `category` | `attributeType` required, optional `enumeration` | `enumeration` |
| `calculated` | `attributeValue` required, optional `isRecurring` | `attributeValue` |
| `global` | `attributeValue` required, optional `isRecurring` | `attributeValue` |

<!-- /SECTION: configuration -->
