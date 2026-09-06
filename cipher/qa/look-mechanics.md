# Cipher look mechanics

Cipher keeps both boots and his lower torso anchored. His eyes lead each gaze, followed by small eyelid and eyebrow changes, a near-rigid head and neck turn, and restrained shoulder follow-through. His skull, facial proportions, jacket, hands, and panel must not stretch or warp.

The digital panel is rigid and attached at his forearm. It follows the arm and torso with a slight continuous lag, stays connected, and changes occlusion naturally as he turns. Its abstract blocks remain unreadable. Jacket trim, circuitry, and interface glow keep their clean, separate color regions.

Motion budget: each 22.5-degree step changes the eyes, head angle, shoulder visibility, and panel angle by one even increment. Boots, baseline, body scale, and lower-body registration remain stable. No adjacent pose may flip the panel side, change its size, or jump the head position.

- `000 up`: pupils and upper eyelids aim upward; chin lifts slightly; more collar underside is visible; shoulders stay balanced; panel tips back a little while remaining attached.
- `090 screen-right`: pupils, nose, face, and head turn unmistakably toward the image's right edge; the left side of his head and jacket becomes more visible; the far cheek and far eye narrow; panel follows the rightward torso turn and becomes slightly more side-on.
- `180 down`: pupils and eyelids aim down; chin lowers toward the collar; upper hair mass becomes more visible; shoulders round slightly; panel tilts inward beneath his gaze.
- `270 screen-left`: pupils, nose, face, and head turn unmistakably toward the image's left edge; the right side of his head and jacket becomes more visible; the far cheek and far eye narrow; panel follows the leftward torso turn and becomes partly occluded by the forearm.

Diagonals interpolate those four families in clockwise viewer coordinates. Eyes remain inside their original anime eye apertures, and head turns use near-rigid anatomy rather than raster warping. The loop closes with `337.5` one even step before `000`.
