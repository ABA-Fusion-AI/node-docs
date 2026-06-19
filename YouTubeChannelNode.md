---
node_id: "youtube"
title: "YouTube Nodes"
description: "Manage YouTube channels, playlists, playlist items, videos, and video categories"
category: "actions"
subcategory: "youtube"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - youtube
  - video
  - playlist
  - channel
  - oauth2
related_nodes:
  - YouTubeChannelNode
  - YouTubePlaylistNode
  - YouTubePlaylistItemNode
  - YouTubeVideoNode
  - YouTubeVideoCategoryNode
---

<!-- SECTION: header -->
# YouTube Nodes

> **Category:** Actions | **Type:** Action Nodes

Manage YouTube channels, playlists, playlist items, videos, and video categories using OAuth2 credentials.

<!-- /SECTION: header -->

---

<!-- SECTION: overview -->
## Overview

This document covers the suite of **YouTube** action nodes:

- **YouTube Channel** - Retrieve, update, or upload a channel banner
- **YouTube Playlist** - Create, retrieve, update, or delete playlists
- **YouTube Playlist Item** - Add, retrieve, or remove items from a playlist
- **YouTube Video** - Upload, retrieve, update, delete, or rate videos
- **YouTube Video Category** - Retrieve video categories by region

### Key Features

- **OAuth2 Access:** Uses Google Client ID, Client Secret, and Refresh Token
- **Multiple Operations:** Select an operation per node to expose the needed fields
- **Media Uploads:** Supports video uploads, thumbnails, captions, and channel banners
- **Regional Data:** Fetch video categories for a specific country

### Use Cases

- Publish videos and update metadata on a schedule
- Build playlist automation for new uploads
- Moderate and update channel branding
- Fetch categories for regional content targeting

<!-- /SECTION: overview -->

---

<!-- SECTION: configuration -->
## Configuration

### Common Auth Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `clientId` | `string` | Yes | - | Google OAuth2 Client ID |
| `clientSecret` | `string` | Yes | - | Google OAuth2 Client Secret |
| `refreshToken` | `string` | Yes | - | OAuth2 refresh token |

> **Note:** Each node validates credentials with a lightweight YouTube API call during setup.

### YouTube Channel Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | - | `Retrieve a channel`, `Retrieve all channels`, `Update a channel`, `Upload a channel banner` |
| `channelId` | `string` | Conditional | - | Required for retrieve/update/banner by ID; optional for update if using the authenticated channel |
| `channelTitle` | `string` | Conditional | - | Channel title (update only) |
| `channelDescription` | `string` | Conditional | - | Channel description (update only) |
| `channelDefaultLanguage` | `string` | Conditional | - | Default language (ISO 639-1) |
| `channelCountry` | `string` | Conditional | - | Country (ISO 3166-1 alpha-2) |
| `channelKeywords` | `string` | Conditional | - | Comma-separated keywords |
| `channelTrackingAnalyticsAccountId` | `string` | Conditional | - | Google Analytics tracking ID |
| `channelModerationEnabled` | `boolean` | Conditional | - | Moderate all comments |
| `bannerPath` | `string` | Conditional | - | Path to banner image (.jpg/.png), min 2048x1152, max 6 MB |

### YouTube Playlist Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | `Retrieve all playlists` | `Create a playlist`, `Delete a playlist`, `Get a playlist`, `Retrieve all playlists`, `Update a playlist` |
| `playlistId` | `string` | Conditional | - | Required for get/update/delete |
| `limit` | `number` | Conditional | `10` | Max results for retrieve all (1-50) |
| `title` | `string` | Conditional | - | Playlist title (create/update) |
| `description` | `string` | Conditional | - | Playlist description (create/update) |
| `privacyStatus` | `enum` | Conditional | `public` | `public`, `private`, `unlisted` |

### YouTube Playlist Item Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | - | `Add an item to a playlist`, `Delete an item from a playlist`, `Get a playlist's item`, `Retrieve all playlist items` |
| `playlistId` | `string` | Conditional | - | Required for all operations |
| `videoId` | `string` | Conditional | - | Required for add/get/delete |
| `limit` | `number` | Conditional | `50` | Max results for retrieve all (1-50) |

### YouTube Video Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | - | `Delete a video`, `Get a video`, `Retrieve all videos`, `Rate a video`, `Update a video`, `Upload a video` |
| `videoId` | `string` | Conditional | - | Required for get/update/delete/rate |
| `limit` | `enum` | Conditional | `10` | Max results for retrieve all (`10`, `25`, `50`) |
| `filePath` | `string` | Conditional | - | Local video path for upload (e.g. `/tmp/video.mp4`) |
| `title` | `string` | Conditional | - | Video title (upload/update) |
| `description` | `string` | Conditional | - | Video description (upload/update) |
| `categoryId` | `enum` | Conditional | `People & Blogs` | Video category label (mapped to YouTube category ID) |
| `tags` | `string` | Conditional | - | Comma-separated tags |
| `privacyStatus` | `enum` | Conditional | `public` | `public`, `private`, `unlisted` |
| `license` | `enum` | Conditional | `youtube` | `youtube`, `creativeCommon` |
| `madeForKids` | `boolean` | Conditional | `false` | Self-declared Made for Kids |
| `videoLanguage` | `enum` | Conditional | - | Human-readable language label (mapped to ISO code) |
| `titleDescriptionLanguage` | `enum` | Conditional | - | Language for title/description |
| `captionCertification` | `enum` | Conditional | `none` | `none`, `noCaption`, `captionCertified` |
| `shortsRemixMode` | `enum` | Conditional | `allow` | `allow`, `audioOnly`, `deny` |
| `thumbnailPath` | `string` | Conditional | - | Thumbnail image path (.jpg/.png) |
| `captionPath` | `string` | Conditional | - | Subtitles file path (.srt/.vtt) |
| `captionLanguage` | `enum` | Conditional | `fr` | Subtitle language label (mapped to ISO code) |
| `captionName` | `string` | Conditional | - | Subtitle track name |
| `rating` | `enum` | Conditional | - | `like`, `dislike`, `none` (rate operation) |

**Supported upload extensions:** `.mp4`, `.mov`, `.avi`, `.mkv`, `.webm`, `.wmv`, `.flv` for videos; `.jpg`, `.jpeg`, `.png`, `.webp` for images; `.srt`, `.vtt` for captions.

### YouTube Video Category Parameters

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `operation` | `enum` | Yes | `Retrieve all video categories` | Retrieve categories for a selected region |
| `region` | `enum` | Yes | `United States` | Country label (mapped to ISO 3166-1 alpha-2) |

<!-- /SECTION: configuration -->

---

<!-- SECTION: inputs-outputs -->
## Inputs & Outputs

### Inputs

These are action nodes. They execute once per incoming item. Fields marked as **expression** in the UI can reference the incoming input item.

### Outputs

| Output | Type | Description |
|--------|------|-------------|
| `success` | `object` | API response or operation result |
| `error` | `Error` | Thrown when validation, auth, or API requests fail |

### Sample Output Shapes

**YouTube Playlist Item - Add:**
```json
{
  "success": true,
  "playlistId": "PL123",
  "videoId": "abc123"
}
```

**YouTube Channel - Upload banner:**
```json
{
  "success": true,
  "steps": ["Step 1: Validate and prepare image", "Step 2: Verify API access"],
  "cropped": true,
  "compressed": false,
  "upscaled": false,
  "convertedToJpeg": false,
  "originalSize": "5.32 MB",
  "finalSize": "4.11 MB",
  "finalDimensions": { "width": 2048, "height": 1152 },
  "bannerUploadResponse": { "url": "https://..." }
}
```

<!-- /SECTION: inputs-outputs -->

---

<!-- SECTION: examples -->
## Examples

### Retrieve All Playlists

```json
{
  "clientId": "your-client-id",
  "clientSecret": "your-client-secret",
  "refreshToken": "your-refresh-token",
  "operation": "Retrieve all playlists",
  "limit": 10
}
```

### Add Video to Playlist

```json
{
  "clientId": "your-client-id",
  "clientSecret": "your-client-secret",
  "refreshToken": "your-refresh-token",
  "operation": "Add an item to a playlist",
  "playlistId": "PL123",
  "videoId": "abc123"
}
```

### Upload a Video With Thumbnail

```json
{
  "clientId": "your-client-id",
  "clientSecret": "your-client-secret",
  "refreshToken": "your-refresh-token",
  "operation": "Upload a video",
  "filePath": "/tmp/video.mp4",
  "title": "Launch demo",
  "description": "Short product demo",
  "privacyStatus": "unlisted",
  "thumbnailPath": "/tmp/thumb.jpg"
}
```

### Retrieve Video Categories for France

```json
{
  "clientId": "your-client-id",
  "clientSecret": "your-client-secret",
  "refreshToken": "your-refresh-token",
  "operation": "Retrieve all video categories",
  "region": "France"
}
```

<!-- /SECTION: examples -->

---

<!-- SECTION: workflow-example -->
## Workflow Integration

### Sample Workflow: Upload and Add to Playlist

```json
{
  "nodes": [
    {
      "id": "upload-video",
      "type": "YouTubeVideoNode",
      "position": { "x": 100, "y": 120 },
      "config": {
        "clientId": "your-client-id",
        "clientSecret": "your-client-secret",
        "refreshToken": "your-refresh-token",
        "operation": "Upload a video",
        "filePath": "/tmp/video.mp4",
        "title": "Weekly update",
        "privacyStatus": "unlisted"
      }
    },
    {
      "id": "add-to-playlist",
      "type": "YouTubePlaylistItemNode",
      "position": { "x": 380, "y": 120 },
      "config": {
        "clientId": "your-client-id",
        "clientSecret": "your-client-secret",
        "refreshToken": "your-refresh-token",
        "operation": "Add an item to a playlist",
        "playlistId": "PL123",
        "videoId": "{{input.id}}"
      }
    }
  ],
  "connections": [
    {
      "source": "upload-video",
      "sourceOutput": "success",
      "target": "add-to-playlist",
      "targetInput": "input"
    }
  ]
}
```

<!-- /SECTION: workflow-example -->

---

<!-- SECTION: troubleshooting -->
## Troubleshooting

### Common Issues

#### OAuth2 refresh failed

**Cause:** Invalid `clientId`, `clientSecret`, or `refreshToken`.

**Solution:** Regenerate OAuth2 credentials and ensure the refresh token has YouTube scopes.

#### API Client not initialized

**Cause:** `setup()` did not complete successfully.

**Solution:** Check auth parameters and network access, then retry.

#### File not found / unsupported extension

**Cause:** Upload paths are incorrect or files are not supported.

**Solution:** Verify local paths and use supported extensions.

#### Banner image too small or too large

**Cause:** Banner must be at least 2048x1152 and under 6 MB.

**Solution:** Provide a larger image or compress before upload.

### Error Codes (Channel Banner)

| Code | Message | Solution |
|------|---------|----------|
| `required` | Banner image path is required | Provide `bannerPath` |
| `file_not_found` | File not found | Check the file path |
| `invalid_format` | Unsupported format | Use `.jpg` or `.png` |
| `invalid_image` | Failed to read image metadata | Use a valid image file |
| `invalid_image_dimensions` | Missing dimensions | Use a valid image file |
| `image_too_small` | Image too small | Minimum 2048x1152 |
| `image_too_large` | Cannot compress below 6 MB | Use a smaller file |

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->
## Related

- YouTube Channel
- YouTube Playlist
- YouTube Playlist Item
- YouTube Video
- YouTube Video Category

<!-- /SECTION: related -->

---

<!-- SECTION: changelog -->
## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-14 | Initial release |

<!-- /SECTION: changelog -->
