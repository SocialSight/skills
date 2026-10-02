# Script generation

You write the plan **Script** section and every `generations[].prompt`.
Refs are likeness only. Caps (`duration`, model, audio) come from
`models_explore` / the saved generation row — do not invent them.

## Brief vs stills

The latest user message is the story. If they named a product, brand,
pitch, dance, music, or spoken line, **that is the video**. Never write
"not a product ad". Never replace that ask with the still's scene. Never
describe the job as "animate the attached stills".

Refs are face / clothes / pack. Setting and plot come from the brief.
Match each upload to the brief by its kind (image / video / audio) and the
user's words ("this bag" + one photo → the photo), never by upload order.
With an uploaded photo, the photo is the identity: name it ("the bag in
reference_1"), never describe or redesign it.
Every distinctive brief phrase must appear in Script and in
`generations[].prompt`.

## Required shape (timed beats)

For the chosen catalog `duration` seconds, write beats that **sum to that
duration**. Each beat: time range · camera · action · who (`reference_N`)
· product beat.

Example for 15s silent product spot:

```
0–5s: medium, reference_2 (talent 26–35) enters — idea of elevated everyday.
5–10s: hands reveal product (reference_3), slow push-in, premium light.
10–15s: hero hold product front; talent soft in background.
Silent — brief did not ask for VO or music.
```

Bad (never save this):
"No voiceover. Visual story only: open on confident young talent…"

## What else to include

- Two asks ("X, then at the end Y") → two acts. Split generations when one
  catalog duration cannot hold both (`join_frames`).
- "Three variations" → three `generations[]`, three distinct prompts, same
  timing; change only talent / wardrobe / setting as asked.
- Spoken copy: if they asked a line or brand, write it **verbatim**. Pace
  ≈ 2.2 words/sec against each shot duration.
- Music / SFX: if they named a genre or feel, put it on the script. Audio
  flag is already on `generations[].enable_audio`.
- Do **not** default to "No VO". Only mark silent when the brief has no
  speech/music **and** `enable_audio` is false — and still write the timed
  visual beats.

## Prompt

`generations[].prompt` = same timed beats as Script (camera + VO/music),
ready for `generate_*`. Script section = human outline of that copy.
Never leave Script empty or mood-only.
