# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical acoustic-stealth maze thriller built for the **ODS Weekly Dev Jam**.

[![Built with ODS Portal](https://img.shields.io/badge/Built%20With-ODS%20Portal-7c3aed.svg)](https://github.com/Osmantic/ODS)
[![Tests](https://github.com/vaibhavsrv/echo-maze/actions/workflows/test.yml/badge.svg)](https://github.com/vaibhavsrv/echo-maze/actions/workflows/test.yml)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0%20(Pure%20Vanilla)-success.svg)]()
[![Single File Portable](https://img.shields.io/badge/Format-Single%20HTML%20File-cyan.svg)]()

- 🎮 **Play Live Online (Main Game):** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)  
- 🌲 **Play Echo Grove Prototype (Pass 4 Variant):** [https://vaibhavsrv.github.io/echo-maze/grove.html](https://vaibhavsrv.github.io/echo-maze/grove.html)
- 🤖 **Engineered In:** Built 100% inside **ODS Portal** with local autonomous agent execution.
- 🧠 **Local Model:** Qwen2.5-Coder-3B-Instruct (Open-source model $\le$ 180B parameters, running locally via ODS Portal + Pixel; ~2.5 GB memory footprint fitting the 8 GB unified memory budget of Apple Silicon M1).
- 📜 **Prompt Evolution Log (55 Turns with Portal Dropdowns):** [PROMPTS.md](PROMPTS.md)
- 💾 **Portal Session Export:** [`ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json`](ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json) (Native ODS Portal conversation export, re-importable in any ODS Portal instance).
- 📦 **Downloadable Chat Archive:** [`echo-maze-ods-portal-chats.zip`](echo-maze-ods-portal-chats.zip)

---

## 🏛️ Zero-Dependency Architectural Manifesto

> *"Constraints breed elegance."*

**ECHO MAZE** was deliberately designed under the strict constraints of the **demoscene and JS13k** development philosophy:

1. **Zero External Dependencies**: Zero npm packages, zero canvas frameworks (Pixi/Phaser), zero bundlers (Webpack/Vite), and **zero external audio files**. 
2. **Procedural Web Audio Synthesis**: All 8 audio effects (440 Hz exponential sonar sweeps, 80 ms acoustic reflection clicks, 60 Hz biological double-thump heartbeats, LFO sawtooth monster groans, alert stings) are synthesized **live from mathematical oscillators and gain nodes**.
3. **Pure Portability & Offline Execution**: The entire engine, simulation loop, asset renderer, particle pool, and test runner reside in a single self-contained `index.html`. It runs identically in any modern desktop or mobile browser without network requests.
4. **Deterministic Simulation**: A custom seeded Mulberry32 PRNG ensures that identical seeds (`?seed=N`) produce bit-for-bit identical labyrinths, enemy spawn coordinates, and patrol cycles.

---

```
                       [ 📡 SONAR WAVEFRONT ]
            . . . . . . . . . . . . . . . . . . . . . . .
            .   # # # # # # # # # # # # # # # # # # #   .
            .   # [P] . . . . . . . . . . . . . # [C] # .
            .   # . # # # # . # # # # # # # . # . . #   .  <-- Red/Blue Siren
            .   # . #   ( ( ( 440Hz ) ) )   # . # . #   .      Searchlight
            .   # . # # # # . # # # # # # # . # # . #   .
            .   # . . . . . . [🔔 DECOY] . . . . . [EXIT]#.
            .   # # # # # # # # # # # # # # # # # # #   .
            . . . . . . . . . . . . . . . . . . . . . . .
               "Sound illuminates the dark... but summons the hunters."
```

---

## ⚡ 15 Unique Levels & Thriller Mechanics (V1.1)
- **1-Block Tactical Movement**: Discrete grid hopping with ease-out tweening and responsive squash-and-stretch physics.
- **The Pitch-Black Void Trail**: Everywhere you move, the path behind you collapses into an abyssal pitch-black void.
- **Scaling Cop Patrols (1 to 5 Cops)**: Cops multiply as you advance! Corner drones spawn with 360° sweeping volumetric red & blue siren searchlights, pulse scanners, and emergency lightbars.
- **Dynamic Suspense Blackout Cycle**: High-voltage electrical power cuts plunge the labyrinth into total darkness with warning flickers and acoustic surges.
- **Anti-Clockwise Countdown Dial**: A cyber radial dial sweeps counter-clockwise with mechanical audio ticking and emergency lockdown upon hitting 0.
- **Progressive Acoustic Hints**: 3-tier escalation pipeline (exit hum volume boost, perimeter arc, illuminated safe path avoiding enemies by $\ge 4$ tiles). Mode selectable via `SettingsManager` (`OFF`, `SUBTLE`, `GUIDED`).
- **Tactical Decoys**:
  - **Brass Decoy Bell (`E`)**: Reusable 2 times per level with an enforced 6-second cooldown.
  - **Acoustic Pebble Raycast (`F`)**: Casts up to 5 walkable tiles ahead to create a targeted acoustic distraction (2 pebbles per level).
- **Subtle Alert Levels (`●○○○` to `●●●●`)**: Real-time facility threat level indicators tracking patrolling, investigation, and pursuit states.
- **Deterministic Stealth Ratings (S/A/B/C)**: Derived strictly from recorded player events in the event log (investigations triggered, sonar pings vs par, hints used).
- **Sequential Campaign (1 &rarr; 15)**: Levels must be cleared in strict linear sequence ($1 \to 2 \to 3 \to 4 \dots \to 15$).
- **Cross-Device Ready**: Full support for desktop keyboard (WASD / Arrows), touch D-Pad, and touch action buttons on mobile devices.

---

## 🗺️ Campaign Overview (15 Levels)

| Level | Name | Grid | Cops | Thriller & Unique Features |
| :---: | :--- | :---: | :---: | :--- |
| **L1** | Rookie Patrol | 11×11 | 1 Cop | Baseline patrol, infinite energy, 35s timer |
| **L2** | Dormant Ear | 13×13 | 1 Cop | Sleeper cop awakens on sonar pings |
| **L3** | Wandering Shadow | 15×15 | 1 Cop | Cop hears uncrept footsteps (Radius 2.5 tiles) |
| **L4** | Resonant Decoy | 17×17 | 1 Cop | Brass decoy bell distraction (`E`) |
| **L5** | Twin Enforcement | 19×19 | 2 Cops | 2 Cops in opposite corners with staggered sirens |
| **L6** | Blackout Surge | 21×21 | 2 Cops | 2 Cops + rapid electric blackouts |
| **L7** | Crossfire Sector | 21×21 | 2 Cops | 2 Fast Interceptors + Decoy Bell |
| **L8** | Triple Threat | 23×23 | 3 Cops | 3 Cops in 3 corners with rotating lightbars |
| **L9** | Radar Pulse Ambush | 23×23 | 3 Cops | Scanner Drone sweeps counter-sonar pulses |
| **L10** | Gauntlet Run | 25×25 | 3 Cops | 3 High-speed Vipers (Speed 3.8!) |
| **L11** | Quadrant Lockdown | 27×27 | 4 Cops | 4 Cops seal ALL four corners of the perimeter |
| **L12** | Acoustic Reaper | 27×27 | 4 Cops | Extreme acoustic hearing (Radius 3.8 tiles!) |
| **L13** | Eclipse Protocol | 29×29 | 4 Cops | 4 Cops + 2 Pulse Scanners + deep blackout |
| **L14** | Apex Swarm | 31×31 | 5 Cops | 5 Cops swarming mega-labyrinth (4 corners + center stalker) |
| **L15** | Grand Escape: Final Lockdown | 33×33 | 5 Cops | 5 Apex Hunters (Speed 3.9) + multi-pulse radar! |

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

## 🧪 Dual Automated Verification Suites

### 1. Headless Continuous Integration (`node:test`)
A dedicated headless test suite executes the core mathematical simulation in CI on every commit:
```bash
node --test tests/invariants.test.mjs
```
```text
✔ 1. PRNG determinism & reproducibility (Mulberry32) (2.3ms)
✔ 2. BFS maze full connectivity invariant across 200 procedural seeds (14.3ms)
✔ 3. Exit distance invariant (exit >= 60% of max BFS graph depth) (11.5ms)
✔ 4. Energy budget & single-charge ping invariant (0.3ms)
✔ 5. Monster navigation never enters wall cells across 5,000 simulated frames (6.5ms)
✔ 6. Acoustic event propagation and hearing radius discrimination (0.1ms)
✔ 7. Continuous pathing to tactical decoy bell without teleportation (0.2ms)
✔ 8. Line-of-sight raycasting correctness (0.1ms)
✔ 9. Campaign progression monotonicity (15 levels, 1 to 5 cops, expanding grids) (0.4ms)
✔ 10. Multi-monster state isolation (zero shared mutable state) (0.1ms)
ℹ tests 10 | pass 10 | fail 0 | duration_ms 149ms
```

### 2. In-Browser Runtime Self-Tests (`runSelfTests()`)
Includes **20 automated runtime self-tests** inside `index.html` (validating **52 mathematical and simulation invariants** across 200 random seeds) and **16 automated self-tests** in `grove.html`, executed directly on canvas initialization or via URL parameter `?debug=1`:
- BFS maze connectivity across 200 random seeds (zero unreachable floor cells).
- Optimal exit distance thresholds ($\ge 60\%$ max BFS depth).
- PRNG deterministic reproducibility (Mulberry32 bit-for-bit parity).
- Monster FSM hearing radii and BFS corridor acoustic bounds.
- Sonar line-of-sight raycasting and shell expansion immunity.
- 150-particle pooled ceiling and telemetry budgets (< 8.0 ms average frame time).
- High score and deterministic stealth rating calculations (S/A/B/C).
- Closed secret wall reachability and audio lifecycle safety.
