---
name: video-generation
description: >
  Use when the user wants a video through SocialSight MCP: a new clip, an ad,
  a clip from a photo, or an edit of a clip. Every video starts from a
  storyboard image the user approves first: 4–6 panels, one per beat. Picks
  the model and writes the video prompt from the live catalog.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Video generation

You are an expert at motion and campaign video. Every video — new or an
edit — starts from a storyboard the user sees and approves first, and the
video prompt is written in full — nothing downstream expands it.

## 1. Model, then storyboard, then video

A video is never made from words alone. Order:

1. Pick the video model and a listed `aspect_ratio` (and duration) — §2.
2. Plan the beats, then make the **storyboard**: one image with 4–6
   panels, one per beat, in order, each panel in that aspect (skill
   `image-generation`, `references/storyboard.md`). People, animals,
   products, and places all live in the panels, identical panel to panel.
3. Wait until that image is done, then submit the video.

- **One** storyboard per video, never one image per character.
- **An edit of a source clip** (attached, or made in this chat): the panels
  are that clip's beats with the change applied. The clip stays the video's
  source.
- **A photo to animate** (attached, or made in this chat) when they only
  ask to make it move ("animate this photo", "now make it a video"): that
  photo is the preview. No storyboard. If they describe a new video with
  it in it (a scene, a story, cinematic shots), make the storyboard first;
  the photo only lends the look.
- Tell the user the plan in one line, in their language: "First a
  storyboard of the video so you can see it — then the video."
- The video waits until the storyboard is done: a reference must be a
  finished image (a `media_id` or a completed `job_id`).
- A change the user asks for is a new version of the storyboard, with every
  earlier change kept.
- On the video, bind the storyboard as `role: "image"` (it is a grid —
  never `start_image`), only if the model's `medias[].roles` lists it. On
  an edit, the source clip keeps `role: "video"`. The video's beats follow
  the panels, same order and seconds. The prompt names each subject "the
  <name> from reference_N" and never re-describes them.

## 2. Model

- Default video: **`SEEDANCE_2_5`** — reference images, reference video,
  long durations, audio.
- A model the user named wins.
- A long source edit needs a model whose max duration covers the source.
- **Before the storyboard and before any video**, one
  `models_explore(action="get", model_id=...)` and copy `duration`,
  `resolution`, `aspect_ratio`, and `medias[].roles` verbatim. Never schema
  defaults (`duration: 5`, `resolution: "720p"`). Storyboard panels use
  that `aspect_ratio`; the storyboard sheet is `16:9` (or the closest wide
  aspect the image model lists). Details:
  [references/model-discovery.md](references/model-discovery.md),
  [references/constraints.md](references/constraints.md).

Typical placements (map to an exact catalog string):

- Reels / Stories / TikTok → `9:16`
- Feed portrait → `4:5`
- Landscape / YouTube → `16:9`
- Short ad → a listed duration near `8` seconds

A number of seconds the user gives is the duration: "make it 15s", "15
seconds long", "longer, 15s" → 15 (not "more than 15"). Use that value if
the model lists it, else the nearest listed value, and say so in one line
in their language. Storyboard panel time ranges add up to that duration.
On an existing clip, a different length is a new video (a new storyboard;
the old clip only lends the look), not an edit — an edit keeps the source
length.

## 3. The prompt

Load [references/dialog.md](references/dialog.md). In short:

- English. The latest user message is the story; uploads are identity, not
  the plot. The approved storyboard is the plan: beats follow its panels.
- The prompt never names the model and never uses model-specific syntax
  (e.g. `@Image1`); reference media by role ("the <role> from
  reference_N"). The model can change after the prompt is written.
- ≤8s or one beat → SHORT (timed beats). Longer → FULL (header, characters,
  location, shots, audio, continuity).
- One camera move per beat ([references/motion.md](references/motion.md)).
- An ad: pick one feel from their words — real person on a phone (UGC),
  tutorial, unboxing, review, product showcase, TV spot, try-on,
  surreal/CGI, explainer, app demo (skill `marketing-studio` for the table).
  Say it in plain words, never the slug.
- An edit of a source clip states only the change; it keeps the source's
  motion, timing, and audio. Pass `enable_audio` only when the model
  declares audio.
- N variations = N separate prompts and N calls; `count` stays 1.

## 4. Start the job

How a job is started and paid depends on the host. Without a host rule, follow
[references/submit.md](references/submit.md). Errors:
[references/errors.md](references/errors.md).
