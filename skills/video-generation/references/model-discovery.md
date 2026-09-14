# Model discovery

Query the live catalog. Do not copy model tables into prompts or notes — they drift.

## Find a model

- `models_explore(action="recommend", query="...", type="video", limit=5)` — shortlist for a goal.
- `models_explore(action="search", query="...", type="video")` — broader match.
- `get_generation_models(modality="video")` — IDs and high-level flags. Models marked `recommended` are reasonable defaults.

These tools take **top-level** arguments, not a `params` object.

## Read constraints

```text
models_explore(action="get", model_id="<ID>")
```

Take values only from:

| Field | Meaning |
|---|---|
| `durations` | Allowed duration integers. Empty → omit `duration`. |
| `resolutions` | Exact strings (often `"720P"`, not `"720p"`). |
| `qualities` | Image-style quality enums when present. |
| `aspect_ratios` | Exact strings. |
| `medias[].roles` | Roles you may send on `params.medias`. |
| `medias[].max` / `required` / `counts_toward` | Per-role caps and budget. |
| `parameters[].options` | Exact allowed values for that parameter. |

Lists are enums, not ranges. If `8` is not in `durations`, do not send `8`.

## Fill generate_video

Copy catalog strings **verbatim** into `params`. Do not lowercase resolutions. Do not use the tool schema default of `duration: 5` or `resolution: "720p"`.

If the user asked for a duration that is not listed, pick the closest allowed value and adjust the shot (pace, hold, coverage) so the request still makes sense.

## Audio

Pass `enable_audio` only when the catalog entry declares audio support. On a from-scratch generate, `enable_audio` creates **new** audio. On an edit with a source video, the source audio is preserved — see the skill body.
