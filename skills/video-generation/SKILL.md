---
name: video-generation
description: >
  Use when the user wants a video through SocialSight MCP: a new clip, an ad,
  a clip from a photo, or an edit of a clip. Picks the model and its values
  from the live catalog and writes the video prompt in full. A storyboard
  preview is made only when the user asks for one.
compatibility: "Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it."
---

# Video generation

The video prompt is written in full: nothing downstream expands it.

## 1. Model

- Default: **`SEEDANCE_2_5`** (reference images, reference video, long
  durations, audio). A model the user named wins.
- One `models_explore(action="get", model_id=...)` first, unless the host
  already gave you the model's values. Copy `duration`, `resolution`,
  `aspect_ratio`, and `medias[].roles` verbatim, never schema defaults
  (`duration: 5`, `resolution: "720p"`). Reading the limits:
  [references/constraints.md](references/constraints.md). Finding a model:
  [references/model-discovery.md](references/model-discovery.md).
- Aspect ratio: what they asked for, else `9:16` (Reels, Stories, TikTok,
  Shorts). Feed portrait → `4:5`. YouTube or landscape → `16:9`. Always an
  exact string the model lists.
- Duration: seconds the user gives are the duration ("make it 15s",
  "longer, 15s" → 15, not "more than 15"). Use that value if the model
  lists it, else the nearest listed one, and say so in one line. A new
  video with no length given → a listed value near 8 seconds. An edit
  keeps the clip's length.

## 2. New video, photo, or edit

- **A new video** is made from their words. Photos they attached are
  identity (who or what is in it), not the plot.
- **A photo to animate** ("animate this", "now make it a video") is a new
  video that starts from that photo and keeps its subject and look.
- **An edit of a clip** keeps the clip as the video: its shots, motion,
  timing, length, and audio. It is bound as `role: "video"`. You cannot see
  it, so never invent what it shows. A different length or a new story is
  a new video, not an edit: the old clip only lends the look.

## 3. A storyboard, only when they ask

Make one only when the user asks to see the video before it is made
("show me a storyboard first"). Never on your own.

- One image, 4–6 panels, one per beat, each panel in the video's aspect
  (skill `image-generation`, `references/storyboard.md`). Never one image
  per character.
- The video comes after it is done. Bind it as `role: "image"` if the model
  lists that role (it is a grid, never `start_image`). A new video's beats
  follow its panels, same order and seconds. An edit keeps the clip's own
  timing: the storyboard only shows who and what goes in.
- A change they ask for is a new version of the storyboard, with every
  earlier change kept.

## 4. The prompt

Written as [references/dialog.md](references/dialog.md) sets out. In short:

- English. The story is the video they asked for; a later message changes
  only what it names.
- Never name the model or use model-specific syntax (`@Image1`). Bound
  media is "the <name> from reference_N", never re-described. The model can
  change after the prompt is written.
- ≤8s or one beat → SHORT (timed beats). Longer → FULL. One camera move
  per beat ([references/motion.md](references/motion.md)).
- An ad takes one feel from their words (skill `marketing-studio`), said in
  plain words, never the slug.
- An edit states only the change: who or what replaces what, and any new
  lines or captions. No header, shot list, beats, camera moves, or
  location: the model would make that video instead of editing the clip.
  New speech or on-screen text is written out in full: each line in quotes,
  in order, short enough for the clip; subtitles are those same words.
- `enable_audio` only when the model declares audio. They asked for a
  voice or sound and it does not: say so in one line before starting, or
  use a model with audio when they named none.
- N variations are N prompts and N calls; `count` stays 1.

## 5. Start the job

How a job is started and paid depends on the host. Without a host rule, follow
[references/submit.md](references/submit.md). Errors:
[references/errors.md](references/errors.md).
