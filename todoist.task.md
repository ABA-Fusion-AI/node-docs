---
node_id: "todoist-task"
title: "Todoist - Task"
description: "Create, retrieve, move, update, complete, reopen, and delete Todoist tasks"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - task
  - productivity
related_nodes:
  - todoist-project
  - todoist-section
  - todoist-comment
  - todoist-label
  - todoist-reminder
---

<!-- SECTION: header -->

# Todoist - Task

> **Category:** Integrations | **Type:** Action Node

Run task operations in Todoist: `Create`, `Get`, `Get Many`, `Update`, `Delete`, `Close`, `Reopen`, `Move`, and `Quick Add`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token |
| `operation` | `enum` | No | `Create` | One of: `Close`, `Create`, `Delete`, `Get`, `Get Many`, `Move`, `Quick Add`, `Reopen`, `Update` |
| `taskId` | `string` | Yes* | - | Existing task ID (`Close`, `Delete`, `Get`, `Move`, `Reopen`, `Update`) |
| `content` | `string` | Yes* | - | Task content/title (`Create`) |
| `contentOptional` | `string` | No | - | Updated task content (`Update`) |
| `projectId` | `string` | Yes* | - | Target project ID (`Create`) |
| `labelNames` | `string` | No | - | Comma-separated label names (`Create`, `Update`) |
| `additionalFields` | `enum` | No | `Hide` | Show advanced fields for `Create` and `Update` |
| `description` | `string` | No | - | Task description (`Create`, `Update`, advanced) |
| `dueDateTime` | `datetime` | No | - | Due datetime (`Create`, `Update`, advanced) |
| `dueString` | `string` | No | - | Natural language due text (`Create`, `Update`, advanced) |
| `dueStringLocale` | `string` | No | - | Due text locale code (`Create`, `Update`, advanced) |
| `parentID` | `string` | No | - | Parent task ID (`Create`, advanced) |
| `sectionID` | `string` | No | - | Section ID (`Create`, advanced) |
| `order` | `number` | No | - | Task order (`Create`, `Update`, advanced) |
| `priority` | `number` | No | - | Priority from `1` to `4` (`Create`, `Update`, advanced) |
| `dueDate` | `date` | No | - | Due date (`YYYY-MM-DD` or `MM/DD/YYYY`) (`Create`, `Update`, advanced) |
| `assigneeID` | `string` | No | - | Assignee user ID (`Create`, `Update`, advanced) |
| `duration` | `number` | No | - | Duration value (`Create`, `Update`, advanced) |
| `durationUnit` | `enum` | No | - | `Minute` or `Day` (`Create`, `Update`, advanced) |
| `deadlineDate` | `date` | No | - | Deadline date (`Create`, `Update`, advanced) |
| `getManyProjectId` | `string` | No | - | Filter tasks by project (`Get Many`) |
| `getManySectionId` | `string` | No | - | Filter tasks by section (`Get Many`) |
| `getManyParentId` | `string` | No | - | Filter tasks by parent (`Get Many`) |
| `getManyLabel` | `string` | No | - | Filter tasks by label name (`Get Many`) |
| `getManyTaskIds` | `string` | No | - | Comma-separated task IDs (`Get Many`) |
| `getManyCursor` | `string` | No | - | Pagination cursor (`Get Many`) |
| `getManyLimit` | `number` | No | - | Max items from `1` to `200` (`Get Many`) |
| `moveTo` | `enum` | Yes* | - | Move target type: `Project`, `Section`, `Parent` (`Move`) |
| `targetProjectId` | `string` | Yes* | - | Target project ID (`Move` when `moveTo=Project`) |
| `targetSectionId` | `string` | Yes* | - | Target section ID (`Move` when `moveTo=Section`) |
| `targetParentId` | `string` | Yes* | - | Target parent task ID (`Move` when `moveTo=Parent`) |
| `quickAddText` | `string` | Yes* | - | Todoist quick-add text (`Quick Add`) |
| `quickAddNote` | `string` | No | - | Optional note (`Quick Add`) |
| `quickAddReminder` | `string` | No | - | Optional reminder text (`Quick Add`) |
| `quickAddAutoReminder` | `boolean` | No | `false` | Auto reminder flag (`Quick Add`) |

* Required only for listed operations.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created task object |
| `Get` | Task object |
| `Get Many` | Task list (with pagination cursor when applicable) |
| `Update` | Updated task object |
| `Delete` | Todoist API delete response |
| `Close` | Todoist API close response |
| `Reopen` | Todoist API reopen response |
| `Move` | Todoist API move response |
| `Quick Add` | Quick-added task summary/object |

<!-- /SECTION: configuration -->
