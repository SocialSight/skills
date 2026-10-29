# Character sheet

One `generate_image` prompt that renders a model sheet for a character,
creature, or product. Fill the template for any topic; never copy a
specific story, character, or brand. Write it in English.

## Character template

```
Character model sheet of <NAME>, on a plain <COLOR> studio background.

Top-left: title "<NAME>" and a short info block — type: <TYPE>;
personality: <2–3 TRAITS>; build: <BUILD>; style: <STYLE>.

Color palette strip, labeled: <MATERIAL/COLOR 1>, <MATERIAL/COLOR 2>,
<MATERIAL/COLOR 3>, <MATERIAL/COLOR 4>.

Views, same scale, evenly spaced: full-body front, full-body side,
full-body 3/4 hero, full-body rear. Head turnaround: front, side, 3/4.

Expression strip, labeled: <EXPR 1>, <EXPR 2>, <EXPR 3>, <EXPR 4>
(4–6 total). Action poses (2–3, small): <POSE FROM THE STORY>,
<POSE FROM THE STORY>.

Identity: <IDENTITY BLOCK, word for word>.
Anatomy and proportions: <HEAD-TO-BODY RATIO, LIMB LENGTH, HEIGHT CUE>.
Materials: <SKIN / FUR / FABRIC / SURFACE, FINISH>.
Signature features, exact counts: <exactly one tail>, <exactly two
antennae>, <one scar over the left eye>.

Render: <material detail>, soft even key light, contact shadows under
every figure, consistent lighting across all views.

Negative: no clothing or props unless part of the identity; no
environment; no extra characters; no text beyond the title, info block,
and labels; must not resemble any existing character, mascot, or
celebrity.
```

## Product template

```
Product model sheet of <PRODUCT>, on a plain <COLOR> studio background.

Top-left: title "<PRODUCT>" and a short info block — type: <TYPE>;
size: <DIMENSIONS CUE>; finish: <FINISH>; style: <STYLE>.

Color palette strip, labeled: <MATERIAL/COLOR 1>, <MATERIAL/COLOR 2>,
<MATERIAL/COLOR 3>.

Views, same scale: front, side, top, back, held in one hand for scale.
Close-ups: label, <MATERIAL 1> texture, <CLOSURE / DETAIL>.

Identity: <IDENTITY BLOCK, word for word>. Exact counts: <exactly one
cap>, <three ribs on the grip>.

Render: <material detail>, soft even key light, contact shadows,
consistent lighting across all views.

Negative: no props beyond the hand; no environment; no extra products;
no text beyond the title, info block, labels, and the product's own
printed label; must not resemble any existing brand or product.
```

## Rules

- The identity block is the same ≤350-char look used in the video prompt;
  the sheet only expands it with anatomy, materials, and counts.
- With an uploaded photo, the photo is the identity: write "the <role>
  from reference_N" and never redesign it. It wins over the negatives:
  drop "must not resemble any existing …" for that subject (it is the
  user's own person, character, or product), in both templates.
- Counts are numbers, not "some" or "a few".
- Action poses come from the brief's beats, not generic hero poses.
