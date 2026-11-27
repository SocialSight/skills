# Constraints

The limits are the selected model's own values: its `medias[]`, durations,
and aspect ratios from `models_explore(action="get", model_id=...)`, or the
values the host already gave you. This page is how to read them.

## Start and end frames spend the image budget

`start_image` and `end_image` (one each at most) count against the
reference image cap; they are not extra slots. The catalog marks this as
`counts_toward: "reference_image"`.

- **SEEDANCE_2_5:** reference images + start + end ≤ 30. One reference, a
  start, and an end use 3, leaving 27 for `role: "image"`.
- **VEO3_1:** `max_inputs: 3`. One reference, a start, and an end fill it.

The image cap is the `max` of `reference_image` in `medias[]`. Do not read
it from `max_reference_inputs`: that counts every input (images, video,
audio) and is 50 on Seedance 2.5.

## Seedance rejects very wide or very tall stills

Seedance fails the **job**, often after a `job_id` exists, when a reference,
start, or end still has a pixel aspect ratio outside **0.39–2.50** (a 2.66
panorama, for example). This is the still's own shape, not
`params.aspect_ratio`, and validation does not catch it. When the video
needs that still (the photo to animate, or its only reference), ask for a
crop closer to 9:16, 1:1, or 16:9. Otherwise leave it out and say so in
`summary`. Never resubmit the same media as it is.

## Durations are a list, not a range

`[4, 6, 8]` does not mean 4 to 8: `5` is invalid on that model. An empty
list means the model takes no `duration`: omit the field, never send the
schema default of `5`.

A length the user wants that is not listed: use the closest listed value
and rewrite the timed beats to sum to it, or, when they named no model,
pick one that lists it.
Never send an unlisted number, and never split one request into several
clips to reach it.
