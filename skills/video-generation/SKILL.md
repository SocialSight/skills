---
name: video-generation
description: >
  Use when generating, animating, editing, or extending video or campaign
  stills through SocialSight MCP. Use when choosing aspect, quality,
  resolution, or duration, quoting credits, or submitting generate_image /
  generate_video. Query the catalog before any job so schema defaults are
  never sent.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Video generation

You are an expert at campaign stills and motion. You know the SocialSight
tools: `generate_image` and `generate_video` (arguments nest under
`params`), and `models_explore` for live catalog values. You pick aspect,
quality, resolution, and duration from that catalog — never schema
defaults (`duration: 5`, `resolution: "720p"`).

Typical placements (map to an exact catalog string; do not invent):

- Instagram Reels / Stories → `9:16`
- Feed portrait → `4:5`
- Landscape / YouTube → `16:9`
- Stills → quality `2K` unless they asked sharper (`4K` if listed)
- Short ad → a listed duration near `8` seconds

N variations = N separate calls. Each call uses that row's planner prompt
(talent / wardrobe / setting change as asked). `count` stays 1 — the API
rejects `count > 1`.

## Call shape

```json
{ "params": { "model": "SEEDANCE_2_5", "prompt": "..." } }
```

Primitive-arg tools are top-level: `models_explore`, `get_generation_models`, `job_status`, `job_display`, `balance`, `transactions`. This agent does not receive `job_status` / `job_display`.

Media goes in `params.medias` as `[{ "value": "<media_id or completed job_id>", "role": "..." }]`. Never a URL. Import first (skill `media-refs`). On `generate_image`, `role` is ignored; only `value` matters.

## Discover before generating

Call `models_explore(action="get", model_id=...)` and read `durations`, `resolutions`, `qualities`, `aspect_ratios`, `medias[].roles`, and `parameters[].options`. These are exact value lists, not ranges. Do not cache the catalog.

Load [references/model-discovery.md](references/model-discovery.md) for the discovery workflow.

## Duration

`supported_durations` / `durations` is a discrete list, not a min/max range. Some models publish an empty list and take no `duration` at all (KLING_2_6_MOTION). When a requested duration is not in the list, do not send that number. 15s on a model that lists 4/6/8 is two catalog clips (8+8 or 8+6) with join frames — never `duration: 15`. Round only to a listed value.

## Budget

Read `medias[]` on the catalog entry. `start_image` and `end_image` consume the `reference_image` budget (`counts_toward`). On SEEDANCE_2_5 that is `#ref_image + #start_frame + #end_frame ≤ 30`. On VEO3_1, `max_inputs: 3` — anchor + start + end saturates it. Do **not** use `max_reference_inputs` for the image cap — on 2.5 that is 50 and counts all inputs. Details in [references/constraints.md](references/constraints.md).

## Submit, then stop

After approve, submit starts each `generations[]` row (and `stills[]`) as its own
`generate_*`. Credits are checked on that call (HTTP 402 if the wallet is
short) — do not pre-quote with `get_cost`. Producer does the same on edit /
stills. After each successful submit, report the `job_id` and stop that job.
If a later row 402s, keep earlier jobs and report which generation failed.
The client polls (`polling: "client_side"`). Do **not** call `job_status`.

## Author vs edit

With a source video (`role: "video"`), edit mode preserves the source structure, motion, timing, and audio. Without one, generation is from scratch and the audio is newly generated via `enable_audio` — not the original. Pass `enable_audio` only when the selected model declares audio support.

## Script generation

Load [references/script.md](references/script.md). The planner writes the
script from the user message; refs are likeness, not the plot. Always write
spoken lines and music when the brief has them. "No VO" is not the default.
Each campaign variation gets its own `generations[].prompt`.

Errors: [references/errors.md](references/errors.md).
