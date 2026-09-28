# Constraints

Always re-read the selected model's `medias[]` via `models_explore(action="get", model_id=...)`. The numbers below are how to interpret that payload, with two models as worked examples.

## `counts_toward`

`start_image` and `end_image` consume the `reference_image` budget. They are not a free extra slot.

The catalog publishes this as `counts_toward: "reference_image"` on `start_frame` / `end_frame`. On **SEEDANCE_2_5** the rule the API enforces is:

```
#ref_image + #start_frame + #end_frame ≤ 30
```

Start and end are max 1 each. Anchor + `start_image` + `end_image` uses 3 of 30, leaving 27 for `role: "image"`.

Do **not** use `max_reference_inputs` for this. On 2.5 that field is 50 and counts **all** inputs (images + video + audio), not just images. The image cap lives in `medias[]` under `name: "reference_image"` (`max: 30`).

On **VEO3_1**, `max_inputs: 3`. Anchor + `start_image` + `end_image` saturates it. There is no leftover for extra `role: "image"` refs.

## Seedance reference image aspect ratio

Seedance downloads each `role: "image"` / start / end still and rejects the
**job** (often after `job_id` exists) when a still's pixel aspect ratio is
outside **0.39–2.50** (`InvalidParameter` on `image_url`, e.g. a 2.66
panorama). This is **not** the same as `params.aspect_ratio` (output frame).

Catalog validate does **not** check still dimensions — submit can succeed
and the provider fails later. If a ref is ultra-wide or ultra-tall,
omit that media from `params.medias` (and say so in `summary`), or ask the
user to re-upload a crop closer to 9:16 / 1:1 / 16:9. Do not resubmit the
same medias as-is.

## `supported_durations` is a discrete list

Not a min/max range. `[4, 6, 8]` does **not** mean 4–8. `5` is invalid on that model (VEO3_1_LITE is this shape). Do not group, interpolate, or treat the endpoints as a span.

If `durations` / `supported_durations` is `[]` (e.g. KLING_2_6_MOTION), the model takes no `duration` at all — omit the field. Do not send the schema default of `5`.

If the user wants a length that is not in the list, round to an allowed value and rewrite the shot so it still works at that length. Never send a number that is not in the list.
