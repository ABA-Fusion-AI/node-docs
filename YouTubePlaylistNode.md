---
node_id: "YouTubePlaylistNode"
title: "YouTube Playlist"
description: "Create, retrieve, update or delete playlists"
category: "actions"
subcategory: "youtube"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - youtube
  - playlist
  - oauth2
related_nodes:
  - YouTubeChannelNode
  - YouTubePlaylistItemNode
  - YouTubeVideoNode
  - YouTubeVideoCategoryNode
---

<!-- SECTION: header -->

# YouTube Playlist

> **Category:** Actions | **Type:** Action Node

Playlist operations: `Create a playlist`, `Delete a playlist`, `Get a playlist`, `Retrieve all playlists`, `Update a playlist`.

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
| `operation` | `enum` | Yes | `Retrieve all playlists` | `Create a playlist`, `Delete a playlist`, `Get a playlist`, `Retrieve all playlists`, `Update a playlist` |
| `playlistId` | `string` | Yes* | - | Playlist ID (`Get`, `Update`, `Delete`) |
| `limit` | `number` | No | `10` | Max results (1-50) (`Retrieve all playlists`) |
| `title` | `string` | No | - | Playlist title (`Create`, `Update`) |
| `description` | `string` | No | - | Playlist description (`Create`, `Update`) |
| `privacyStatus` | `enum` | No | `public` | `public`, `private`, `unlisted` (`Create`, `Update`) |

*Required only for the listed operation.

### Outputs

| Operation | Output |
| --- | --- |
| `Create a playlist` | Created playlist response |
| `Delete a playlist` | `{ success: true }` |
| `Get a playlist` | Playlist response |
| `Retrieve all playlists` | Playlist list response |
| `Update a playlist` | Updated playlist response |

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

#### API Client not initialized

**Cause:** OAuth setup failed.

**Solution:** Verify OAuth credentials and retry.

#### playlistId is required

**Cause:** Operation requires `playlistId`.

**Solution:** Provide the playlist ID for `Get`, `Update`, or `Delete`.

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->

## Related

- YouTube Channel
- YouTube Playlist Item
- YouTube Video
- YouTube Video Category

<!-- /SECTION: related -->
