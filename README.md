# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical acoustic-stealth maze thriller with a bioluminescent Green Grove aesthetic.

- 🎮 **Play Live Online:** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)  
- 🧪 **Run Automated Self-Tests:** [https://vaibhavsrv.github.io/echo-maze/?test=1](https://vaibhavsrv.github.io/echo-maze/?test=1)

---

## 🌲 Visuals & Green Grove Theme
- **Bioluminescent Green Grove Palette**: High-contrast near-black emerald background (`#030805`), radiant player core (`#eafff1`) with soft emerald halo (`#4dff9a`), sonar wave band (`#39ff88`) with echo ring (`#86efac`), and 2.5D moss-capped stone labyrinth walls.
- **Colorblind Accessible Design**: Non-color-exclusive identifiers across all systems:
  - Solid circular rings for player sonar pings vs. jagged/dashed outlines (`[8, 6]`) for monster threat wavefronts.
  - Distinct geometric predatory eyes and hexagonal armored chassis for patrol drones.
  - Dedicated text state banners (`"HEARD."`, `"SIREN"`, `"LOCKED n/m"`, `"KEYS n/m"`).
  - High-contrast text exceeding WCAG AAA standards (15.5:1 for HUD text, 16.8:1 for headers).
- **High Performance Offscreen Sprites**: Zero `shadowBlur` and zero per-frame canvas filters; offscreen pre-rendered sprites for player halo, key auras, exit vortex, and ambient vignette ensure smooth 60 FPS gameplay (< 1.5 ms CPU frame times).

---

## ⚡ Core Thriller & Stealth Mechanics
- **1-Block Tactical Movement**: Responsive discrete grid hopping with ease-out tweening and squash-and-stretch physics.
- **The Pitch-Black Void Trail**: Everywhere you move, the stone pathway behind you collapses into an abyssal pitch-black void.
- **Keys and Locked Exit Doors**:
  - Seeded deterministic key placement on walkable cells satisfying strict minimum path distance requirements from start, exit, and monster spawns.
  - Reachability contract: all keys are guaranteed reachable without passing through the exit door.
  - Key visibility: revealed when crossed by a line-of-sight sonar ring (3.5s fading glint) or within close player proximity (1.5 tiles).
  - Exit doors remain firmly locked with a golden padlock barrier until all required keys for the level are acquired.
- **Siren Acoustic Propagation**:
  - Picking up a key triggers a 3-pulse siren alarm (1 initial + 2 follow-ups at 1-second intervals) positioned at the key's exact location.
  - Monsters within the BFS hearing radius (18 path tiles) investigate the alarm site. Sleeping monsters awaken.
- **Dynamic Suspense Blackout Cycle**: High-voltage electrical power cuts plunge unvisited corridors into total darkness with electrical flickers and acoustic surges.
- **Anti-Clockwise Countdown Dial**: Radial timer sweeps counter-clockwise with mechanical ticking and emergency lockdown upon expiration.
- **Progressive Acoustic Hints**: 3-tier escalation pipeline (exit hum volume boost, perimeter safe arc, safe route breadcrumbs avoiding enemies by $\ge 4$ tiles). Mode selectable via `SettingsManager` (`OFF`, `SUBTLE`, `GUIDED`).
- **Tactical Decoys**:
  - **Brass Decoy Bell (`E`)**: Reusable 2 times per level with a 6-second cooldown.
  - **Acoustic Pebble Raycast (`F`)**: Casts up to 5 walkable tiles ahead to trigger a distraction (2 pebbles per level).
- **Deterministic Stealth Ratings (S/A/B/C)**: Evaluated from player event logs, ping allowances (including key finding allowances), and hint usage.

---

## 🗺️ Campaign Overview (15 Levels)

| Level | Name | Grid | Cops | Keys | Thriller & Unique Features |
| :---: | :--- | :---: | :---: | :---: | :--- |
| **L1** | Rookie Patrol | 11×11 | 1 Cop | 1 Key | Baseline patrol, infinite energy, 35s timer |
| **L2** | Dormant Ear | 13×13 | 1 Cop | 1 Key | Sleeper cop awakens on sonar pings |
| **L3** | Wandering Shadow | 15×15 | 1 Cop | 2 Keys | Cop hears uncrept footsteps (Radius 2.5 tiles) |
| **L4** | Resonant Decoy | 17×17 | 1 Cop | 2 Keys | Brass decoy bell distraction (`E`) |
| **L5** | Twin Enforcement | 19×19 | 2 Cops | 3 Keys | 2 Cops in opposite corners with staggered sirens |
| **L6** | Blackout Surge | 21×21 | 2 Cops | 3 Keys | 2 Cops + rapid electric blackouts |
| **L7** | Crossfire Sector | 21×21 | 2 Cops | 3 Keys | 2 Fast Interceptors + Decoy Bell |
| **L8** | Triple Threat | 23×23 | 3 Cops | 3 Keys | 3 Cops in 3 corners with rotating searchlights |
| **L9** | Radar Pulse Ambush | 23×23 | 3 Cops | 3 Keys | Scanner Drone sweeps counter-sonar pulses |
| **L10** | Gauntlet Run | 25×25 | 3 Cops | 3 Keys | 3 High-speed Vipers (Speed 3.8!) |
| **L11** | Quadrant Lockdown | 27×27 | 4 Cops | 3 Keys | 4 Cops seal ALL four corners of the perimeter |
| **L12** | Acoustic Reaper | 27×27 | 4 Cops | 3 Keys | Extreme acoustic hearing (Radius 3.8 tiles!) |
| **L13** | Eclipse Protocol | 29×29 | 4 Cops | 3 Keys | 4 Cops + 2 Pulse Scanners + deep blackout |
| **L14** | Apex Swarm | 31×31 | 5 Cops | 3 Keys | 5 Cops swarming mega-labyrinth |
| **L15** | Grand Escape: Final Lockdown | 33×33 | 5 Cops | 3 Keys | 5 Apex Hunters (Speed 3.9) + multi-pulse radar! |

---

## 🕹️ Controls
| Action | Desktop Keyboard | Mobile / Touchscreen |
| :--- | :--- | :--- |
| **Move 1 Block** | `W`, `A`, `S`, `D` or `Arrow Keys` | On-Screen D-Pad or Swipe on Canvas |
| **Sonar Ping** | `Spacebar` | `📡 PING` Button or Tap Canvas |
| **Decoy Bell** | `E` | `🔔 BELL` Button (2 uses, 6s cooldown) |
| **Pebble Decoy** | `F` | `🪨 PEBBLE` Button (2 uses, 5-tile raycast) |
| **Creep (Silent)** | `Shift` | Built-in assist mode |
| **Pause** | `P` or `Escape` | `⏸ PAUSE` in Header |
| **Restart Level** | `R` | Menu button |

---

## 🧪 Automated Verification (51 Self-Tests)
Includes a comprehensive test suite embedded directly in `index.html` verifying:
1. BFS maze connectivity across 200 random seeds.
2. Optimal exit distance thresholds ($\ge 60\%$ max BFS depth).
3. PRNG deterministic reproducibility (Mulberry32).
4. Monster FSM hearing radii and corridor acoustic bounds.
5. Sonar line-of-sight raycasting.
6. Key placement validity and door-blocked reachability.
7. Key glint line-of-sight and proximity glow.
8. Siren event localization and multi-pulse propagation.
9. Hint safety against live monster positions.
10. **Color Audit**: Strict enforcement of zero unlisted hex or rgb color literals outside `CONFIG.THEME` and CSS variables.

Run tests in any modern browser by loading `index.html?test=1`.
