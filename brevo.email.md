---
node_id: "brevo-email"
title: "Brevo - Email"
description: "Send transactional emails with Brevo and fetch account info"
category: "integrations"
subcategory: "brevo"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - brevo
  - email
  - transactional
related_nodes:
  - brevo-contact
  - brevo-contact-attribute
  - brevo-sender
---

<!-- SECTION: header -->

# Brevo - Email

> **Category:** Integrations | **Type:** Action Node

Run Brevo email operations: `Send Transactional Email` and `Get Account`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `apiKey` | `string` | Yes | - | Brevo API key |
| `partnerKey` | `string` | No | - | Optional Brevo partner key |
| `operation` | `enum` | No | `Send Transactional Email` | One of: `Send Transactional Email`, `Get Account` |
| `senderEmail` | `string` | Yes* | - | Sender email address (`Send Transactional Email`) |
| `senderName` | `string` | No | - | Sender name (`Send Transactional Email`) |
| `to` | `string` | Yes* | - | Recipients list (CSV or JSON) (`Send Transactional Email`) |
| `cc` | `string` | No | - | CC recipients (`Send Transactional Email`) |
| `bcc` | `string` | No | - | BCC recipients (`Send Transactional Email`) |
| `replyToEmail` | `string` | No | - | Reply-to email (`Send Transactional Email`) |
| `replyToName` | `string` | No | - | Reply-to name (`Send Transactional Email`) |
| `mode` | `enum` | No | `content` | `content` or `template` (`Send Transactional Email`) |
| `subject` | `string` | Yes* | - | Email subject (required when no `templateId`) |
| `htmlContent` | `string` | Yes* | - | HTML body (`mode=content`, html or text required) |
| `textContent` | `string` | Yes* | - | Text body (`mode=content`, html or text required) |
| `templateId` | `number` | Yes* | - | Template ID (`mode=template`) |
| `templateParamsJson` | `string` | No | - | Template params JSON object (`mode=template`) |
| `headersJson` | `string` | No | - | Headers JSON object |
| `tags` | `string` | No | - | Tags as CSV/JSON string list |
| `attachmentsJson` | `string` | No | - | Attachments JSON array |
| `scheduledAt` | `string` | No | - | Scheduled send datetime |
| `batchId` | `string` | No | - | Batch ID |
| `debug` | `boolean` | No | `false` | Enable debug logs |

* Required only for listed operations/modes.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Send Transactional Email` | Brevo send response (for example, message ID) |
| `Get Account` | Brevo account object |

<!-- /SECTION: configuration -->
