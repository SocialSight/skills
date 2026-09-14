# Upload flows

Import or upload first. Generation `params.medias[].value` accepts a `media_id` or a **completed** `job_id` only — never a URL.

## URL → media_id

```json
{
  "params": {
    "url": "https://example.com/product.jpg",
    "type": "auto",
    "file_name": "product.jpg"
  }
}
```

Call `media_import_url`. `type` is `auto` | `image` | `video` | `audio`. Use `auto` unless the user or model context makes the type explicit. `file_name` is optional.

The tool returns a MediaItem. Copy `media_id` into `params.medias[].value`.

## Local file → media_id

Only when the agent/runtime can read the bytes and PUT them itself. Do not use this for chat attachments the MCP server cannot read.

1. `media_upload`:

```json
{
  "params": {
    "filename": "hero.png",
    "content_type": "image/png",
    "size_bytes": 184320
  }
}
```

Returns `upload_id`, `media_id`, `upload_url`, `method`, `headers`, `expires_at`, `max_bytes`.

2. PUT the file bytes to `upload_url` using the returned `method` and `headers`. Do not rewrite the headers.

3. `media_confirm`:

```json
{
  "params": {
    "upload_id": "<upload_id from step 1>",
    "filename": "hero.png",
    "content_type": "image/png"
  }
}
```

Returns the MediaItem. Use that `media_id` in generation.

## Existing media

`show_medias` lists previously uploaded and completed generated files:

```json
{ "params": { "modality": "image", "limit": 50 } }
```

Optional filters: `source` (`uploaded` | `generated`), `modality` (`image` | `video` | `audio`), `page_token`. Do not call this to wait for a job just submitted — report the `job_id` and stop.

## Reusing a generation

A **completed** `job_id` is valid in `params.medias[].value`. The MCP server resolves it to the job's result `media_id`. Pending or failed jobs are not valid.

## Chaining clips

MediaItem may include `last_frame_media_id`. Pass that as `role: "start_image"` on the next clip. Do not generate a join frame from the last frame.
