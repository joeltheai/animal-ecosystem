# animal-ecosystem

A single-file, dependency-free **procedural slugcat rig study**.

Open `index.html` in a browser. There is no build step and no external assets.
The stage is intentionally flat: one creature, one ground line, nothing else.
Everything is drawn with circles, lines, and a handful of simple shapes so the
motion is easier to read.

## Controls

| Input | Action |
| --- | --- |
| `A` / `D` or `←` / `→` | Move |
| `Shift` | Run |
| `Space` / `W` / `↑` | Jump — hold for a higher jump |
| `R` | Reset |
| `H` | Hide HUD |
| `F` | Fullscreen |

The character starts seated. Move and it drops onto all fours; stop and it sits
back up. There is no scenery, follower, water, audio, or collectible to distract
from the animation.

## What is being simulated

The character follows the split Rain World uses between simulation and cosmetic
animation:

- **Two-chunk core** — a hip and a chest, connected by a spine whose elevation is
  driven by a damped spring. The body leans with speed, loosens in the air, and
  the chest/head lag behind the hips. This is the part that actually moves the
  creature around.
- **Paper-doll torso** — the visible body is a short chain of overlapping circles
  drawn over the two-chunk core. A single dark pass behind them gives the body one
  outer silhouette, with a separate head circle, ears, eyes, and nose.
- **Four cosmetic IK limbs** — thin arms and short legs with two-bone IK, joints
  drawn as circles. Feet stay planted while the body moves, then take a step when
  they fall too far behind. A small scheduler alternates the two diagonal pairs so
  one pair is always planted; no all-four-legs-in-the-air glitch.
- **Tail** — a Verlet chain with stiffness that falls off toward the tip, plus
  ground collision and a cosmetic traveling wave. It drags when running, curls up
  when seated, and streams behind in the air.
- **Stand / crawl** — when idle, the chest rises, the head settles above it, and
  the front paws hang. When movement starts, the body drops toward the ground,
  the paws lower, and the walk cycle takes over.
- **Jump and fall** — variable jump height, coyote time, a jump buffer, a stretch
  on takeoff, a squash on landing, tucked limbs on short jumps, and reaching /
  flailing arms and legs after a long fall.
- **Face** — blinks, damped ears that flop with movement and air, and a tiny
  head-tilt reaction to vertical velocity.
- **Step follow-through** — each footfall pumps a small vertical spring in the
  body, and the whole rig squashes around the floor on impact.

The body/limb positions are recomputed every frame. There is no walk-cycle sprite
sheet and no keyframed skeleton animation.

## Tweaking

Everything is in `index.html`:

- `RIDE`, `BODY_LEN`, `HEAD_R`, `HIP_R` — the character's proportions.
- `WALK`, `RUN`, `GRAVITY` — locomotion feel.
- `Slugcat.update` — acceleration, jump, stand blend, spine spring, squash.
- `Slugcat.updateLeg` / `startStep` / `scheduleGait` — foot planting, step arcs,
  diagonal-pair alternation, and stride length.
- `Slugcat.updateTail` / `drawTail` — the physical tail and its cosmetic wave.
- `drawBody` / `drawHead` — the paper-doll body and face.
- `ZOOM`, `FLOOR_SCREEN` — the framerate-independent stage framing.

`window.__slug` exposes the rig, camera, and reset function in the console.

## Reference notes

- Joar Jakobsson & James Therrien, **The Rain World Animation Process** (GDC):
  https://www.youtube.com/watch?v=sVntwsrjNe4
- **Rain World Devlog**, especially the tail-stiffness and long-fall arm updates:
  https://candlesign.github.io/Rain-World-Devlog/
- Unity, **Exploring procedural design in Rain World: The Watcher** — the
  two-spherical-chunk model and cosmetic limbs:
  https://unity.com/blog/exploring-procedural-design-rain-world
- Rain World Wiki, **Movement** — upper/lower chunks, body modes, animations:
  https://rainworld.miraheze.org/wiki/Movement
- Rain World Wiki technical movement pages:
  https://rainworld.miraheze.org/wiki/User:Alphappy/Live/Movement

This is a fan-made procedural study, not affiliated with Videocult or Akupara.
No Rain World sprites or game assets are used.
