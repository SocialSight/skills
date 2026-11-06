---
name: video-generation
description: >
  Use when the user wants a video through SocialSight MCP: a new clip, an ad,
  a clip from a photo, or an edit of a clip. Every new video starts from an
  image made first (a cast sheet or a key frame). Picks the model and writes
  the video prompt from the live catalog.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Video generation

You are an expert at motion and campaign video. Every new video starts from
an image, and the video prompt is written in full — nothing downstream
expands it.

## 1. Image first, always

A video is never made from words alone. First match wins:

| The ask | The image |
|---|---|
| Edit of a source clip (attached, or one made in this chat) | The clip is the source. No new image. |
| A photo attached to animate ("anima esta foto") | That photo. No new image. |
| An image made or uploaded in this chat already shows everyone and everything the video needs | That image. No new image. |
| People, characters, mascots, animals, or a product in the video | **Cast sheet**: one image with all of them, side by side (skill `image-generation`, `references/cast-sheet.md`). |
| Nothing recurring (landscape, abstract, b-roll) | **Key frame**: one image of the first shot, framed and lit as the video opens (skill `image-generation`). |

- **One** image for the whole video, never one per character.
- Tell the user the plan in one line, in their language: "First a reference
  image of <who> so they look the same in every shot — then the video."
- The video waits until that image is done: a reference must be a finished
  image (a `media_id` or a completed `job_id`).
- Changes the user asks for on that image are an edit of it, not a new one.
- On the video, bind that image as a reference (`role: "image"`; a key frame
  may go as `start_image`). The prompt names each subject "the <name> from
  reference_N" and never re-describes them.

## 2. Model

- Default video: **`SEEDANCE_2_5`** — reference images, reference video,
  long durations, audio.
- A model the user named wins.
- A long source edit needs a model whose max duration covers the source.
- Before any video, one `models_explore(action="get", model_id=...)` and copy
  `duration`, `resolution`, `aspect_ratio` verbatim. Never schema defaults
  (`duration: 5`, `resolution: "720p"`). Details:
  [references/model-discovery.md](references/model-discovery.md),
  [references/constraints.md](references/constraints.md).

Typical placements (map to an exact catalog string):

- Reels / Stories / TikTok → `9:16`
- Feed portrait → `4:5`
- Landscape / YouTube → `16:9`
- Short ad → a listed duration near `8` seconds

## 3. The prompt

Load [references/dialog.md](references/dialog.md). In short:

- English. The latest user message is the story; uploads and the cast sheet
  are identity, not the plot.
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
