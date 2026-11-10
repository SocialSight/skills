# Storyboard

One image that previews what happens in the video: 4–6 panels, one per beat,
in order. Every video starts from it — with people, animals, products, or
just a place. The video binds it as a reference image (never as its first
frame — it is a grid). Write it in English.

## Template

```
Storyboard sheet for a <N>-second video, on a plain off-white background.
<K> panels in a grid, read left to right, top to bottom, each panel in
<VIDEO ASPECT>, separated by thin white gutters, numbered with its time
range under it (0–3s, 3–6s, …).

Subjects, identical in every panel:
<NAME 1>: <identity block, ≤350 chars> | the <role> from reference_N.
<NAME 2>: <identity block> | the product from reference_N.

Panel 1 (0–<t>s): <framing>, <camera move>, <action and who>.
Panel 2 (<t>–<t>s): <framing>, <camera move>, <action and who>.
…
Panel <K> (<t>–<N>s): <the payoff, held>.

Same look in every panel: <photoreal live-action | 3D animated | 2D
illustrated>, <grade>, <light>. One fixed location.

Negative: no text beyond the panel numbers and time ranges, no speech
bubbles, no arrows or UI, no extra characters.
```

Nothing recurring (a place, b-roll): drop the Subjects block.

## Edit of a clip

The panels are the source clip's beats, in its order, with the change
applied ("the two fighters are now a Siamese and a calico cat"). Image
models cannot see video: write each panel from what the clip shows
(setting, framing, light, poses), then the change. Panel aspect = the
clip's.

## Rules

- Every `<…>` in the template is replaced with concrete content from this
  request; never output template words or generic stand-ins ("the same
  people, products, wardrobe", "the subject", "as before").
- Drop any template line that does not apply (no people → no Subjects
  block; no product → no product line).
- If the request has no story yet (only "make it longer", "now 15s"), take
  the beats from the last approved storyboard or video prompt in the
  conversation; never fill panels with placeholders.
- Panels are the video's beats, same order and seconds; they sum to its
  duration (the seconds the user asked for, or the nearest listed value).
  The video prompt follows the same beats.
- 4 panels for ≤8s, up to 6 for longer videos.
- Identity blocks: concrete visual traits (age, build, face, hair, fur,
  wardrobe, colors, signature features with exact counts). The video prompt
  names the same subjects "the <name> from reference_N".
- A subject from an uploaded photo is "the <role> from reference_N" in
  every panel; never redesign it.
- Each panel is a frame: framing, light, pose. Name the camera move in
  words ("slow push-in"), never draw arrows.
- Write every panel concretely: what is in it. Never "same as panel 2 of
  reference_1" — the image model cannot read references that way;
  reference_N only says who / what must look the same.
- A change to the storyboard keeps every earlier change.
- Sheet aspect `16:9` if the image model lists it, else the closest wide
  one it lists.
