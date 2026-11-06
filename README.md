# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical acoustic-stealth maze thriller built for the **ODS Weekly Dev Jam**.

[![Built with ODS Portal](https://img.shields.io/badge/Built%20With-ODS%20Portal-7c3aed.svg)](https://github.com/Osmantic/ODS)
[![Tests](https://github.com/vaibhavsrv/echo-maze/actions/workflows/test.yml/badge.svg)](https://github.com/vaibhavsrv/echo-maze/actions/workflows/test.yml)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-0%20(Pure%20Vanilla)-success.svg)]()
[![Single File Portable](https://img.shields.io/badge/Format-Single%20HTML%20File-cyan.svg)]()

- 🎮 **Play Live Online (Main Game):** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)
- 🌲 **Play Echo Grove Prototype (Visual Variant):** [https://vaibhavsrv.github.io/echo-maze/grove.html](https://vaibhavsrv.github.io/echo-maze/grove.html)
- 🤖 **Development Environment:** Built collaboratively in **ODS Portal** using a human-in-the-loop co-design workflow (Lead Designer & Playtester with local agent assistance).
- 🧠 **Local Model Engine:** `Qwen2.5-Coder-3B-Instruct` (Local open-source model $\le 180\text{B}$ parameters, 4-bit quantization, running locally within memory constraints).
- 💻 **Host Environment:** Apple Silicon (MacBook Air M1, 8 GB Unified Memory, macOS Darwin arm64).
- 📜 **Prompt Evolution Log (55 Turns across 14 Stages):** [PROMPTS.md](PROMPTS.md)
- 💬 **Complete Verbatim Transcript:** [CHATS_TRANSCRIPT.md](CHATS_TRANSCRIPT.md)
- 💾 **Portal Session Export:** [`ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json`](ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json) (Normalized ODS Portal schema v1 conversation export, fully importable in any ODS Portal instance via `parseConversationImport`).
- 📦 **Downloadable Chat Archive:** [`echo-maze-ods-portal-chats.zip`](echo-maze-ods-portal-chats.zip)

---

## 1. Game Overview

**ECHO MAZE** places the player in total sensory darkness inside a procedurally generated labyrinth. There are no ambient lights, no mini-maps, and no overhead navigation aids.

To navigate, the player emits an expanding acoustic sonar pulse. The pulse sweeps through corridors, illuminating walls through true line-of-sight raycasting. However, sound waves do not stop at walls—they propagate across corridor paths to the ears of patrolling security drones and hound units. Every sonar ping reveals your surroundings, but simultaneously broadcasts your exact coordinates to nearby hunters.

Surviving requires spatial memory, acoustic discipline, and tactical misdirection.

---

## 2. Gameplay Mechanics

```
                       [ 📡 SONAR WAVEFRONT ]
            . . . . . . . . . . . . . . . . . . . . . . .
            .   # # # # # # # # # # # # # # # # # # #   .
            .   # [P] . . . . . . . . . . . . . # [C] # .
            .   # . # # # # . # # # # # # # . # . . #   .  <-- Red/Blue Siren
            .   # . #   ( ( ( 480Hz ) ) )   # . # . #   .      Searchlight
            .   # . # # # # . # # # # # # # . # # . #   .
            .   # . . . . . . [🔔 DECOY] . . . . . [EXIT]#.
            .   # # # # # # # # # # # # # # # # # # #   .
            . . . . . . . . . . . . . . . . . . . . . . .
               "Sound illuminates the dark... but summons the hunters."
```

- **1-Block Tactical Movement:** Discrete grid hops with ease-out tweening and responsive squash-and-stretch animations. Movement is decoupled into independent X and Y axis sweeps with delta-time wall sliding to prevent corner sticking.
- **The Abyssal Void Trail:** Visited floor tiles collapse permanently behind you into pitch-black void particles, preventing blind back-tracking and forcing forward momentum.
- **Sonar Acoustics & Energy Budget:** Pressing `Spacebar` emits an expanding sonar wavefront (14 tiles/sec, max radius 10). Pings consume 25 units from a 100-capacity energy meter that regenerates at 12 units/sec. Level 1 features unlimited energy as an introductory tutorial.
- **Silent Creeping:** Footsteps emit acoustic noise (1 event per 0.8 tiles traversed). Holding `Shift` activates Creep mode, halving movement speed but dropping footstep noise emission to zero.
- **Tactical Decoys:**
  - *Brass Decoy Bell (`E`):* Reusable 2 times per level with a 6-second cooldown. Rings with resonant metallic harmonics (radius 12 tiles) to pull pursuers away from bottlenecks.
  - *Acoustic Pebble Raycast (`F`):* Throws a pebble up to 5 walkable tiles ahead, creating a remote acoustic distraction (2 pebbles per level).
- **Dynamic Blackout Thriller Cycle:** High-voltage electrical power cuts plunge the facility into total darkness after a 0.85-second warning flicker, lasting between 2.2 and 4.2 seconds before an acoustic power surge restores ambient visibility.
- **Anti-Clockwise Countdown Dial:** A cyber radial dial sweeps counter-clockwise on canvas with mechanical ticking audio. Hitting zero triggers facility lockdown and instant failure.
- **Keys & Locked Gate System:** Later tiers require locating brass keys hidden in corridor alcoves. Collecting a key triggers facility sirens and alerts nearby patrols.
- **Progressive Acoustic Hints:** 3-tier escalation pipeline (exit hum volume boost, perimeter arc, illuminated safe path avoiding enemies by $\ge 4$ tiles), selectable via `SettingsManager` (`OFF`, `SUBTLE`, `GUIDED`).

---

## 3. Technical Architecture

ECHO MAZE was built under the strict constraints of the **demoscene and JS13k** development philosophy:

1. **Zero External Dependencies:** Zero npm packages, zero canvas libraries (no Pixi, Phaser, or Three.js), zero bundlers, and **zero external audio files**.
2. **Pure Single-File Distribution:** The entire game engine, simulation loop, raycaster, procedural audio synthesizers, asset renderer, particle pool, and regression test suites reside inside a single, self-contained `index.html`.
3. **Deterministic Simulation:** A custom seeded Mulberry32 pseudo-random number generator ensures that identical seeds (`?seed=N`) produce bit-for-bit identical labyrinths, enemy spawn points, and patrol cycles.
4. **Clean Separation of Concerns:**
   - `CONFIG`: Central immutable configuration table containing all physics constants, speeds, acoustic radii, energy budgets, and level definitions.
   - `AudioEngine`: Standalone procedural sound generator driven by Web Audio API nodes.
   - `Game`: Core simulation and rendering orchestrator operating on delta-time loops.
   - `SettingsManager`: Local storage abstraction with safe fallback defaults for sandboxed iframes.

---

## 4. Procedural Maze Generation

Labyrinths are carved procedurally using depth-first recursive back-tracking with an explicit stack:
- Operates on odd grid dimensions ($11 \times 11$ up to $33 \times 33$).
- Carving steps by 2 tiles, guaranteeing continuous wall boundaries between corridors.
- Random neighbor selection is driven exclusively by the seeded Mulberry32 PRNG.

---

## 5. BFS & Enemy Pathfinding

Immediately following maze generation, a Breadth-First Search (BFS) traverses the entire floor graph from the player start position `(1, 1)`:
- **Full Connectivity Validation:** Verifies that reachable floor cells strictly equal total floor cells (`reachableFloorCount === totalFloorCount`). Any unreachable pocket triggers an immediate deterministic re-seed.
- **Exit Distance Guarantee:** The exit portal is placed on a reachable candidate cell located at $\ge 60\%$ of the maximum BFS graph depth from start.
- **Dynamic Corridor Navigation:** Enemies compute shortest paths across corridors using graph BFS. BFS recalculations are throttled to target transitions, eliminating per-frame graph traversals.

---

## 6. Acoustic Event System

Acoustic propagation is completely decoupled from rendering and player state through a centralized noise event bus:
- Each acoustic emission pushes an event object: `{ id, type, cellX, cellY, time, radius }`.
- Events expire after 0.1 seconds, preventing lingering ghost detections.
- Enemies evaluate sound using **graph BFS corridor distance**, never straight-line Euclidean distance through solid walls.
- Sound prioritization: Enemies in `INVESTIGATE` mode replace their current investigation target only if a new acoustic event is closer by remaining BFS corridor distance.

---

## 7. Monster Finite-State Machines

Pursuers are autonomous agents governed by explicit finite-state machines (FSM):
- `ASLEEP`: Dormant units remain inactive until an expanding sonar ping crosses their hearing threshold.
- `WANDER`: Patrols select random reachable floor cells, pausing 1.5 seconds at waypoints before continuing patrol routes.
- `INVESTIGATE`: Upon hearing an acoustic event within their hearing radius, units elevate their speed (from 3.4 to 3.8 tiles/sec), play an alert growl, and pathfind to the sound's recorded static cell.
- `SEARCH`: Upon reaching the acoustic origin without capturing the player, units execute a 3-second search pattern (triggering counter-sonar red pulses in later levels) before returning to `WANDER`.
- `CAPTURE`: Triggered when player and monster centers are within 0.45 tiles, freezing simulation and executing camera shake.
- **Multi-Monster State Isolation:** Each enemy instance maintains isolated path buffers, state timers, and target objects. One blocked enemy never halts or mutates the behavior of another.

---

## 8. Procedural Web Audio Implementation

There are zero external audio files. All sound effects and continuous ambient cues are synthesized live from the Web Audio API:
- **Sonar Ping:** Dual-frequency sine sweep (480 Hz $\to$ 960 Hz) followed by a lowpass-filtered secondary echo reflection (440 Hz $\to$ 720 Hz).
- **Footstep Hop:** 260 Hz $\to$ 75 Hz triangle wave transient.
- **Clock Tick:** High-frequency metallic transient pitch-shifting upward during the final 8 seconds.
- **Monster Growl:** Modulated sawtooth wave with low-frequency oscillator (LFO) pitch variation.
- **Decoy Bell:** Synthesized metallic overtones with decaying resonance harmonics.
- **Spatial Exit Hum:** Continuous oscillator that dynamically adjusts volume and stereo panning (`clamp(dx / 6, -1, 1)`) based on the player's Euclidean distance and angle to the exit.
- **Biometric Heartbeat:** Double-thump sub-bass pulse (60 Hz / 80 Hz) that accelerates in tempo and volume as pursuers close within 12 to 2 path tiles.
- **Voice Management:** Simultaneous one-shot voices are strictly capped at 8 to eliminate audio distortion and CPU spikes.

---

## 9. Campaign Progression (15 Levels)

Progress is strictly sequential ($1 \to 15$). Getting captured or running out of time resets campaign progress back to Level 1, enforcing Roguelike Ironman stakes.

| Level | Name | Grid | Cops | Key Thriller & Tactical Features |
| :---: | :--- | :---: | :---: | :--- |
| **L1** | Rookie Patrol | 11×11 | 1 Cop | Baseline patrol, infinite energy tutorial, 35s timer |
| **L2** | Dormant Ear | 13×13 | 1 Cop | Sleeper cop awakens on sonar pings |
| **L3** | Wandering Shadow | 15×15 | 1 Cop | Hound cop hears uncrept footsteps (Radius 2.5 tiles) |
| **L4** | Resonant Decoy | 17×17 | 1 Cop | Brass decoy bell introduced (`E`) |
| **L5** | Twin Enforcement | 19×19 | 2 Cops | 2 Cops in opposite corners with staggered sirens |
| **L6** | Blackout Surge | 21×21 | 2 Cops | 2 Cops + rapid electrical power blackouts |
| **L7** | Crossfire Sector | 21×21 | 2 Cops | 2 Fast Interceptors + Decoy Bell |
| **L8** | Triple Threat | 23×23 | 3 Cops | 3 Cops in 3 corners with rotating lightbars |
| **L9** | Radar Pulse Ambush | 23×23 | 3 Cops | Scanner Drone sweeps counter-sonar pulses |
| **L10** | Gauntlet Run | 25×25 | 3 Cops | 3 High-speed Vipers (Speed 3.8!) |
| **L11** | Quadrant Lockdown | 27×27 | 4 Cops | 4 Cops seal ALL four corners of the perimeter |
| **L12** | Acoustic Reaper | 27×27 | 4 Cops | Extreme acoustic hearing (Radius 3.8 tiles!) |
| **L13** | Eclipse Protocol | 29×29 | 4 Cops | 4 Cops + 2 Pulse Scanners + deep blackout |
| **L14** | Apex Swarm | 31×31 | 5 Cops | 5 Cops swarming mega-labyrinth (4 corners + center stalker) |
| **L15** | Grand Escape | 33×33 | 5 Cops | 5 Apex Hunters (Speed 3.9) + multi-pulse radar! |

---

## 10. Accessibility & Performance Considerations

- **Reduced Motion Support:** Respects OS-level `prefers-reduced-motion` settings. Caches query results and listens for `MediaQueryList` change events, disabling camera shake and rapid screen flashes.
- **Colorblind-Safe Palette:** Uses distinct luminance levels and shapes for non-color identification (pulsing threat rings, distinctive monster eye shapes, and textual HUD warnings). Contrast exceeds 14:1 against the dark background (well above WCAG AA 4.5:1).
- **Object Pooling:** Dynamic particles (void trail, wall sparks) operate within a pre-allocated recycling pool capped at 150 particles, preventing garbage collection stutter.
- **Frame Rate Optimization:** Rolling 120-frame telemetry demonstrates average frame processing times under 1.8 ms on modern hardware (well below the 16.6 ms 60 FPS budget).

---

## 11. Testing & Verification

### Automated Headless Continuous Integration (`node:test`)
A dedicated headless test suite executes the core mathematical simulation in GitHub Actions CI on every commit (`tests/invariants.test.mjs`):
```bash
node --test tests/invariants.test.mjs
```
```text
✔ 1. PRNG determinism & reproducibility (Mulberry32) (2.1ms)
✔ 2. BFS maze full connectivity invariant across 200 procedural seeds (11.2ms)
✔ 3. Exit distance invariant (exit >= 60% of max BFS graph depth) (10.6ms)
✔ 4. Energy budget & single-charge ping invariant (0.1ms)
✔ 5. Monster navigation never enters wall cells across 5,000 simulated frames (6.2ms)
✔ 6. Acoustic event propagation and hearing radius discrimination (0.1ms)
✔ 7. Continuous pathing to tactical decoy bell without teleportation (0.2ms)
✔ 8. Line-of-sight raycasting correctness (0.1ms)
✔ 9. Campaign progression monotonicity (15 levels, 1 to 5 cops, expanding grids) (0.1ms)
✔ 10. Multi-monster state isolation (zero shared mutable state) (0.1ms)

ℹ tests 10 | pass 10 | fail 0 | cancelled 0 | skipped 0 | duration_ms 124.6
```

### In-Browser Runtime Self-Tests (`runSelfTests()`)
Upon canvas initialization (or when loading the game with `?debug=1`), `index.html` runs 19 automated self-test suites directly in the browser environment, validating 52 mathematical and state invariants across 200 generated seeds.

---

## 12. ODS Portal Co-Design & Export Documentation

ECHO MAZE was developed through an authentic human-agent co-design loop inside **ODS Portal**:
- **Co-Design Role:** The human served as Lead Designer, Systems Architect, and Playtester, steering the local agent away from cognitive traps (such as screen boredom) and establishing strict mathematical contracts. The local agent implemented modular game components and Web Audio synthesizers.
- **Conversation Export:** The accompanying file [`ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json`](ods-portal-a77d18ae-9291-48d6-a283-8524bd49fbf3.json) is a structured conversation export conforming to the ODS Portal schema version 1. It records the complete 55 prompt-reply turns and is fully re-importable in any ODS Portal instance via `parseConversationImport()`. It does not contain ephemeral workspace preview tokens or raw daemon task execution receipts.

---

## 13. Controls

| Action | Desktop Keyboard | Mobile / Touchscreen |
| :--- | :--- | :--- |
| **Move 1 Block** | `W`, `A`, `S`, `D` or `Arrow Keys` | On-Screen Virtual D-Pad or Canvas Swipe |
| **Sonar Ping** | `Spacebar` | `📡 PING` Touch Button or Tap Canvas |
| **Decoy Bell** | `E` | `🔔 BELL` Touch Button (2 uses, 6s cooldown) |
| **Pebble Decoy** | `F` | `🪨 PEBBLE` Touch Button (2 uses, 5-tile raycast) |
| **Creep (Silent)** | `Shift` | Built-in assist mode |
| **Pause** | `P` or `Escape` | `⏸ PAUSE` Header Control |
| **Restart Level** | `R` | Menu button |

---

## 14. License & Repository Links

- **Live Game:** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)
- **Repository:** [https://github.com/vaibhavsrv/echo-maze](https://github.com/vaibhavsrv/echo-maze)
- **License:** Apache License 2.0 (see [LICENSE](LICENSE))
