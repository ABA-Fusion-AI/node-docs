---
node_id: "YouTubeVideoNode"
title: "YouTube Video"
description: "Upload, retrieve, update, delete or rate videos"
category: "actions"
subcategory: "youtube"
version: "1.0.0"
language: "en"
last_updated: "2026-04-14"
author: "Fusion Team"
tags:
  - youtube
  - video
  - upload
  - oauth2
related_nodes:
  - YouTubeChannelNode
  - YouTubePlaylistNode
  - YouTubePlaylistItemNode
  - YouTubeVideoCategoryNode
---

<!-- SECTION: header -->

# YouTube Video

> **Category:** Actions | **Type:** Action Node

Video operations: `Retrieve all videos`, `Get a video`, `Upload a video`, `Update a video`, `Delete a video`, `Rate a video`.

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
| `operation` | `enum` | Yes | - | `Delete a video`, `Get a video`, `Retrieve all videos`, `Rate a video`, `Update a video`, `Upload a video` |
| `videoId` | `string` | Yes* | - | Video ID (`Get`, `Update`, `Delete`, `Rate`) |
| `limit` | `enum` | No | `10` | `10`, `25`, `50` (`Retrieve all videos`) |
| `filePath` | `string` | Yes* | - | Local video path (`Upload a video`) |
| `title` | `string` | No | - | Video title (`Upload`, `Update`) |
| `description` | `string` | No | - | Video description (`Upload`, `Update`) |
| `categoryId` | `enum` | No | `People & Blogs` | Video category label (`Upload`, `Update`) |
| `tags` | `string` | No | - | Tags, comma separated (`Upload`, `Update`) |
| `privacyStatus` | `enum` | No | `public` | `public`, `private`, `unlisted` (`Upload`, `Update`) |
| `license` | `enum` | No | `youtube` | `youtube`, `creativeCommon` (`Upload`, `Update`) |
| `madeForKids` | `boolean` | No | `false` | Self-declared Made for Kids (`Upload`, `Update`) |
| `videoLanguage` | `enum` | No | - | Video language label (`Upload`, `Update`) |
| `titleDescriptionLanguage` | `enum` | No | - | Title/description language (`Upload`, `Update`) |
| `captionCertification` | `enum` | No | `none` | `none`, `noCaption`, `captionCertified` (`Upload`, `Update`) |
| `shortsRemixMode` | `enum` | No | `allow` | `allow`, `audioOnly`, `deny` (`Upload`, `Update`) |
| `thumbnailPath` | `string` | No | - | Thumbnail image path (.jpg/.png) (`Upload`, `Update`) |
| `captionPath` | `string` | No | - | Subtitle file path (.srt/.vtt) (`Upload`, `Update`) |
| `captionLanguage` | `enum` | No | `fr` | Subtitle language label (`Upload`, `Update`) |
| `captionName` | `string` | No | - | Subtitle track name (`Upload`, `Update`) |
| `rating` | `enum` | Yes* | - | `like`, `dislike`, `none` (`Rate a video`) |

*Required only for the listed operation.

### Outputs

| Operation | Output |
| --- | --- |
| `Retrieve all videos` | Video list response |
| `Get a video` | Video response |
| `Upload a video` | Upload response object |
| `Update a video` | Updated video response |
| `Delete a video` | `{ success: true }` |
| `Rate a video` | `{ success: true }` |

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

#### videoId is required

**Cause:** Operation requires `videoId`.

**Solution:** Provide a valid video ID.

#### filePath is required

**Cause:** Upload requires a local file.

**Solution:** Provide a valid local video path.

#### Unsupported file extension

**Cause:** The file type is not supported.

**Solution:** Use a supported extension (video: .mp4, .mov, .avi, .mkv, .webm, .wmv, .flv; images: .jpg, .jpeg, .png, .webp; captions: .srt, .vtt).

<!-- /SECTION: troubleshooting -->

---

<!-- SECTION: related -->

## Related

- YouTube Channel
- YouTube Playlist
- YouTube Playlist Item
- YouTube Video Category

<!-- /SECTION: related -->
