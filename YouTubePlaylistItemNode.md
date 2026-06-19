---
node_id: "YouTubePlaylistItemNode"
title: "YouTube Playlist Item"
description: "Add, retrieve or remove items from a playlist"
category: "actions"
subcategory: "youtube"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - youtube
  - playlist
  - item
  - oauth2
related_nodes:
  - YouTubeChannelNode
  - YouTubePlaylistNode
  - YouTubeVideoNode
  - YouTubeVideoCategoryNode
---

<!-- SECTION: header -->

# YouTube Playlist Item

> **Category:** Actions | **Type:** Action Node

Playlist item operations: `Add an item to a playlist`, `Delete an item from a playlist`, `Get a playlist's item`, `Retrieve all playlist items`.

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
| `operation` | `enum` | Yes | - | `Add an item to a playlist`, `Delete an item from a playlist`, `Get a playlist's item`, `Retrieve all playlist items` |
| `playlistId` | `string` | Yes* | - | Playlist ID (all operations) |
| `videoId` | `string` | Yes* | - | Video ID (`Add`, `Get`, `Delete`) |
| `limit` | `number` | No | `50` | Max results (1-50) (`Retrieve all playlist items`) |

*Required only for the listed operation.

### Outputs

| Operation | Output |
| --- | --- |
| `Add an item to a playlist` | `{ success: true, playlistId, videoId }` |
| `Delete an item from a playlist` | `{ success: true, deletedPlaylistItemId }` |
| `Get a playlist's item` | Playlist item response |
| `Retrieve all playlist items` | Playlist item list response |

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
| `success` | `object` | API response or operation result |
| `error` | `Error` | Thrown on validation or API failures |

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: troubleshooting -->

## Troubleshooting

### Common Issues

#### playlistId is required

**Cause:** Operation requires `playlistId`.

**Solution:** Provide a valid playlist ID.

#### videoId is required

**Cause:** Operation requires `videoId`.

**Solution:** Provide a valid video ID.

#### Video not found in playlist

**Cause:** The video is not part of the playlist.

**Solution:** Confirm the video exists in the playlist before deleting.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->

## Related

- YouTube Channel
- YouTube Playlist
- YouTube Video
- YouTube Video Category

<!-- /SECTION: related -->
