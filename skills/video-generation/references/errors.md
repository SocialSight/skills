# Errors

## Insufficient credits

A submit that the account cannot afford fails with HTTP 402 (no `job_id`, no
debit). Keep any jobs already started in the same batch; report which
generation failed and ask the user to top up — do not cancel earlier jobs or
retry the same paid call.

## Parameter outside catalog `options`

The request was rejected before a job existed. Change the request: pick a value from `parameters[].options`, `durations`, `resolutions`, or `aspect_ratios`. Do not resubmit the same payload.

## `count != 1`

The schema allows 1–4; anything above 1 is rejected. Omit `count` or set it to `1`. For N variations, make N separate `generate_video` calls (submit each, report each `job_id`).

## Job `status: "error"`

The job was created and then failed. The payload includes `error_code` and `retryable`.

| What happened | What to do |
|---|---|
| Parameter / validation rejection (no job, or a clear invalid-value error) | Change the request. Do not resubmit as-is. |
| Seedance `InvalidParameter` / `image_url` aspect ratio outside 0.39–2.50 | Drop or replace that still (see constraints). Do not retry the same medias. |
| Job `status: "error"` and `retryable` is true | Resubmit the same payload as-is, once. |
| Two identical failures | Change the prompt or parameters. Do not loop. |

`retryable` on a **non-terminal** job (`pending` / `processing`) only means "poll later". The client does that — you do not call `job_status` after submit.

After submit, `polling` is `"client_side"` and `agent_action` is `"report_job_id_and_stop"`: do not poll that job. Once every planned call is submitted, report the `job_id`s and end the turn, even when you expect the widget to show an error later.
