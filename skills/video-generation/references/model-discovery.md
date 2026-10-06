# Model discovery

Query the live catalog. Do not copy model tables into prompts or notes — they drift.

## Find a model

`models_explore` takes **top-level** arguments, not a `params` object.

| `action` | What it does | Empty `items` when… |
|---|---|---|
| `recommend` | Optional `query` substring filter, then keep the curated `recommended: true` allowlist (else fall back to query matches). Default `limit=5`. Order ≈ catalog order — **not** a ranker. | `query` matches nothing |
| `search` | Substring filter on catalog text. Default `limit=20`. | Same: zero matches |
| `list` | Full modality catalog; **ignores `query`**. Default `limit=20`. | Almost never |
| `get` | One model by `model_id`. | Uses `error`, not empty items |

Empty success looks like `{ "items": [], "has_more": false, "error": null }` — not a tool error.

### What `recommended` means

Curated **allowlist** from SocialSight (manifest `explore.recommendation` or a
hardcoded copy dict on the MCP catalog) — editorial push defaults ("good for
i2v", "fast iteration", …). **Not** best-fit for this brief: no wallet,
history, live price, or query scoring. `query` only narrows; it does not rank.

### Default path (do this)

```text
models_explore(action="get", model_id="SEEDANCE_2_5")   # our default video
# or: recommend / list, then pick SEEDANCE_2_5 from items
```

SocialSight's default video model is **`SEEDANCE_2_5`**, even when other
allowlist rows (Wan, Veo, Kling, …) appear first. Use another model only if
the user names one, Seedance is missing, or its caps cannot meet the brief.

**Do not** paste the user brief as `query` — long chat text rarely
substring-matches and returns `items: []`. Optional short `query` only for a
catalog token (e.g. a family name), never the full message.

### If `items` is empty

1. Call `action="list"` with the same `type` (ignores the bad query), **or**
   call `recommend` again with no query.
2. For video, still prefer **`SEEDANCE_2_5`** when present; otherwise any
   allowlist row that fits the brief.
3. Never invent a `model_id`.

`get_generation_models(modality="video")` also exposes `recommended` flags if
you need a flat ID list.

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

A duration that is not listed: see [constraints.md](constraints.md).

## Audio

Pass `enable_audio` only when the catalog entry declares audio support. On a from-scratch generate, `enable_audio` creates **new** audio. On an edit with a source video, the source audio is preserved — see the skill body.
