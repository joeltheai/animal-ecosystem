# animal-ecosystem

A single-file, dependency-free **procedural Rain World character study**.

Open `index.html` in any modern browser. No build step, no packages, no assets.
Everything you see is drawn and animated at runtime on a canvas: the slugcat, the
slugpup, the rain, the ruins, the tail physics, and the wobble in the grass.

The first version of this file was a wireframe lizard test. This one is a
from-scratch rework of the character, the rig, the locomotion, and the world
around them.

## Controls

| Input | Action |
| --- | --- |
| `A` / `D` or `←` / `→` | Move |
| `Shift` | Sprint |
| `Space`, `W`, or `↑` | Jump — hold for a higher jump |
| `S` or `↓` | Slide (while running) / crouch |
| Mouse | Look — head, eyes, and ears follow |
| `Q` | Squeak (the pup reacts) |
| `R` | Reset the world |
| `V` | Toggle the procedural rig view |
| `H` | Toggle HUD |
| `M` | Toggle sound |
| `F` | Fullscreen |

Sound starts on the first key press or click. The slugpup follows you on its own.

## What's going on

### The slugcat (and the pup)

The character is built from a tiny physics core plus cosmetic layers, following
Rain World's split between simulation and decoration:

- **Body** — a compact path-following torso with procedural ripple, a leaning
  upright/scurrying pose, breathing, squash and stretch, and soft socket shading.
  The chest is high, the hips low, and the body shifts onto two feet when idle.
- **Hind legs** — analytic two-bone IK with planted feet. Steps are triggered by
  displacement, predicted for the speed, coordinated so one diagonal pair stays
  on the ground, and lifted in a smooth arc with dust and little landing puffs.
- **Arms** — separate cosmetic IK limbs that swing during the scurry, tuck during
  short jumps, and reach/flail toward the ground after a long fall. The air-flail
  meter grows with time in the air and speed, as described in the Rain World
  devlog (Update 145).
- **Tail** — a Verlet chain with declining stiffness toward the tip, ground
  collision, a lazy idle curl, and a running lift. This is the "four balls of
  physics and thirteen triangles" idea from the devlog, done in canvas.
- **Head** — a spring-followed head with tilt, mouse tracking, blinks, a squeak
  mouth, and two damped ear springs that twitch, sweep back when falling, and
  react to squeaks and landings.
- **Pup** — the same rig at 0.62 scale, driven by a small follow AI. It trots
  behind, looks at you, jumps when you jump or squeak, and catches up smoothly.

Movement has acceleration, skidding, sprint, slope gravity, coyote time, a jump
buffer, variable jump height, slide momentum, a slide jump, and ground snapping
on gentle downhills.

### The world

- Endlessly generated rolling terrain with one-way industrial platforms.
- Three parallax layers of ruins, a viaduct, pipes, fans, catwalks, and cables.
- A framing foreground overhang and ledge, with reeds and hanging vines.
- Rain with ground ripples, splashes, drifting fog, dust, motes, batflies, and
  occasional lightning with thunder.
- Procedural audio: rain, wind, a low drone, footsteps, jump/land, squeak, thunder.

Press `V` to see the actual rig: body path, IK chains, planted-foot targets,
tail chain, velocity vector, and ground contact.

## Tweaking it

Everything is in `index.html`, in one script. The useful knobs are near the top:

- `walkSpeed`, `runSpeed`, `gravity` — general movement feel.
- `TERRAIN_BASE` — the world's ground line.
- `Slugcat` constructor / `reset` — body size (`scale`), spine length and radii,
  leg lengths, tail length and thickness.
- `updateSpine`, `updateLegs`, `updateTail`, `updateFace` — the animation layers.
- `makeBolt`, `drawFarSkyline`, `drawMidRuins`, `drawViaduct`, `drawGround`,
  `drawPlatform` — environment generation and art.
- `window.__slug` in the console exposes the player, pup, camera, and world
  helpers for tinkering.

The character and world are deterministic enough to feel like a place, but the
small details are intentionally organic and slightly different each run.

## References

This is a fan-made procedural study, not affiliated with Videocult or Akupara.
No Rain World sprites or assets are used; the visuals are original canvas shapes
generated at runtime.

- Joar Jakobsson & James Therrien, **The Rain World Animation Process** (GDC talk)
  https://www.youtube.com/watch?v=sVntwsrjNe4
- Rain World devlog, especially the player/limb and tail updates:
  https://candlesign.github.io/Rain-World-Devlog/
- Unity, **Exploring procedural design in Rain World: The Watcher** — the
  two-spherical-chunk model and cosmetic limbs:
  https://unity.com/blog/exploring-procedural-design-rain-world
- Rain World Wiki, **Slugcat** and **Movement**, for the body modes, sprite
  anatomy, and control context:
  https://rainworld.miraheze.org/wiki/Slugcat
  https://rainworld.miraheze.org/wiki/Movement

## Notes

- The game is a sandbox: there is no score, death, or rain cycle timer. The rain
  is weather, not a fail state.
- It is designed for a keyboard and mouse. On narrow screens the HUD collapses
  the control hints, but the rig itself is the same.
- If sound does not start, click the page once; browsers require a gesture.
