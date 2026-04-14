---
node_id: "brevo-sender"
title: "Brevo - Sender"
description: "Create, delete, and list Brevo senders"
category: "integrations"
subcategory: "brevo"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - brevo
  - sender
  - email
related_nodes:
  - brevo-email
  - brevo-contact
  - brevo-contact-attribute
---

<!-- SECTION: header -->

# Brevo - Sender

> **Category:** Integrations | **Type:** Action Node

Manage Brevo senders with operations: `Create`, `Get Many`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `apiKey` | `string` | Yes | - | Brevo API key |
| `partnerKey` | `string` | No | - | Optional Brevo partner key |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Delete`, `Get Many` |
| `name` | `string` | Yes* | - | Sender name (`Create`) |
| `email` | `string` | Yes* | - | Sender email (`Create`) |
| `senderId` | `string` | Yes* | - | Sender ID (`Delete`) |
| `returnAll` | `boolean` | No | `false` | Return full sender list (`Get Many`) |
| `limit` | `number` | No | `10` | Max senders returned when `returnAll=false` (`Get Many`) |
| `debug` | `boolean` | No | `false` | Enable debug logs |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created sender object |
| `Get Many` | Sender array (full or limited) |
| `Delete` | `{ success: true }` on successful delete |

<!-- /SECTION: configuration -->
