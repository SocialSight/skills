---
name: video-generation
description: >
  Use when generating, animating, editing, or extending video through
  SocialSight MCP. Use when the user asks to make a clip, animate a still, add
  start or end frames, attach reference images, video, or audio, quote video
  credits, choose duration, resolution, or aspect ratio, submit generate_video,
  or names Seedance, Veo, or Kling. Use before any video job so catalog values
  replace schema defaults.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Video generation

## Call shape

`generate_video` nests arguments under a required `params` object:

```json
{ "params": { "model": "SEEDANCE_2_5", "prompt": "..." } }
```

Primitive-arg tools are top-level: `models_explore`, `get_generation_models`, `job_status`, `job_display`, `balance`, `transactions`.

Media goes in `params.medias` as `[{ "value": "<media_id or completed job_id>", "role": "..." }]`. Never a URL. Import first (skill `media-refs`).

## Discover before generating

Call `models_explore(action="get", model_id=...)` and read `durations`, `resolutions`, `qualities`, `aspect_ratios`, `medias[].roles`, and `parameters[].options`. These are exact value lists, not ranges. Do not restate or cache the catalog — query it.

**Never use the tool schema defaults.** The schema defaults to `resolution: "720p"` (lowercase) while the catalog publishes `"720P"` (uppercase), and to `duration: 5`, which is invalid on models like VEO3_1_LITE that only accept `[4, 6, 8]`. Both fail. Values always come from the catalog.

Load [references/model-discovery.md](references/model-discovery.md) for the discovery workflow.

## Duration

`supported_durations` / `durations` is a discrete list, not a min/max range. Some models publish an empty list and take no `duration` at all (KLING_2_6_MOTION). When a requested duration is not in the list, round to an allowed value and adjust the shot rather than sending the requested number.

## Budget

Read `medias[]` on the catalog entry. `start_image` and `end_image` consume the `reference_image` budget (`counts_toward`). On SEEDANCE_2_5 that is `#ref_image + #start_frame + #end_frame ≤ 30`. On VEO3_1, `max_inputs: 3` — anchor + start + end saturates it. Do **not** use `max_reference_inputs` for the image cap — on 2.5 that is 50 and counts all inputs. Details in [references/constraints.md](references/constraints.md).

## Cost, then submit, then stop

1. Quote the whole plan with `get_cost: true`. Returns `{ credits, model_id, job_type }` and creates no job.
2. Submit `generate_video` without `get_cost`.
3. The response carries `polling: "client_side"` and `agent_action: "report_job_id_and_stop"`. Report the `job_id` and end the turn. Do **not** call `job_status` — the client polls it, respecting `poll_after_seconds` (3 pending, 5 processing, null when terminal).

`count` is reserved. The schema publishes 1–4 but the payload rejects anything above 1. N variations means N separate calls. Omit `count` or set it to 1.

## Author vs edit

With a source video (`role: "video"`), edit mode preserves the source structure, motion, timing, and audio. Without one, generation is from scratch and the audio is newly generated via `enable_audio` — not the original. Pass `enable_audio` only when the selected model declares audio support.

Errors: [references/errors.md](references/errors.md).
