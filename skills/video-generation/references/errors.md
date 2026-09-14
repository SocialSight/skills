# Errors

## Insufficient credits

A submit that the account cannot afford fails. Quote first with `get_cost: true`, compare to `balance`, and only then submit. If credits are short, report the quote and stop — do not retry the same paid call.

## Parameter outside catalog `options`

The request was rejected before a job existed. Change the request: pick a value from `parameters[].options`, `durations`, `resolutions`, or `aspect_ratios`. Do not resubmit the same payload.

## `count != 1`

The schema allows 1–4; anything above 1 is rejected. Omit `count` or set it to `1`. For N variations, make N separate `generate_video` calls (cost-quote each, submit each, report each `job_id`).

## Job `status: "error"`

The job was created and then failed. The payload includes `error_code` and `retryable`.

| What happened | What to do |
|---|---|
| Parameter / validation rejection (no job, or a clear invalid-value error) | Change the request. Do not resubmit as-is. |
| Job `status: "error"` and `retryable` is true | Resubmit the same payload as-is, once. |
| Two identical failures | Change the prompt or parameters. Do not loop. |

`retryable` on a **non-terminal** job (`pending` / `processing`) only means "poll later". The client does that — you do not call `job_status` after submit.

After submit, `polling` is `"client_side"` and `agent_action` is `"report_job_id_and_stop"`. Report the `job_id` and end the turn even when you expect the widget to show an error later.
