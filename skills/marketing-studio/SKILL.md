---
name: marketing-studio
description: >
  Use when planning a branded ad video or its storyboard through SocialSight
  MCP: UGC, tutorial, unboxing, product review, product showcase, TV spot,
  virtual try-on, or conceptual product ad. Use when the brief needs a style
  (cast, shots, camera motion, sound) before writing generate_image /
  generate_video prompts.
---

# Marketing Studio

You plan ads, not just UGC. Before writing a script or any prompt, pick
**one mode** from `references/modes.md` from the user's words. The mode sets
the cast, the shot list, the camera and motion, and the sound. The brief
still wins on product, lines, and setting.

## Picking flow

- Looks like a real person filmed on a phone → `ugc` family (`ugc`,
  `unboxing`, `tutorial`, `try_on` with an organic feel).
- Polished broadcast commercial → `tv_spot`.
- Show the product itself, little or no presenter → `showcase`.
- A presenter giving an opinion → `review`.
- Someone wearing or using it → `try_on`.
- Surreal, levitating, splash, CGI → `conceptual`.
- "Surprise me" → the mode that best fits the product; say which.
- No clue at all → `ugc`.

Keep the mode across the storyboard and the footage of the same ad unless
the user asks for another.

## Writing it

- Storyboard stills are frames: framing, light, and pose only.
- Video beats follow `references/motion.md`: camera · action · motion ·
  sound, one camera move per beat.
- Tell the user the mode in plain words ("an unboxing video", "a polished
  TV-style ad"), never the slug.

Adapted from the Marketing Studio modes in
[higgsfield-ai/skills](https://github.com/higgsfield-ai/skills) (MIT).
