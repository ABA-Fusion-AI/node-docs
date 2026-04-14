---
node_id: "todoist-label"
title: "Todoist - Label"
description: "Create, retrieve, update, list, and delete Todoist labels"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - label
  - productivity
related_nodes:
  - todoist-task
  - todoist-project
---

<!-- SECTION: header -->

# Todoist - Label

> **Category:** Integrations | **Type:** Action Node

Manage Todoist labels with operations: `Create`, `Get`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Delete`, `Get`, `Get Many`, `Update` |
| `labelId` | `string` | Yes* | - | Label ID (`Delete`, `Get`, `Update`) |
| `name` | `string` | Yes* | - | Label name (`Create`) |
| `nameOptional` | `string` | No | - | Updated label name (`Update`) |
| `color` | `enum` | No | - | One of: `red`, `green`, `yellow`, `orange` (`Create`, `Update`) |
| `isFavorite` | `boolean` | No | - | Mark label as favorite (`Create`, `Update`) |
| `order` | `number` | No | - | Label order (`Create`, `Update`) |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created label object |
| `Get` | Label object |
| `Get Many` | Labels list |
| `Update` | Updated label object |
| `Delete` | Todoist API delete response |

<!-- /SECTION: configuration -->
