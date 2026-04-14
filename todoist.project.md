---
node_id: "todoist-project"
title: "Todoist - Project"
description: "Create, read, update, archive, unarchive, and delete Todoist projects"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - project
  - productivity
related_nodes:
  - todoist-task
  - todoist-section
  - todoist-comment
  - todoist-label
  - todoist-reminder
---

<!-- SECTION: header -->

# Todoist - Project

> **Category:** Integrations | **Type:** Action Node

Manage Todoist projects with operations: `Create`, `Get`, `Get Many`, `Update`, `Archive`, `Unarchive`, `Delete`, and `Get Collaborators`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token used to authenticate requests |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Archive`, `Delete`, `Get`, `Get Collaborators`, `Get Many`, `Unarchive`, `Update` |
| `name` | `string` | Yes* | - | Project name (`Create`) |
| `nameOptional` | `string` | No | - | New project name (`Update`) |
| `color` | `enum` | No | - | One of: `red`, `green`, `yellow`, `orange` (`Create`, `Update`) |
| `isFavorite` | `boolean` | No | `false` | Mark project as favorite (`Create`, `Update`) |
| `viewStyle` | `enum` | No | `List` | One of: `List`, `Board` (`Create`, `Update`) |
| `projectId` | `string` | Yes* | - | Project ID (`Delete`, `Get`, `Archive`, `Unarchive`, `Get Collaborators`, `Update`) |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created project object |
| `Get` | Project object |
| `Get Many` | Project list |
| `Update` | Updated project object |
| `Archive` | Todoist API archive response |
| `Unarchive` | Todoist API unarchive response |
| `Delete` | Todoist API delete response |
| `Get Collaborators` | Project collaborators list |

<!-- /SECTION: configuration -->
