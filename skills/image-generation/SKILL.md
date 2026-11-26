---
name: image-generation
description: >
  Use when the user wants a still through SocialSight MCP: a new image, an
  edit of a photo, product or campaign photography, a character sheet, or
  a storyboard of a video they asked to preview (4–6 panels, one per beat).
  Picks the model and writes the image prompt in full.
compatibility: "Requires the SocialSight MCP server. If `generate_video` or `generate_image` are unavailable, stop and tell the user to connect it."
---

# Image generation

SocialSight does not enhance prompts on the backend. Write every prompt in
full.

## Model

- Default: **`GEMINI_NANO_BANANA_2_1`** (Nano Banana): most images and
  edits, storyboards, and cast sheets.
- **`SEEDREAM_5_5`**: photorealism (lifelike people, fashion, product
  photos), when the user's plan allows it.
- A model the user named wins.
- One `models_explore(action="get", model_id=...)` first, unless the host
  already gave you the model's values. Copy `quality` and `aspect_ratio`
  verbatim, never schema defaults. Quality `2K` unless they asked sharper
  (`4K` if listed).
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

## A storyboard for a video

Only when the user asks to see a video before it is made (skill
`video-generation`): one image, 4–6 panels, one per beat of the video, in
order, each in the **video model's** listed `aspect_ratio` (never invent
it). Template: [references/storyboard.md](references/storyboard.md).

- People, animals, products, and places are identical from panel to panel;
  an uploaded photo is "the <role> from reference_N".
- An edit of a clip: the panels are the video they asked for, beat by beat.
  Image models take no video and you cannot see the clip: build them from
  what they said and the photos they attached.
- A change to the storyboard keeps every earlier change.

A **cast sheet** ([references/cast-sheet.md](references/cast-sheet.md)) is
only for an explicit ask for character / product sheets, never a video's
preview.

## Start the job

N stills are N calls; `count` stays 1. How a job is started and paid depends
on the host. Without a host rule, follow
`video-generation/references/submit.md`.
