---
name: photo-modes
description: >
  Use when writing the photographic prompt for generate_image: a studio product
  shot, lifestyle scene, close-up with hands, moodboard, hero banner, or
  editorial still. Use when the user wants product photography, campaign
  imagery, or a styled photo and the prompt must be written in full — lens,
  light quality, surface, and atmosphere — because SocialSight has no backend
  prompt enhancer.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Photo modes

SocialSight does not enhance prompts on the backend. Write the photographic prompt in full: lens, light quality, surface, and atmosphere. Do not keep it short.

## Call shape

`generate_image` nests arguments under `params`. Discover `qualities`, `aspect_ratios`, and reference limits with `models_explore(action="get", model_id=...)` — never use schema defaults. Media refs go in `params.medias` as `[{ "value": "<media_id or completed job_id>" }]`. `role` is ignored; only `value` matters.

Submit the real job. Omit `count` or set it to 1; N stills are N calls. Credits and HTTP 402 follow `video-generation` (Submit every call, then stop): stop at the first 402, keep the jobs already started, then report each `job_id` and end the turn — do not wait for results.

## Pick a treatment

| Mode | Use when |
|---|---|
| Studio product shot | Catalog / packshot on a controlled background |
| Lifestyle scene | Product in a real environment |
| Close-up with hands | Beauty, demo, tactile detail |
| Moodboard | Vertical pin, art-direction collage feel |
| Hero banner | Wide site / email / campaign header |
| Editorial | Fashion or magazine still |

Load [references/modes.md](references/modes.md) for framing, lighting vocabulary, background treatment, and a worked prompt per mode.
