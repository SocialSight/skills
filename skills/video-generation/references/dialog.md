# Script generation

You write the script and every video prompt, for any topic, from the
templates below. Never reuse a story, character, or brand from an example.
Prompts are in English. Caps (model, `durations`, audio) come from
`models_explore` — do not invent them.

## Fixed inputs

- Duration: the confirmed brief's (already a listed value for the model).
  With no brief, the length resolved from the model's `durations`
  (rounded per `constraints.md`). Beats always sum to that resolved length,
  never to an unlisted number the user said.
- With a shot list: 2–10 beats in whole seconds that sum to the duration.
  The prompt follows those beats in order. Inside a beat you may split into
  faster cuts with decimal timecodes (a 3s beat = 0.6s + 0.4s + 2.0s).
- A character or product look is at most ~350 characters of concrete visual
  traits. The video and image prompts have no length cap.
- Sound: the brief's sound. narration — an off-screen voice. dialogue — the
  on-screen cast speaks. music_only — nobody speaks; AUDIO ends with "No
  dialogue". With no brief, the mode's Sound column (modes.md); no mode →
  music only. A line the user quoted goes in verbatim whatever the setting:
  spoken with narration or dialogue; with music_only, on screen as a TEXT:
  line instead (with text none, one end-card TEXT: in the final beat).
- Text: the brief's text — subtitles, keywords, labels, or none (see
  "On-screen text"). With a shot list, each shot's line and text are the
  script: copy them word for word, never rewrite them here.

## Brief vs uploads

The latest user message is the story. If they named a product, brand,
pitch, dance, music, or spoken line, **that is the video**. Never write
"not a product ad", never replace the ask with the still's scene, and never
describe the job as "animate the attached stills".

Image uploads are identity only (face, clothes, pack); setting and plot
come from the brief. One exception: a screenshot (an app, a website, a
chart, a document) is an overlay, not a character — the beat says where it
sits over the frame ("the listing from <<<reference_2>>> slides in over her
left shoulder") and what the presenter does with it. A source video (`role: "video"`) is different: edit
mode keeps its structure, motion, timing, and audio, so the prompt states
only the change ("make her jacket red"), never new beats or a new length.
Match each upload by its kind (image / video / audio) and the user's words
("this bag" + one photo → the photo), never by upload order. Name an
uploaded photo ("the bag in reference_1"); never describe or redesign it.
Every distinctive brief phrase must appear in the prompt.

## Pick a tier

- **SHORT** — ≤8s or a single product beat: timed beats only.
- **FULL** — the default for everything else: six sections, in order.

## SHORT

One line per beat: time range · framing + camera move · action · who.

```
0–3s: close-up, slow push-in, <HERO> lifts <PRODUCT> into window light.
3–6s: macro, rack focus to the label; liquid settles.
6–8s: hero hold, <PRODUCT> front and centered; camera locked.
```

## FULL

1. **HEADER** — one line: exact duration, aspect ratio, look (live-action /
   3D animated / …), color grade, light, lens and depth of field, camera
   energy, tone. End with "Keep faces, costumes and locations identical in
   every shot."
2. **CHARACTERS** — one identity block per person, creature, or product
   (≤350 chars, from the saved look). Paste it word for word wherever that
   character appears. With an uploaded photo, write "the <role> from
   <<<reference_N>>>", never a description.
3. **LOCATION** — one fixed geography: landmarks and where they sit
   relative to each other. It never changes mid-video.
4. **SHOTS** — one block per shot-list beat, same order and seconds:
   `BEAT n (mm:ss–mm:ss) — <title>`, then its cuts: framing + camera move +
   action + who. Add `Speed: REAL TIME` or `SLOW MOTION (~N%)` when the
   speed changes. Cut times are contiguous inside the beat. Hold the payoff.
   With narration, each beat ends with its VO: line; with dialogue, the
   speaking cut names who talks and quotes the line. With any text but
   none, the beat ends with TEXT: — what appears on screen there.
5. **AUDIO** — only when the model has audio. With a voice, first voice —
   who it is (gender, age, tone, pitch, pace) and the two or three words to
   lean on. Then the bed — kept under the voice — and synced sounds per
   beat with timestamps at the peaks. Write "No dialogue" when nobody
   speaks.
6. **CONTINUITY RULES** — objects move only for an on-screen cause; no
   teleporting, duplicated props, or geography changes; signature features
   stay attached and single; no extra characters, logos, or HUD unless
   asked; no text beyond the TEXT: lines; no end card, title card, or
   slogan except as "End on a hold" (Writing guidance) allows. With any
   text but none, one TEXT STYLE: line fixes the look
   (see "On-screen text").

### Skeleton

```
HEADER: <N>s, <ASPECT>, <LOOK>, <GRADE>, <LIGHT>, <LENS + DOF>, <CAMERA ENERGY>, <TONE>. Keep faces, costumes and locations identical in every shot.

CHARACTERS:
<HERO>: <identity block, ≤350 chars>
<RIVAL or PRODUCT>: <identity block> | the <role> from <<<reference_1>>>

LOCATION: <SETTING>. <LANDMARK A> on the left, <LANDMARK B> behind <HERO>, <EXIT> on the right.

SHOTS:
BEAT 1 (00:00–00:03) — <setup>
  0.0–1.5s: wide, static. <HERO> <goal action> near <LANDMARK A>. Speed: REAL TIME
  1.5–3.0s: medium, slow push-in. <HERO> notices <TRIGGER>.
BEAT 2 (00:03–00:06) — <complication>
  3.0–3.6s: close-up, whip pan. <TRIGGER> hits <OBJECT>.
  3.6–4.0s: insert. <OBJECT> tips over.
  4.0–6.0s: medium, handheld. <HERO> reacts. SLOW MOTION (~50%)
BEAT 3 (00:06–00:10) — <PAYOFF>
  6.0–10.0s: medium-wide, camera locked. <PAYOFF>; hold. Speed: REAL TIME

AUDIO: bed — <MUSIC/AMBIENCE>. 00:03.4 <impact SFX>; 00:06.2 <payoff sting>. No dialogue.

CONTINUITY RULES: <the six rules above>.
```

With a voice and text, each beat carries its shot's line and text, and the
AUDIO opens with the voice:

```
BEAT 1 (00:00–00:03) — <setup>
  0.0–3.0s: wide, slow push-in. <HERO> <goal action>. Speed: REAL TIME
  VO: "<the shot's line>"
  TEXT: <the shot's text>
…
AUDIO: voice — <gender, age, tone, pitch, pace>; lean on "<word>", "<word>". bed — <restrained MUSIC>, under the voice. 00:05.6 <impact SFX> on the turn.
TEXT STYLE: <one of the three looks below>.
```

### Good vs bad

- Bad: "A fun, energetic scene where the hero has a chaotic morning."
  Good: "3.0–3.6s: close-up, whip pan. The cup slides off the table edge.
  SLOW MOTION (~40%)."
- Bad: "the same man again". Good: paste his full identity block, word for
  word.
- Bad: "in a kitchen". Good: "Kitchen: window on the left, stove behind
  him, door on the right."

## Writing guidance

- Every gag or product moment is physical cause → effect, both on screen.
- One clear goal per character.
- Escalate beat to beat; the last beat is the biggest.
- End on a hold, not a cut-away: the last image of the story stays on
  screen. An end card (title, slogan, logo lockup, "bright end-frame") is
  ad grammar. First match wins: (1) the user asked for a card or logo
  frame → add it, in any mode; (2) music_only with a quoted line and text
  none → the end-card TEXT: from "Fixed inputs"; (3) `tv_spot` /
  `app_demo` → their end card or CTA; with text none, a textless end
  frame instead (product or logo as an image, no words, no written CTA);
  (4) anything else — explainer, story, documentary — no card, and
  "payoff" means the idea landing. A card never replaces the held image:
  the image holds first, then the card closes the final beat, inside the
  fixed duration.
- Two asks ("X, then at the end Y") → two acts inside the same beats.
- "Three variations" → three calls, three distinct prompts, same timing;
  change only talent / wardrobe / setting as asked.

## Dialog and sound

- Spoken copy: if they asked for a line or brand, write it **verbatim**.
  Pace ≈ 2.2 words/sec against the cut it plays over.
- Narration you write (sound: narration, when the shot list has no line):
  one line per beat, ≈2.2 words per second of that beat (a 3s beat ≈ 6
  words), present tense, plain words, no brand claims they did not make.
  The lines read as one thought in order; the last one lands the idea
  ("It's already over."). The duration stays fixed: write only the words
  the idea needs within it. When the lines run short, the remaining beats
  play under the bed with no VO — never stretch, repeat, or pad lines to
  fill the length, and never change the duration to fit them.
- Dialogue (sound: dialogue): the speaker is on screen in that cut, named
  with their identity block; quote the line; one speaker per cut. A
  presenter talks to camera: eye contact, natural gestures, pauses.
- Music / SFX: if they named a genre or feel, put it in AUDIO (or the beats
  on SHORT). Set `enable_audio` only when the model supports audio. With a
  voice, the bed stays under it; add a click / whoosh when an overlay or a
  TEXT: lands.
- Never leave the prompt empty or mood-only.

## On-screen text

brief.text picks one look; TEXT STYLE: states it once and every beat's
TEXT: follows it. Text appears only where a TEXT: says so.

Video models render type badly: dense lettering, boxes, arrows, and
numbers come out garbled. Keep text light — plain bold sans-serif, high
contrast, one color, no frames or arrows unless the user asked — and
never put a different supplemental text (keywords, labels) on every beat
when the voice already says it. Subtitles are exempt: they follow every
spoken line.

- **subtitles** — the spoken line, whole, under the picture. Bold white
  sans-serif, centered near the bottom on a semi-transparent dark bar; each
  subtitle on screen exactly while its line is spoken. The default with
  narration.
- **keywords** — one or two bold words that pop as the voice says them.
  Large uppercase white sans-serif, high contrast, upper third or centered;
  each word appears on its syllable and leaves on the next cut.
- **labels** — a short caption (≤3 words) beside the thing it names, on at
  most three beats, not every one. Bold condensed sans-serif, one accent
  color; each label appears with its fact and stays for the beat.
- **none** — nothing. The continuity rule "no text" holds, except the
  music_only end card for a quoted line (see "Fixed inputs"). A `tv_spot`
  / `app_demo` with none closes on a textless end frame ("End on a hold").

Timing: a TEXT: lands with the words it echoes (subtitles / keywords) or
with the fact it names (labels), never before. Text is never the only
carrier of a fact the picture should show.
