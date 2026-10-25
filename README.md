# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical acoustic-stealth maze thriller built for the **ODS Weekly Dev Jam**.

🎮 **Play Live Online:** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)

---

## ⚡ 15 Unique Levels & Thriller Mechanics
- **1-Block Tactical Movement**: Discrete grid hopping with ease-out tweening and responsive squash-and-stretch physics.
- **The Pitch-Black Void Trail**: Everywhere you move, the path behind you collapses into an abyssal pitch-black void.
- **Scaling Cop Patrols (1 to 5 Cops)**: Cops multiply as you advance! Corner drones spawn with 360° sweeping volumetric red & blue siren searchlights, pulse scanners, and emergency lightbars.
- **Dynamic Suspense Blackout Cycle**: High-voltage electrical power cuts plunge the labyrinth into total darkness with warning flickers and acoustic surges. Blackout duration and frequency escalate in higher levels.
- **Anti-Clockwise Countdown Dial**: A cyber radial dial sweeps counter-clockwise with mechanical audio ticking and emergency lockdown upon hitting 0.
- **Acoustic Gate Clues**: Dynamic golden compass needle rotating on the player's perimeter ring pointing directly to the gate, accompanied by drifting golden echo wisps.
- **Sequential Campaign (1 &rarr; 15)**: Levels must be cleared in strict linear sequence ($1 \to 2 \to 3 \to 4 \dots \to 15$).
- **Permadeath Failure Reset**: High stakes! If you are caught by a cop or run out of time at *any* stage (e.g., Level 4), the run fails and you must restart from **Level 1**!
- **Mission Level Selector (Practice)**: Jump directly to any of the 15 levels from the Title Screen to inspect or practice specific sectors.
- **Cross-Device Ready**: Full support for desktop keyboard (WASD / Arrows), touch D-Pad, and swipe gestures on mobile devices (iOS, Android, Tablets).

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
| **Decoy Bell** | `E` | `🔔 BELL` Button |
| **Creep (Silent)** | `Shift` | Built-in assist mode |
| **Pause** | `Escape` | `⏸ PAUSE` in Header |
| **Restart Level** | `R` | Menu button |

---

## 🧪 Automated Verification
Includes 19 automated self-tests verifying:
- BFS maze connectivity across 200 random seeds
- Optimal exit distance thresholds ($\ge 60\%$ max BFS depth)
- PRNG deterministic reproducibility
- Monster FSM hearing radii and collision avoidance
- Audio node lifecycle and cleanup
- Bell interaction single-use contracts

Run self-tests in browser console or telemetry HUD by appending `?debug=1` to the URL.
