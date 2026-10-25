# ECHO MAZE — Complete 43-Part Prompt Evolution Log
> **ODS Weekly Dev Jam Submission** • *Human-in-the-Loop Iterative Development Log*

This document archives the complete 43-part iterative prompt journey that engineered **ECHO MAZE** from architectural inception to its final 15-level tactical acoustic thriller.

---

## 🏗️ Part I: Core Architecture & Gameplay Foundation (Prompts 1 – 10)

### 🔹 Prompt 01 — Project Vision & Technology Constraints
```text
I want to build a browser game called ECHO MAZE for the ODS Weekly Dev Jam. The tagline is "Light costs noise." The concept is a tactical acoustic-stealth thriller where the player navigates dark mazes, uses sonar to understand the environment, manages limited energy, and avoids sound-sensitive enemies. Build this in a single index.html file using vanilla HTML5, CSS, Canvas and Web Audio API. Zero external engines or audio files.
```

### 🔹 Prompt 02 — Central Configuration & Levels Table
```text
Put all gameplay tuning numbers in a single CONFIG object at the top of the JavaScript: tile sizes, movement speeds, sonar ranges, wall reveal duration, energy costs, monster speeds, hearing radii, capture distances, and audio limits. Use a separate LEVELS table for level-specific properties. No scattered magic numbers.
```

### 🔹 Prompt 03 — Seeded PRNG & Deterministic Maze Generation
```text
Implement deterministic procedural maze generation using a seeded random algorithm (Mulberry32 PRNG) so identical seeds always produce identical mazes. Support ?seed=N in the URL for repeatable runs.
```

### 🔹 Prompt 04 — BFS Connectivity & Exit Distance Validation
```text
Use Breadth-First Search (BFS) to validate that every floor cell in the maze is reachable from the start (1, 1). Ensure the exit portal is placed on a reachable cell at least 60% of the maximum BFS distance from the start.
```

### 🔹 Prompt 05 — Delta-Time Movement & Wall Collision
```text
Implement player movement driven by delta time with wall sliding and reliable bounding-box collision detection so movement speed is silky smooth and frame-rate independent across 60Hz, 120Hz, and mobile displays.
```

### 🔹 Prompt 06 — Line-of-Sight Sonar Rings & Wall Reveal
```text
When the player presses Space, emit expanding sonar rings from their center. Walls within the pulse radius should be revealed only if there is an unblocked line of sight from the player. Revealed walls must fade smoothly over 3.5 seconds without permanently unveiling the maze.
```

### 🔹 Prompt 07 — Energy Meter & Depletion State
```text
Add an energy meter (max 100). Each accepted sonar ping costs 25 energy, regenerating at 12 energy/sec. Energy must never fall below zero. Render an energy arc ring around the player with a clear depleted indicator when energy is insufficient. Level 1 must have unlimited energy.
```

### 🔹 Prompt 08 — Central Noise Event System
```text
Implement a central noise-event bus where each event contains { id, type, x, y, time, radius }. Accepted pings emit exactly one event with radius 10. Process each event once and expire it after 0.1 seconds. Visual effects and UI must never generate accidental noise events.
```

### 🔹 Prompt 09 — Distance-Based Footstep Audio & Stealth Creep
```text
Footstep noise events must be generated strictly from actual movement distance (one event per 0.8 tiles traversed), never per animation frame. When the player holds Shift (Creep mode) or is stationary, zero footsteps must be emitted.
```

### 🔹 Prompt 10 — Game Lifecycle: Pause, Victory & Quick Restart
```text
Wire game lifecycle states: Escape toggles pause/resume (freezing delta-time and timers), R restarts the run, and stepping onto the exit cell triggers the victory state with a celebratory visual flash.
```

---

## 🧠 Part II: Listener Monster AI & Sensory Perception (Prompts 11 – 20)

### 🔹 Prompt 11 — Monster Explicit Finite State Machine
```text
Create the Listener monster AI driven by an explicit finite-state machine: WANDER, INVESTIGATE, SEARCH, and CAPTURE. The monster must move along grid corridors using delta time and search for 3 seconds before resuming wandering.
```

### 🔹 Prompt 12 — Path-Distance Hearing (No Euclidean Cheats)
```text
The monster must hear a noise event only when the BFS path distance through the maze is less than or equal to its configured hearing radius. Straight-line euclidean distance through walls must never be used.
```

### 🔹 Prompt 13 — Sound Investigation vs Live Player Tracking
```text
When a monster hears a sound, it must record the exact grid cell of the noise event and investigate that static location. It must NEVER track the player's live position or cheat through the dark.
```

### 🔹 Prompt 14 — Closer-Sound Target Replacement & BFS Throttling
```text
If the monster is already investigating a sound and hears a new valid event, replace its target only if the new sound is closer by remaining BFS path distance. Recalculate BFS paths only when the target changes, never on every frame.
```

### 🔹 Prompt 15 — Unreachable Sound Handling & Wall Clipping Defense
```text
If an acoustic event occurs in an unreachable cell, route the monster to the closest reachable cell and search there. The monster must never teleport, clip into walls, or pass through impassable tiles.
```

### 🔹 Prompt 16 — Capture Sequence & Mutual Exclusion
```text
Capture occurs when player and monster centers are within 0.45 tiles. On capture, freeze simulation, trigger screen shake and red flash, display HEARD. with a Retry button. Ensure Capture and Victory are mutually exclusive—the first event wins.
```

### 🔹 Prompt 17 — Campaign Architecture: Level Transitions & Run IDs
```text
Configure campaign progression. Level transitions must cleanly flush active sonar rings, wall reveals, noise events, monster paths, targets, and audio nodes. Use a unique run ID so lingering callbacks from old levels are ignored.
```

### 🔹 Prompt 18 — Level 1: The Rookie Patrol
```text
Configure Level 1: 9x9 maze, zero monsters, unlimited sonar energy. Serves as the introductory stealth tutorial.
```

### 🔹 Prompt 19 — Level 2: The Dormant Ear
```text
Configure Level 2: 13x13 maze with one sleeping Listener in the center. The sleeper stays dormant during wandering and wakes up only when an expanding sonar ping reaches its hearing threshold.
```

### 🔹 Prompt 20 — Level 3: Wandering Shadow
```text
Configure Level 3: 17x17 maze with an active wandering Listener, limited energy, and audible footsteps. Players must utilize the Shift creep key to navigate undetected.
```

---

## 🔔 Part III: Distractions, Multi-Enemy & Synthesized Audio (Prompts 21 – 30)

### 🔹 Prompt 21 — Level 4: The Resonant Decoy Bell
```text
Configure Level 4: 19x19 maze featuring a brass decoy bell placed on a reachable cell away from the starting route. The bell remains invisible until revealed by a sonar ping.
```

### 🔹 Prompt 22 — Bell Interaction Rules & Acoustic Decoy
```text
When within 0.6 tiles of the revealed bell, pressing E rings it. It emits a loud noise event (radius 12) at the bell's coordinates to lure the monster away. The bell is single-use per run and works via normal monster hearing rules.
```

### 🔹 Prompt 23 — Level 5: Twin Enforcement (Dual Independent Listeners)
```text
Configure Level 5: 23x23 maze featuring two simultaneous Listeners (Listener A and Listener B) with completely independent state machines, targets, search timers, and paths. Neither may share mutable state.
```

### 🔹 Prompt 24 — Asymmetric Hearing Profiles & Monster Audio Growls
```text
Give Listener A a footstep radius of 2.0 tiles, ping radius of 10, and low growl pitch. Give Listener B a footstep radius of 3.5 tiles, ping radius of 7, and higher pitch. If one is blocked, the other must continue hunting normally.
```

### 🔹 Prompt 25 — Level 6 Finale: Eclipse Protocol & Enemy Radar Pulse
```text
Configure the Finale: 25x25 maze with an Apex Listener that emits an active red sonar pulse (radius 7, 0.8s) whenever it arrives at an investigation target and enters SEARCH.
```

### 🔹 Prompt 26 — Counter-Radar Detection & Line-of-Sight Check
```text
When the enemy red pulse fires, reveal nearby walls in red. If the player is within the 7-tile pulse radius AND has line of sight to the monster, generate a noise event at the player's location so the monster immediately hunts them.
```

### 🔹 Prompt 27 — Procedural Web Audio Engine (Zero External Audio)
```text
Build a procedural Web Audio API sound synthesizer that initializes on first user click or keypress. If AudioContext fails or is blocked, the game must fail gracefully and run silently without throwing errors.
```

### 🔹 Prompt 28 — Procedural Ping, Footstep & Monster Sounds
```text
Synthesize: 1) Sonar Ping with dual-frequency sine sweep and exponential echo decay; 2) Footsteps using filtered white noise bursts; 3) Monster growls using low-frequency sawtooth waves modulated by biquad bandpass filters.
```

### 🔹 Prompt 29 — Exit Beacon Hum with 3D Stereo Panning
```text
Create a continuous resonant portal hum that gets louder as the player nears the exit. Apply stereo panning using clamp(dx / 6 tiles, -1, 1) relative to player position so the player can navigate by acoustic stereo cues alone.
```

### 🔹 Prompt 30 — Dynamic Proximity Heartbeat System
```text
Synthesize a biological double-thump heartbeat (sub-bass sine wave). Dynamically scale tempo and volume based on the closest monster's BFS path distance (accelerating from 12 tiles down to 2 tiles), relaxing smoothly when danger recedes.
```

---

## 📱 Part IV: Polish, Verification & Mobile Hardening (Prompts 31 – 38)

### 🔹 Prompt 31 — Audio Resource Lifecycle & Max Voice Capping
```text
Cap simultaneous one-shot audio voices to 8. On pause, fade continuous audio nodes smoothly. On restart or level transition, immediately stop and disconnect all continuous hums and heartbeat oscillators to eliminate memory leaks.
```

### 🔹 Prompt 32 — Title Screen, Vignette & Reduced Motion
```text
Build a sci-fi cyber title screen with ECHO MAZE typography, "Light costs noise" tagline, start button, volume slider, and controls overview. Add a subtle canvas vignette. Respect CSS prefers-reduced-motion to disable screen shake for sensitive players.
```

### 🔹 Prompt 33 — In-Game HUD, Energy Arc & Contextual Hints
```text
Design the HUD: level badge, objective line, sound toggle, pause button, and dynamic energy arc around the player. Display contextual hints during the first 5 seconds explaining movement, Space for sonar, and danger attraction.
```

### 🔹 Prompt 34 — Victory & Capture Cinematic Screens
```text
On capture: red flash, screen shake, HEARD. title, and restart button. On victory: green resonance flash, level stats card displaying real elapsed time and total sonar pings used, with a button to advance to the next level.
```

### 🔹 Prompt 35 — Assist Mode & Safe LocalStorage Persistence
```text
Implement an Assist Mode toggle (wider hearing margins, reduced monster speed). Persist volume, mute, and assist settings in localStorage wrapped in try/catch blocks with fallback defaults for sandboxed iframes.
```

### 🔹 Prompt 36 — Developer Telemetry Overlay (?debug=1)
```text
When ?debug=1 is in the URL, render a live developer overlay displaying PRNG seed, FPS, frame time, player grid coordinates, monster FSM states, active paths, and noise event logs.
```

### 🔹 Prompt 37 — Automated Self-Test Regression Suite (runSelfTests)
```text
Add runSelfTests() testing 19 engine invariants: 200 seeded mazes connected; exit >= 60% distance; deterministic PRNG; energy bounds; monster never enters walls across 500s; BFS hearing accuracy; bell single-use; and red pulse line-of-sight. Output PASS/FAIL to console.
```

### 🔹 Prompt 38 — Mobile Touch Controls: Virtual D-Pad & Gestures
```text
Add responsive on-screen touch controls for mobile and tablet players: virtual tactile D-Pad (Up/Down/Left/Right), dedicated PING and BELL touch buttons, and canvas swipe gesture support with preventDefault to block accidental page scrolling.
```

---

## 🔥 Part V: The Thriller Overhaul & 15-Level Campaign (Prompts 39 – 43)

### 🔹 Prompt 39 — Eliminating Visual Boredom
```text
i think the screen should not be totally black, it shows boredom
```
**Applied Engineering:**
- Upgraded the static black screen to a glowing cybernetic grid.
- Designed luminous cyan pathway borders (`rgba(0, 243, 255, 0.2)`), linear gradient walkable floors, and tactical glowing floor coordinate crosshairs.

### 🔹 Prompt 40 — Corner Cop Patrol, Void Trail & Countdown Dial
```text
now the path is totally visible, i think we should add a monster/cop starts from any top corner(left,right, left bottom,right bottom) and we have some time ticking anti-clockwise and we need to go the gate with in the span of time, and wherever i started moving the path will be black, there should be a small clue for the gate,
the movement should be of 1 block, and add some good and crazy animations
```
**Applied Engineering:**
- **1-Block Tactical Movement**: Discrete hop step physics with squash-and-stretch bounce animations.
- **The Abyssal Void Trail**: Every cell stepped on collapses permanently into pitch black (`#000000`) with void-purple crumble particles (`rgba(147, 51, 234)`).
- **Corner Police Patrol**: Sci-fi police drones spawn in outer corners with 360° sweeping volumetric siren searchlights, rotating emergency lightbars, and procedural siren audio.
- **Anti-Clockwise Dial**: Canvas radial countdown clock ticking down mechanically to zero lockdown.
- **Resonant Compass**: Dynamic golden compass needle rotating on the player's perimeter ring pointing directly to the exit gate.

### 🔹 Prompt 41 — Suspense Blackout Thriller Cycle
```text
look, we are gonna a suspension we can black out the path for some seconds and then again visible paths, then again after few seconds(random) black out

this will increase the thriller
```
**Applied Engineering:**
- **Dynamic Blackout Thriller Cycle**: High-voltage electrical power cuts periodically plunge the entire maze into pitch blackness.
- **Electric Warning Flicker**: Rapid high-frequency flickering phase (Math.sin(now * 38)) before blackout.
- **Acoustic Power Cut/Surge**: Procedural pitch-drop audio (`playPowerCut`) and pitch-rise restoration surge (`playPowerSurge`) with screen shake.

### 🔹 Prompt 42 — 15 Unique Levels & Swarming Cop Fleet (1 to 5 Cops)
```text
as we will moving to the higher level the cops should also increase in numbers, also add some more difficult and unique levels last 15 levels and as the level increases the thriller also increase and the dificulty too(add some unique levels)
```
**Applied Engineering:**
- **Expanded to 15 Distinct Handcrafted Levels**: Dimensions scale from 11×11 up to 33×33.
- **Scaling Cop Swarm (1 to 5 Cops)**: Cops multiply as levels advance, spawning in all 4 corners (Top-Left, Top-Right, Bottom-Left, Bottom-Right) and interior corridors.
- **Thriller Scaling**: Blackout darkness intervals lengthen (up to 7.7s) and visible windows shrink in higher levels.
- **Specialized Units**: Sleeper Drones, Hound Cops (footstep hearing), Decoy Bells, Pulse Scanners, and Apex Hunters (Speed 3.9).
- **Mission Level Selector**: Added title screen dropdown allowing players and judges to test any of the 15 levels directly.

### 🔹 Prompt 43 — Sequential Ironman Roguelike Rule
```text
the level should be cross at the sequence like 1->2->3->4 if someone fails at 4 he needs to restart from 1
```
**Applied Engineering:**
- **Strict Linear Progression**: Levels must be beaten sequentially ($1 \to 2 \to 3 \to \dots \to 15$).
- **Permadeath Failure Reset**: If captured or timed out at *any* stage (e.g. Level 4), campaign progress fails and resets immediately to **Level 1** (`this.levelIndex = 0`).
- **Dynamic Failure Feedback**: Captured and Timeout screens dynamically display `"Failed at Level X — restart from Level 1!"` with `RESTART FROM LEVEL 1 (R)` buttons.

---

## 🏆 Summary Checklist for Judges
- ✅ **100% Vanilla Code**: Single `index.html` file, 0 external libraries, 0 external audio files.
- ✅ **Procedural Synthesis**: All audio generated via Web Audio API.
- ✅ **Cross-Platform**: Tested on PC, Mac, iPhone, iPad, and Android with full touch D-Pad support.
- ✅ **Deterministic & Tested**: 19 automated self-tests passing with 0 regressions.
- ✅ **Live Playable Link**: [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)
- ✅ **GitHub Repository**: [https://github.com/vaibhavsrv/echo-maze](https://github.com/vaibhavsrv/echo-maze)
