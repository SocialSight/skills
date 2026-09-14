# Photographic modes

Write every prompt in full. Name the lens (or focal length and distance), the light quality (softbox, hard noon, window bounce), the surface the product sits on or against, and the atmosphere (haze, air, color temperature). Short adjective stacks fail on this API because nothing downstream expands them.

Pass product or identity stills as `params.medias[].value` after importing them (skill `media-refs`). `generate_image` ignores `role`.

## Studio product shot

**Framing.** Centered or slight three-quarter. Product occupies most of the frame; leave clean margin for crop. Eye-level or a touch above. No crop through logos or silhouettes.

**Lighting.** Large soft source camera-left, weaker fill camera-right, tight rim to separate from the sweep. Controlled speculars on glass and metal — one highlight, not a cluster.

**Background.** Seamless sweep, cyc, or a single material plane (matte acrylic, stone, paper). No room set, no props unless the brief names them.

**Example prompt.**

> 85mm still-life on a full-frame body, camera two metres back, product dead-center with a 10% margin. Softbox from camera-left at 45°, large diffusion so the key is wraparound and the shadow under the base is a short, soft gradient. Weak bounce fill from the right. Narrow white rim from behind to lift the silhouette. Matte dove-grey paper sweep, no horizon line, no texture in the field. Cool daylight 5600K, no haze, no vignette, catalog sharpness. The bottle stands on its own base; label type remains legible; glass shows one clean specular.

## Lifestyle scene

**Framing.** Environmental medium shot. Product in use or at rest in a real room, street, or landscape. Leave space for context; do not isolate on white.

**Lighting.** Motivated practicals plus a shaped key: window light, late sun, kitchen overhead. Mixed color is allowed if it matches the room.

**Background.** Lived-in interior or exterior with depth. Background slightly soft; product plane sharp.

**Example prompt.**

> 35mm on a full-frame body, waist-level, the ceramic mug on a sunlit oak table in a small apartment kitchen. Window key from camera-left, sheer curtain diffusion, warm 4000K interior mix from a pendant over the sink. Shallow depth so the stove and tiled backsplash fall off. Visible air, light dust in the sunbeam, condensation on the mug. Hands out of frame. Natural color, slight film grain, no studio sweep, no catalog retouching.

## Close-up with hands

**Framing.** Tight crop: product plus hands or a partial face. Fingernails and skin texture in focus. Crop at wrist or mid-forearm, not at the joint awkwardly.

**Lighting.** Beauty dish or small soft source close to the subject. Catchlights in any visible eye; skin rendered, not plastic.

**Background.** Falloff to a simple tone or implied interior. No busy print that competes with skin.

**Example prompt.**

> Macro-adjacent 100mm, close enough to read the texture of the cream as a finger presses it into the back of a hand. Soft beauty light from camera-right, large source, faint fill from below to open the shadows under the knuckles. Neutral grey paper behind, out of focus. Skin pores visible, no frequency-separation look. Cool-neutral 5200K, high detail on the product lettering, quiet atmosphere, no extra props.

## Moodboard

**Framing.** Vertical 2:3 (or the catalog ratio closest to a pin). Art-directed still that could sit on a moodboard: overlapping objects, fabric, fragment of architecture, not a single packshot.

**Lighting.** Directional and graphic — hard slat light, colored gel, or overcast north light. Mood first, catalog second.

**Background.** Layered materials: linen, stone, torn paper, foliage. Allow negative space for type later.

**Example prompt.**

> Vertical 2:3 still, 50mm, slightly elevated. A pair of sunglasses rests on crumpled ivory linen beside a torn gallery card and a sprig of olive. Hard afternoon light through a slatted blind, striped shadow across the cloth, warm 4500K. Shallow depth, tactile fabric weave sharp in a narrow band. No logo lockup, no studio cyc. Atmosphere of a stylist's table the hour before a shoot.

## Hero banner

**Framing.** Wide (16:9 or 21:9 from the catalog). Subject on the visual third; empty field for headline. Horizon low or high on purpose, never through the product.

**Lighting.** Cinematic key, controlled flare optional. Separate subject from the field with rim or haze.

**Background.** Location or gradient built for type. Keep a quiet region where a headline could sit.

**Example prompt.**

> 24mm wide on a full-frame body, 16:9, camera at hip height. The sneaker stands on wet asphalt at dusk, placed on the right third, left two-thirds a deep navy gradient of empty street and sodium haze. Hard rim from behind-right, soft fill from a large source camera-left so the mesh and sole remain readable. Light rain, reflective ground, 3200K practicals in the distance, no people. Sharp product, atmospheric background, space on the left for a campaign headline. No on-image text.

## Editorial

**Framing.** Fashion/magazine: full length, three-quarter, or graphic crop. Gesture and wardrobe matter as much as the product.

**Lighting.** Stylized: beauty, hard flash, or available darkness. Color grade is part of the prompt (bleach, tungsten, cross-process), not an afterthought.

**Background.** Set, seamless, or location with an opinion. Avoid generic stock interiors.

**Example prompt.**

> 85mm editorial portrait, three-quarter, subject stepping through a lime-green seamless. Hard fashion flash from camera-left, crisp shadow on the cyc, small silver bounce on the face. 6400K, slightly overexposed whites, visible grain. Wardrobe and the held product share the same plane of focus; the far edge of the seamless falls off. No smile-for-catalog energy; locked gaze; magazine finish, not ecommerce.
