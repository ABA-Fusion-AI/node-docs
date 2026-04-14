---
node_id: "todoist-section"
title: "Todoist - Section"
description: "Create, retrieve, update, list, and delete Todoist sections"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - section
  - productivity
related_nodes:
  - todoist-project
  - todoist-task
---

<!-- SECTION: header -->

# Todoist - Section

> **Category:** Integrations | **Type:** Action Node

Manage Todoist sections with operations: `Create`, `Get`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Delete`, `Get`, `Get Many`, `Update` |
| `projectId` | `string` | Yes* | - | Project ID (`Create`) |
| `sectionId` | `string` | Yes* | - | Section ID (`Get`, `Delete`, `Update`) |
| `name` | `string` | Yes* | - | Section name (`Create`, `Update`) |
| `order` | `string` | No | - | Section order (`Create`) |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created section object |
| `Get` | Section object |
| `Get Many` | Sections list |
| `Update` | Updated section object |
| `Delete` | Todoist API delete response |

<!-- /SECTION: configuration -->
