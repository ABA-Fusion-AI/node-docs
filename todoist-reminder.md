---
node_id: "todoist-reminder"
title: "Todoist - Reminder"
description: "Create, update, list, and delete Todoist reminders with absolute or relative schedules"
category: "integrations"
subcategory: "todoist"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - integration
  - todoist
  - reminder
  - productivity
related_nodes:
  - todoist-task
  - todoist-project
---

<!-- SECTION: header -->

# Todoist - Reminder

> **Category:** Integrations | **Type:** Action Node

Manage Todoist reminders with operations: `Create`, `Get Many`, `Update`, and `Delete`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --------- | ---- | -------- | ------- | ----------- |
| `token` | `string` | Yes | - | Todoist API token |
| `operation` | `enum` | No | `Create` | One of: `Create`, `Delete`, `Get Many`, `Update` |
| `taskId` | `string` | Yes* | - | Task ID (`Create`) |
| `reminderId` | `string` | Yes* | - | Reminder ID (`Delete`, `Update`) |
| `reminderType` | `enum` | No | `Absolute` | `Absolute` or `Relative` (`Create`, `Update`) |
| `dueDateType` | `enum` | No | `Natural Language` | Absolute due format: `Natural Language`, `Full-Day Date`, `Floating Date With Time`, `Fixed Timezone Date With Time` |
| `naturalLanguageRepresentation` | `string` | Yes* | - | Natural language due text (absolute + natural language mode) |
| `fullDayDate` | `date` | Yes* | - | Date (`YYYY-MM-DD` or `MM/DD/YYYY`) (absolute + full-day mode) |
| `floatingDateWithTime` | `datetime` | Yes* | - | Datetime value (absolute + floating/fixed-timezone mode) |
| `timezone` | `string` | Yes* | - | IANA timezone (absolute + fixed-timezone mode) |
| `minuteOffset` | `number` | Yes* | - | Minutes before task due time (relative mode) |
| `deliveryService` | `enum` | No | - | `Push` or `Email` (`Create`, `Update`) |
| `notifyUserId` | `string` | No | - | Optional user ID to notify (`Create`, `Update`) |
| `isUrgent` | `boolean` | No | - | Urgent reminder flag (`Create`, `Update`) |
| `getManyTaskId` | `string` | No | - | Filter reminders by task ID (`Get Many`) |
| `getManyCursor` | `string` | No | - | Pagination cursor (`Get Many`) |
| `getManyLimit` | `number` | No | - | Maximum reminders from `1` to `200` (`Get Many`) |
| `dueLang` | `string` | No | - | Language code for natural-language due parsing (absolute + natural language mode) |

* Required only for listed operations/modes.

### Outputs

| Operation | Output |
| --------- | ------ |
| `Create` | Created reminder object |
| `Get Many` | Reminder list (with pagination cursor when applicable) |
| `Update` | Updated reminder object |
| `Delete` | Todoist API delete response |

<!-- /SECTION: configuration -->
