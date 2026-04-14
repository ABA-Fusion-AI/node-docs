---
node_id: "todoist-comment"
title: "Todoist - Comment"
description: "Create, retrieve, update, list, and delete Todoist comments"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - comment
  - productivity
related_nodes:
  - todoist-task
  - todoist-project
---

<!-- SECTION: header -->

# Todoist - Comment

> **Category:** Integrations | **Type:** Action Node

Manage Todoist comments with operations: `Create`, `Get`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Delete`, `Get`, `Get Many`, `Update` |
| `taskId` | `string` | Yes* | - | Task ID to attach a comment (`Create`) |
| `projectId` | `string` | No | - | Project ID helper field (available with `Get Many`) |
| `commentId` | `string` | Yes* | - | Comment ID (`Get`, `Delete`, `Update`) |
| `filters` | `enum` | No | - | `By Task`, `By Project`, `By Project and By Task` (`Get Many`) |
| `filterTaskId` | `string` | No | - | Task ID filter (`Get Many`) |
| `filterProjectId` | `string` | No | - | Project ID filter (`Get Many`) |
| `content` | `string` | Yes* | - | Comment content (`Create`, `Update`) |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created comment object |
| `Get` | Comment object |
| `Get Many` | Comments list |
| `Update` | Updated comment object |
| `Delete` | Todoist API delete response |

<!-- /SECTION: configuration -->
