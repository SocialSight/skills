# Roles

`params.medias` is `[{ "value": "<media_id or completed job_id>", "role": "<role>" }]`.

## Canonical catalog roles

These are the roles `models_explore` publishes on `medias[].roles`:

| Catalog role | Typical use |
|---|---|
| `image` | Reference still / product / identity |
| `start_image` | First frame of a video |
| `end_image` | Last frame of a video |
| `video` | Source or reference clip |
| `audio` | Reference soundtrack / voice |

## Alias → payload type

The MCP layer maps the role you send onto a payload input type. Accepted aliases:

| You send (`role`) | Payload type |
|---|---|
| `image`, `reference_image`, `ref_image` | `ref_image` |
| `start_image`, `start_frame` | `start_frame` |
| `end_image`, `end_frame` | `end_frame` |
| `video`, `reference_video`, `ref_video` | `ref_video` |
| `audio`, `reference_audio`, `ref_audio` | `ref_audio` |

Prefer the canonical catalog role (`image`, `start_image`, `end_image`, `video`, `audio`). Confirm the selected model actually lists that role via `models_explore(action="get", model_id=...)`.

## Image generation

`generate_image` ignores `role` entirely. Only `value` is used. You may still send `role: "image"` for consistency.

## Video generation

`role` is required and must match a role the selected model publishes. Start and end are typically max 1 each. Image-budget rules (`counts_toward`) live in the video-generation skill, not here.
