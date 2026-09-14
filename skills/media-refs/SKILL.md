---
name: media-refs
description: >
  Use when turning user attachments into SocialSight media_id values before
  generate_image or generate_video. Use when the user pastes an image, video,
  or audio URL; attaches a local file the runtime can upload; asks to reuse a
  previous generation as a reference, start frame, or end frame; supplies a
  product photo, face, or clip to condition a job; or when params.medias must
  be filled from a media_id or a completed job_id rather than a URL.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Media refs

Turn every user attachment into a `media_id` (or a **completed** `job_id`) before generation. Never put a URL in `params.medias[].value`.

## Call shape

`media_import_url`, `media_upload`, `media_confirm`, and `show_medias` nest arguments under a required `params` object:

```json
{ "params": { "url": "https://example.com/photo.jpg", "type": "image", "file_name": "photo.jpg" } }
```

## Flows

- **HTTPS URL** → `media_import_url`. The returned MediaItem's `media_id` goes in `params.medias[].value`.
- **Local file the runtime can PUT** → `media_upload` → PUT bytes to `upload_url` with the returned `method` and `headers` → `media_confirm` with `upload_id`.
- **Already in SocialSight** → use the existing `media_id`, or a completed `job_id`. `show_medias` lists prior uploads and completed generations.

Remote MCP tools cannot read chat attachments. If the file is only in the chat and the runtime cannot upload it, say so.

## Roles

Pass `params.medias` as `[{ "value": "<media_id or completed job_id>", "role": "<canonical role>" }]`.

Canonical catalog roles: `image`, `start_image`, `end_image`, `video`, `audio`.

`generate_image` ignores `role` entirely — only `value` matters.

A MediaItem may include `last_frame_media_id`. Use that as `start_image` on a following clip; do not generate a join frame.

Load [references/upload-flows.md](references/upload-flows.md) for the exact payloads and [references/roles.md](references/roles.md) for the role table and aliases.
