# Submit

For hosts that call `generate_image` / `generate_video` directly. A host
with its own spend flow (a plan the user approves, then code submits)
replaces this file.

## Call shape

```json
{ "params": { "model": "SEEDANCE_2_5", "prompt": "..." } }
```

Primitive-arg tools are top-level: `models_explore`, `get_generation_models`,
`job_status`, `job_display`, `balance`, `transactions`.

Media goes in `params.medias` as `[{ "value": "<media_id or completed job_id>", "role": "..." }]`. Never a URL — import first (skill `media-refs`). On `generate_image`, `role` is ignored; only `value` matters.

## Model, then storyboard, then video

After `models_explore get` on the video model (aspect and `medias[].roles`
fixed), submit the storyboard, report its `job_id`, and stop.
Make the video on a later turn, once that job is complete, with that
`job_id` in `params.medias` using only a role the model lists.

## Submit every call, then stop

Each job is its own `generate_*` call. Credits are checked on that call —
do not pre-quote with `get_cost`. A short wallet is HTTP 402 with no
`job_id`.

1. Submit the planned calls (N variations = N calls).
2. Stop at the first 402: keep the jobs already started, send no more, say
   which one failed, and ask them to top up.
3. Report the `job_id` of each successful submit and end the turn.

Do not wait for results or call `job_status` — the client polls
(`polling: "client_side"`).
