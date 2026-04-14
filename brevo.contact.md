---
node_id: "brevo-contact"
title: "Brevo - Contact"
description: "Create, update, fetch, and delete Brevo contacts"
category: "integrations"
subcategory: "brevo"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - brevo
  - contact
  - crm
related_nodes:
  - brevo-contact-attribute
  - brevo-email
  - brevo-sender
---

<!-- SECTION: header -->

# Brevo - Contact

> **Category:** Integrations | **Type:** Action Node

Manage Brevo contacts with operations: `Create`, `Create or Update`, `Get`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `apiKey` | `string` | Yes | - | Brevo API key |
| `partnerKey` | `string` | No | - | Optional Brevo partner key |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Create or Update`, `Delete`, `Get`, `Get Many`, `Update` |
| `email` | `string` | Yes* | - | Contact email (`Create`, `Create or Update`) |
| `extId` | `string` | Yes* | - | External contact ID (`Create`, `Create or Update`) |
| `contactIdentifier` | `string` | Yes* | - | Contact email, Brevo ID, or ext_id (`Delete`, `Get`, `Update`) |
| `attributes` | `array<object>` | No | `[]` | Custom attributes as `{ fieldname, fieldvalue }` (`Create`, `Create or Update`, `Update`) |
| `listIds` | `string` | No | - | Comma-separated list IDs (`Create`, `Create or Update`, `Update`) |
| `emailBlacklisted` | `boolean` | No | - | Email blacklist status (`Create`, `Create or Update`, `Update`) |
| `smsBlacklisted` | `boolean` | No | - | SMS blacklist status (`Create`, `Create or Update`, `Update`) |
| `updateEnabled` | `boolean` | No | `true` | Upsert behavior for `Create or Update` |
| `limit` | `number` | No | `50` | Number of contacts to return (`Get Many`) |
| `offset` | `number` | No | `0` | Offset for pagination (`Get Many`) |
| `sort` | `enum` | No | `desc` | `asc` or `desc` (`Get Many`) |
| `modifiedSince` | `string` | No | - | ISO datetime filter (`Get Many`) |
| `debug` | `boolean` | No | `false` | Enable debug logs |

* For `Create` and `Create or Update`, at least one of `email` or `extId` is required.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created contact response |
| `Create or Update` | Upsert contact response |
| `Get` | Contact object |
| `Get Many` | Paginated contact list |
| `Update` | Updated contact response |
| `Delete` | Delete response |

<!-- /SECTION: configuration -->
