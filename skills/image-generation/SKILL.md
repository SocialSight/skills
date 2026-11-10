---
name: image-generation
description: >
  Use when the user wants a still through SocialSight MCP: a new image, an
  edit of a photo, product or campaign photography, a character sheet, or
  the storyboard a video starts from (4–6 panels, one per beat). Picks the
  model and writes the image prompt in full.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Image generation

SocialSight does not enhance prompts on the backend. Write every prompt in
full.

## Model

- Default: **`GEMINI_3_IMAGE`** (Nano Banana Pro) — best identity hold across
  views; the default for storyboards and cast sheets.
- **`OPENAI_IMAGE_2_5`** (GPT Image) — text on the image, graphic layouts.
- **`QWEN_IMAGE_3_0_PRO`** — when they ask for Flexible / fewer restrictions.
- A model the user named wins.
- One `models_explore(action="get", model_id=...)` and copy `quality` and
  `aspect_ratio` verbatim. Quality `2K` unless they asked sharper (`4K` if
  listed). Never schema defaults.
- The prompt never names the model and never uses model-specific syntax
  (e.g. `@Image1`); reference media by role ("the <role> from
  reference_N"). The model can change after the prompt is written.

## Edit vs new

- **Edit** (a photo is the source): the prompt states only the change;
  everything else stays — people, product, framing, layout. Never describe
  what is already in the photo; the model sees it. A second image (a logo, a
  person to add) comes after the source, and the prompt says what to take
  from it.
- **New**: a full photographic prompt — lens, light, surface, atmosphere.
  Pick a treatment (studio, lifestyle, close-up with hands, moodboard, hero
  banner, editorial) from [references/modes.md](references/modes.md).
- Text on the image: quote the exact words, where they sit, how they look.

## The storyboard a video starts from

Every video starts from one **storyboard** (skill `video-generation`): 4–6
panels, one per beat of the video, in order, each in the **video model's**
listed `aspect_ratio`. Template:
[references/storyboard.md](references/storyboard.md). Do not invent the
aspect — wait for the video skill's `models_explore get`.

- People, animals, products, and places all live in the panels, identical
  from panel to panel; an uploaded photo is "the <role> from reference_N".
- An edit of a clip: the panels are that clip's beats with the change
  applied. Image models take no video: write the scenes from what the clip
  shows (setting, framing, light, poses) plus the change.
- A change to the storyboard keeps every earlier change.

A **cast sheet** ([references/cast-sheet.md](references/cast-sheet.md)) is
only for an explicit ask for character / product sheets, never a video's
preview.

## Start the job

N stills are N calls; `count` stays 1. How a job is started and paid depends
on the host. Without a host rule, follow
`video-generation/references/submit.md`.
