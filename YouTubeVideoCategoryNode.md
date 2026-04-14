---
node_id: "YouTubeVideoCategoryNode"
title: "YouTube Video Category"
description: "Retrieve all available YouTube video categories for a selected region"
category: "actions"
subcategory: "youtube"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - youtube
  - categories
  - region
  - oauth2
related_nodes:
  - YouTubeChannelNode
  - YouTubePlaylistNode
  - YouTubePlaylistItemNode
  - YouTubeVideoNode
---

<!-- SECTION: header -->

# YouTube Video Category

> **Category:** Actions | **Type:** Action Node

Category operation: `Retrieve all video categories`.

<!-- /SECTION: header -->

---

<!-- SECTION: configuration -->

## Configuration

### Parameters

| Parameter | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `clientId` | `string` | Yes | - | Google Client ID |
| `clientSecret` | `string` | Yes | - | Google Client Secret |
| `refreshToken` | `string` | Yes | - | OAuth2 refresh token |
| `operation` | `enum` | Yes | `Retrieve all video categories` | Only supported operation |
| `region` | `enum` | Yes | `United States` | Region label mapped to ISO-3166-1 alpha-2 |

### Outputs

| Operation | Output |
| --- | --- |
| `Retrieve all video categories` | `{ region, regionCode, items }` |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->

## Inputs & Outputs

### Inputs

| Input | Type | Description |
| --- | --- | --- |
| `input` | `none` | Uses configuration only; incoming input is ignored |

### Outputs

| Output | Type | Description |
| --- | --- | --- |
| `success` | `object` | API response with region and categories |
| `error` | `Error` | Thrown on validation or API failures |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### Unknown region

**Cause:** Region label is not in the supported list.

**Solution:** Choose a valid region label from the dropdown.

#### API Client not initialized

**Cause:** OAuth setup failed.

**Solution:** Verify OAuth credentials and retry.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->

## Related

- YouTube Channel
- YouTube Playlist
- YouTube Playlist Item
- YouTube Video

<!-- /SECTION: related -->
