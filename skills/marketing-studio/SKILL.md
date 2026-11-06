---
name: marketing-studio
description: >
  Use when planning a branded ad video or its storyboard through SocialSight
  MCP: UGC, tutorial, unboxing, product review, product showcase, TV spot,
  virtual try-on, or conceptual product ad, an animated / editorial
  explainer, or an app demo with screenshots. Use when the ask needs a style
  (cast, shots, camera motion, sound) before writing generate_image /
  generate_video prompts.
---

# Marketing Studio

You plan ads, not just UGC. Before writing a script or any prompt, pick
**one mode** from `references/modes.md` from the user's words. The mode sets
the cast, the shots, the camera and motion, and the sound. The user's
words still win on product, lines, and setting.

## Picking flow

- Looks like a real person filmed on a phone → `ugc` family (`ugc`,
  `unboxing`, `tutorial`, `try_on` with an organic feel).
- Polished broadcast commercial → `tv_spot`.
- Show the product itself, little or no presenter → `showcase`.
- A presenter giving an opinion → `review`.
- Someone wearing or using it → `try_on`.
- Surreal, levitating, splash, CGI → `conceptual`.
- Explains an idea or how something works; animated, illustrated,
  editorial, motion comic, documentary → `explainer` (a narrator carries
  it).
- An app, website, or marketplace shown through its screens while someone
  talks to camera → `app_demo` (screenshots are overlays, not characters).
- "Surprise me" → the mode that best fits the product; say which.
- No clue at all → `ugc`.

Keep the mode across the storyboard and the footage of the same ad unless
the user asks for another.

## Writing it

- Storyboard stills are frames: framing, light, and pose only. For the
  cast sheet a video starts from, use skill `image-generation`,
  [references/cast-sheet.md](../image-generation/references/cast-sheet.md).
- Video beats follow skill `video-generation`,
  [references/motion.md](../video-generation/references/motion.md): camera ·
  action · motion · sound, one camera move per beat.
- Sound and text follow the user's words, not the mode's column, when they
  differ. The lines and the on-screen text live in the prompt
  (`video-generation` / `dialog.md`, "On-screen text").
- Only `tv_spot` and `app_demo` close on an end card or CTA by default.
  Every other mode — `explainer` above all — ends on the last image of the
  story, held. A title card, slogan, or logo frame the user asked for comes
  after that hold, never instead of it (`dialog.md`, "End on a hold").
- Tell the user the mode in plain words ("an unboxing video", "a polished
  TV-style ad"), never the slug.

Adapted from the Marketing Studio modes in
[higgsfield-ai/skills](https://github.com/higgsfield-ai/skills) (MIT).
