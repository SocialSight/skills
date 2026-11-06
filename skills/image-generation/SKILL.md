---
name: image-generation
description: >
  Use when the user wants a still through SocialSight MCP: a new image, an
  edit of a photo, product or campaign photography, or the image a video
  starts from (a cast sheet with every character, or a key frame). Picks the
  model and writes the image prompt in full.
---

Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it.

# Image generation

SocialSight does not enhance prompts on the backend. Write every prompt in
full.

## Model

- Default: **`GEMINI_3_IMAGE`** (Nano Banana Pro) — best identity hold across
  views; the default for cast sheets.
- **`OPENAI_IMAGE_2_5`** (GPT Image) — text on the image, graphic layouts.
- **`QWEN_IMAGE_3_0_PRO`** — when they ask for Flexible / fewer restrictions.
- A model the user named wins.
- One `models_explore(action="get", model_id=...)` and copy `quality` and
  `aspect_ratio` verbatim. Quality `2K` unless they asked sharper (`4K` if
  listed). Never schema defaults.

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

## The image a video starts from

Every new video starts from one image (skill `video-generation`). The video
skill picks the video model and a listed `aspect_ratio` **before** this
image is made:

- **Cast sheet** — every person, character, animal, or product of the
  video in **one** image, side by side. Template:
  [references/cast-sheet.md](references/cast-sheet.md). Aspect `16:9` if
  the image model lists it, else the closest wide one it lists.
- **Key frame** — nothing recurring: the first shot of the video as a still,
  in the **video model's** listed `aspect_ratio`, framed and lit as it
  opens. No motion words. Do not invent an aspect — wait for the video
  skill's `models_explore get`. If the image model does not list that
  aspect, use the closest one it lists, or another image model that lists
  it.

## Start the job

N stills are N calls; `count` stays 1. How a job is started and paid depends
on the host. Without a host rule, follow
`video-generation/references/submit.md`.
