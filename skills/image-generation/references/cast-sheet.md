# Cast sheet

One image with every person, character, animal, or product the user asked
to see as a character / product sheet, side by side. Never one image per
character. Only when they ask for a sheet — never the preview of a video
(that is the storyboard). Write it in English.

## Template

```
Character reference sheet, on a plain light-grey studio background. <N>
subjects side by side, same scale, evenly spaced, each labeled with its
name under it.

<NAME 1>: <identity block>.
<NAME 2>: <identity block> | the <role> from reference_N.
<PRODUCT>: <identity block> | the product from reference_N.

Each person or character: a full-body front view and a full-body 3/4 view,
with a head-and-shoulders close-up above them. Each product: front, side,
and held in one hand for scale.

Render: <photoreal live-action | 3D animated | 2D illustrated>, soft even
key light, contact shadows under every figure, consistent lighting across
all subjects.

Negative: no environment, no props unless part of an identity, no extra
people, no text beyond the name labels, must not resemble any real
celebrity or existing character, mascot, or brand.
```

## Identity block

At most ~350 characters of concrete visual traits: age, build, height cue,
face, hair, skin, wardrobe and colors, signature features. Later prompts
paste the same block word for word, or name "the <name> from reference_N".

## Rules

- Every `<…>` in the template is replaced with concrete content from this
  request; never output template words or generic stand-ins ("the same
  people, products, wardrobe", "the subject", "as before").
- Drop any template line that does not apply (no people → no person views;
  no product → no product line).
- A subject from an uploaded photo is "the <role> from reference_N": never
  redesign it, and drop "must not resemble…" for that subject (it is the
  user's own person, character, or product).
- Counts are numbers: "exactly one scar over the left eye", "three ribs on
  the grip" — never "some" or "a few".
- Stills are frames: no motion words.
- The look matches the ask: an animated sheet for an animated look.
