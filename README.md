# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical acoustic-stealth maze thriller built for the **ODS Weekly Dev Jam**.

- 🎮 **Play Live Online (Main Game):** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)  
- 🌲 **Play Echo Grove Prototype (Pass 4 Variant):** [https://vaibhavsrv.github.io/echo-maze/grove.html](https://vaibhavsrv.github.io/echo-maze/grove.html)
- 📜 **Prompt Evolution Log (49 Stages):** [PROMPTS.md](PROMPTS.md)

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

## 🧪 Automated Verification
Includes **35 automated self-tests** in `index.html` and **16 automated self-tests** in `grove.html` verifying:
- BFS maze connectivity across 200 random seeds
- Optimal exit distance thresholds ($\ge 60\%$ max BFS depth)
- PRNG deterministic reproducibility (Mulberry32)
- Monster FSM hearing radii and BFS corridor acoustic bounds
- Sonar line-of-sight raycasting and shell expansion immunity
- 150-particle pooled ceiling and telemetry budgets (< 8.0 ms average frame time)
- High score and deterministic stealth rating calculations (S/A/B/C)
- Closed secret wall reachability and audio lifecycle safety

Run self-tests in the browser console or telemetry HUD by appending `?debug=1` to the URL.
