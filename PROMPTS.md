# ECHO MAZE: Technical Specification and Prompt Evolution Log

**Project:** ECHO MAZE  
**Tagline:** "Light costs noise."  
**Event:** ODS Weekly Dev Jam Submission  
**License:** Apache-2.0  
**Repository:** https://github.com/vaibhavsrv/echo-maze  
**Live Application:** https://vaibhavsrv.github.io/echo-maze/  

---

## 1. Executive Summary

This log documents the 43-stage iterative engineering process used to construct ECHO MAZE within the ODS Portal environment. The application is implemented entirely within a single `index.html` file utilizing vanilla HTML5 Canvas, ECMAScript 2022, and the Web Audio API. No external game engines, build systems, or pre-recorded audio assets were used.

Development progressed linearly across five structured phases:
1. Core simulation loop, deterministic PRNG maze generation, and delta-time movement.
2. Enemy finite state machine (FSM) with Breadth-First Search (BFS) path-distance acoustics.
3. Decoy mechanics, multi-agent coordination, and real-time procedural audio synthesis.
4. User interface HUD, telemetry overlay (?debug=1), touch controls, and automated unit testing.
5. Tactical thriller mechanics including periodic power-cut blackouts, dynamic police patrols with volumetric sirens, 15 handcrafted progressive stages, and roguelike failure reset.

---

## 2. Phase I: Core Architecture and Baseline Mechanics (Prompts 01–10)

### Prompt 01: Project Scope and Constraints
```text
I want to build a browser game called ECHO MAZE for the ODS Weekly Dev Jam. The tagline is "Light costs noise." The concept is a tactical acoustic-stealth thriller where the player navigates dark mazes, uses sonar to understand the environment, manages limited energy, and avoids sound-sensitive enemies. Build this in a single index.html file using vanilla HTML5, CSS, Canvas and Web Audio API. Zero external engines or audio files.
```

### Prompt 02: Central Configuration and Level Definitions
```text
Put all gameplay tuning numbers in a single CONFIG object at the top of the JavaScript: tile sizes, movement speeds, sonar ranges, wall reveal duration, energy costs, monster speeds, hearing radii, capture distances, and audio limits. Use a separate LEVELS table for level-specific properties. No scattered magic numbers.
```

### Prompt 03: Seeded PRNG and Deterministic Maze Generation
```text
Implement deterministic procedural maze generation using a seeded random algorithm (Mulberry32 PRNG) so identical seeds always produce identical mazes. Support ?seed=N in the URL for repeatable runs.
```

### Prompt 04: BFS Reachability and Exit Distance Constraints
```text
Use Breadth-First Search (BFS) to validate that every floor cell in the maze is reachable from the start (1, 1). Ensure the exit portal is placed on a reachable cell at least 60% of the maximum BFS distance from the start.
```

### Prompt 05: Delta-Time Movement and Collision Sliding
```text
Implement player movement driven by delta time with wall sliding and reliable bounding-box collision detection so movement speed is silky smooth and frame-rate independent across 60Hz, 120Hz, and mobile displays.
```

### Prompt 06: Line-of-Sight Raycasting and Sonar Reveal
```text
When the player presses Space, emit expanding sonar rings from their center. Walls within the pulse radius should be revealed only if there is an unblocked line of sight from the player. Revealed walls must fade smoothly over 3.5 seconds without permanently unveiling the maze.
```

### Prompt 07: Energy Consumption and Depletion Rules
```text
Add an energy meter (max 100). Each accepted sonar ping costs 25 energy, regenerating at 12 energy/sec. Energy must never fall below zero. Render an energy arc ring around the player with a clear depleted indicator when energy is insufficient. Level 1 must have unlimited energy.
```

### Prompt 08: Noise Event Bus Architecture
```text
Implement a central noise-event bus where each event contains { id, type, x, y, time, radius }. Accepted pings emit exactly one event with radius 10. Process each event once and expire it after 0.1 seconds. Visual effects and UI must never generate accidental noise events.
```

### Prompt 09: Distance-Accumulated Footstep Acoustic Emission
```text
Footstep noise events must be generated strictly from actual movement distance (one event per 0.8 tiles traversed), never per animation frame. When the player holds Shift (Creep mode) or is stationary, zero footsteps must be emitted.
```

### Prompt 10: State Lifecycle: Pause, Victory, and Restart
```text
Wire game lifecycle states: Escape toggles pause/resume (freezing delta-time and timers), R restarts the run, and stepping onto the exit cell triggers the victory state with a celebratory visual flash.
```

---

## 3. Phase II: Enemy State Machine and Sensory Perception (Prompts 11–20)

### Prompt 11: Explicit Enemy Finite State Machine
```text
Create the Listener monster AI driven by an explicit finite-state machine: WANDER, INVESTIGATE, SEARCH, and CAPTURE. The monster must move along grid corridors using delta time and search for 3 seconds before resuming wandering.
```

### Prompt 12: Topological Path-Distance Hearing
```text
The monster must hear a noise event only when the BFS path distance through the maze is less than or equal to its configured hearing radius. Straight-line euclidean distance through walls must never be used.
```

### Prompt 13: Static Coordinate Investigation Rules
```text
When a monster hears a sound, it must record the exact grid cell of the noise event and investigate that static location. It must NEVER track the player's live position or cheat through the dark.
```

### Prompt 14: Dynamic Target Preemption and BFS Throttling
```text
If the monster is already investigating a sound and hears a new valid event, replace its target only if the new sound is closer by remaining BFS path distance. Recalculate BFS paths only when the target changes, never on every frame.
```

### Prompt 15: Unreachable Target Fallback and Wall Confinement
```text
If an acoustic event occurs in an unreachable cell, route the monster to the closest reachable cell and search there. The monster must never teleport, clip into walls, or pass through impassable tiles.
```

### Prompt 16: Capture Terminal State and Mutual Exclusion
```text
Capture occurs when player and monster centers are within 0.45 tiles. On capture, freeze simulation, trigger screen shake and red flash, display HEARD. with a Retry button. Ensure Capture and Victory are mutually exclusive—the first event wins.
```

### Prompt 17: Campaign Transitions and Asynchronous Run IDs
```text
Configure campaign progression. Level transitions must cleanly flush active sonar rings, wall reveals, noise events, monster paths, targets, and audio nodes. Use a unique run ID so lingering callbacks from old levels are ignored.
```

### Prompt 18: Stage Configuration: Level 1 (Tutorial Corridor)
```text
Configure Level 1: 9x9 maze, zero monsters, unlimited sonar energy. Serves as the introductory stealth tutorial.
```

### Prompt 19: Stage Configuration: Level 2 (Dormant Unit)
```text
Configure Level 2: 13x13 maze with one sleeping Listener in the center. The sleeper stays dormant during wandering and wakes up only when an expanding sonar ping reaches its hearing threshold.
```

### Prompt 20: Stage Configuration: Level 3 (Active Patrol)
```text
Configure Level 3: 17x17 maze with an active wandering Listener, limited energy, and audible footsteps. Players must utilize the Shift creep key to navigate undetected.
```

---

## 4. Phase III: Decoys, Multi-Agent Systems, and Audio Synthesis (Prompts 21–30)

### Prompt 21: Decoy Bell Placement
```text
Configure Level 4: 19x19 maze featuring a brass decoy bell placed on a reachable cell away from the starting route. The bell remains invisible until revealed by a sonar ping.
```

### Prompt 22: Decoy Interaction and Acoustic Rules
```text
When within 0.6 tiles of the revealed bell, pressing E rings it. It emits a loud noise event (radius 12) at the bell's coordinates to lure the monster away. The bell is single-use per run and works via normal monster hearing rules.
```

### Prompt 23: Multi-Agent Concurrency (Twin Listeners)
```text
Configure Level 5: 23x23 maze featuring two simultaneous Listeners (Listener A and Listener B) with completely independent state machines, targets, search timers, and paths. Neither may share mutable state.
```

### Prompt 24: Asymmetric Sensory Profiles and Voice Pitch
```text
Give Listener A a footstep radius of 2.0 tiles, ping radius of 10, and low growl pitch. Give Listener B a footstep radius of 3.5 tiles, ping radius of 7, and higher pitch. If one is blocked, the other must continue hunting normally.
```

### Prompt 25: Active Radar Emission by Apex Enemy
```text
Configure the Finale: 25x25 maze with an Apex Listener that emits an active red sonar pulse (radius 7, 0.8s) whenever it arrives at an investigation target and enters SEARCH.
```

### Prompt 26: Counter-Radar Detection and Line-of-Sight Trigger
```text
When the enemy red pulse fires, reveal nearby walls in red. If the player is within the 7-tile pulse radius AND has line of sight to the monster, generate a noise event at the player's location so the monster immediately hunts them.
```

### Prompt 27: Web Audio Initialization and Lifecycle Management
```text
Build a procedural Web Audio API sound synthesizer that initializes on first user click or keypress. If AudioContext fails or is blocked, the game must fail gracefully and run silently without throwing errors.
```

### Prompt 28: Procedural Voice Synthesis Specifications
```text
Synthesize: 1) Sonar Ping with dual-frequency sine sweep and exponential echo decay; 2) Footsteps using filtered white noise bursts; 3) Monster growls using low-frequency sawtooth waves modulated by biquad bandpass filters.
```

### Prompt 29: Exit Portal Hum and Spatial Stereo Panning
```text
Create a continuous resonant portal hum that gets louder as the player nears the exit. Apply stereo panning using clamp(dx / 6 tiles, -1, 1) relative to player position so the player can navigate by acoustic stereo cues alone.
```

### Prompt 30: Biometric Heartbeat with Dynamic Proximity Scaling
```text
Synthesize a biological double-thump heartbeat (sub-bass sine wave). Dynamically scale tempo and volume based on the closest monster's BFS path distance (accelerating from 12 tiles down to 2 tiles), relaxing smoothly when danger recedes.
```

---

## 5. Phase IV: User Interface, Telemetry, and Input Systems (Prompts 31–38)

### Prompt 31: Voice Allocation and Oscillator Cleanup
```text
Cap simultaneous one-shot audio voices to 8. On pause, fade continuous audio nodes smoothly. On restart or level transition, immediately stop and disconnect all continuous hums and heartbeat oscillators to eliminate memory leaks.
```

### Prompt 32: Visual Framing and Accessibility Constraints
```text
Build a sci-fi cyber title screen with ECHO MAZE typography, "Light costs noise" tagline, start button, volume slider, and controls overview. Add a subtle canvas vignette. Respect CSS prefers-reduced-motion to disable screen shake for sensitive players.
```

### Prompt 33: HUD Instruments and Transient Context Hints
```text
Design the HUD: level badge, objective line, sound toggle, pause button, and dynamic energy arc around the player. Display contextual hints during the first 5 seconds explaining movement, Space for sonar, and danger attraction.
```

### Prompt 34: Terminal Outcome Presentation
```text
On capture: red flash, screen shake, HEARD. title, and restart button. On victory: green resonance flash, level stats card displaying real elapsed time and total sonar pings used, with a button to advance to the next level.
```

### Prompt 35: Configuration Persistence and Sandboxed Fallbacks
```text
Implement an Assist Mode toggle (wider hearing margins, reduced monster speed). Persist volume, mute, and assist settings in localStorage wrapped in try/catch blocks with fallback defaults for sandboxed iframes.
```

### Prompt 36: Diagnostic Telemetry Engine (?debug=1)
```text
When ?debug=1 is in the URL, render a live developer overlay displaying PRNG seed, FPS, frame time, player grid coordinates, monster FSM states, active paths, and noise event logs.
```

### Prompt 37: Automated Engine Invariant Regression Suite
```text
Add runSelfTests() testing 19 engine invariants: 200 seeded mazes connected; exit >= 60% distance; deterministic PRNG; energy bounds; monster never enters walls across 500s; BFS hearing accuracy; bell single-use; and red pulse line-of-sight. Output PASS/FAIL to console.
```

### Prompt 38: Touch Interface: Virtual D-Pad and Gestures
```text
Add responsive on-screen touch controls for mobile and tablet players: virtual tactile D-Pad (Up/Down/Left/Right), dedicated PING and BELL touch buttons, and canvas swipe gesture support with preventDefault to block accidental page scrolling.
```

---

## 6. Phase V: Tactical Thriller Overhaul and Campaign Expansion (Prompts 39–43)

### Prompt 39: Contrast and Corridor Visibility
```text
i think the screen should not be totally black, it shows boredom
```
**Technical Implementation:**
- Replaced the uniform black void with a low-opacity cybernetic coordinate grid.
- Introduced cyan corridor bounds (`rgba(0, 243, 255, 0.2)`), subtle gradient flooring, and floor tile alignment markers.

### Prompt 40: Police Drone Spawn, Void Collapse, and Countdown Dial
```text
now the path is totally visible, i think we should add a monster/cop starts from any top corner(left,right, left bottom,right bottom) and we have some time ticking anti-clockwise and we need to go the gate with in the span of time, and wherever i started moving the path will be black, there should be a small clue for the gate,
the movement should be of 1 block, and add some good and crazy animations
```
**Technical Implementation:**
- **Discrete Hop Kinematics:** Configured discrete 1-tile grid hops accompanied by vertical squash-and-stretch easing.
- **Abyssal Void Trail:** Each vacated tile collapses permanently to pitch black (`#000000`) with purple particle dissolution (`rgba(147, 51, 234)`).
- **Corner Police Drone Units:** Configured patrol drones spawning in perimeter corners, featuring 360-degree rotating volumetric searchlights, flashing dual-phase emergency lightbars, and modulated police siren audio.
- **Mechanical Radial Countdown:** Implemented an anti-clockwise dial on the HUD tracking remaining stage duration until facility lockdown.
- **Resonant Portal Compass:** Added an angular golden guide needle along the player's perimeter ring pointing toward the exit coordinate.

### Prompt 41: Periodic Blackout Electrical Cycle
```text
look, we are gonna a suspension we can black out the path for some seconds and then again visible paths, then again after few seconds(random) black out

this will increase the thriller
```
**Technical Implementation:**
- **Dynamic Electrical Cycle:** Introduced periodic facility blackouts extinguishing all ambient corridor illumination.
- **High-Frequency Warning Flicker:** Pre-blackout warning state with rapid luminous oscillations (`Math.sin(now * 38)`).
- **Acoustic Transients:** Procedural pitch-drop discharge sound on power cut and ascending capacitor surge sound on power restoration.

### Prompt 42: Scaling Multi-Agent Fleets and 15-Stage Campaign
```text
as we will moving to the higher level the cops should also increase in numbers, also add some more difficult and unique levels last 15 levels and as the level increases the thriller also increase and the dificulty too(add some unique levels)
```
**Technical Implementation:**
- **Handcrafted 15-Stage Table:** Expanded campaign progression from 11x11 to 33x33 dimensions.
- **Scaling Enemy Fleet:** Patrol count scales from 1 up to 5 concurrent units stationed across corners and interior corridors.
- **Darkness Scaling:** Blackout intervals increase up to 7.7 seconds in late-tier stages while visible illumination windows contract.
- **Unit Hierarchy:** Configured Sleeper Drones, Acoustic Hounds (footstep detection), Decoy Bells, Pulse Scanners, and Apex Hunters.
- **Stage Selector:** Integrated title screen dropdown for direct level testing by evaluators.

### Prompt 43: Roguelike Ironman Linear Progression Rule
```text
the level should be cross at the sequence like 1->2->3->4 if someone fails at 4 he needs to restart from 1
```
**Technical Implementation:**
- **Strict Linear Order:** Enforced sequential progress requirement (1 -> 2 -> 3 -> ... -> 15).
- **Permadeath Level Reset:** Capture or countdown timeout at any level immediately resets campaign progress to Stage 1 (`this.levelIndex = 0`).
- **Terminal State Messaging:** Updated fail dialogs to reflect exact failure stage and prompt immediate restart from Level 1.

### Prompt 44: Dedicated Directional WASD Controls
```text
w - upside movement, s - downward movement, a- left movement and d - right movement this should be the movement
```
**Technical Implementation:**
- **Explicit Directional Key Mapping:**
  - `W` / `KeyW`: Upside movement (`dy = -1, dx = 0`).
  - `S` / `KeyS`: Downward movement (`dy = 1, dx = 0`).
  - `A` / `KeyA`: Left movement (`dx = -1, dy = 0`).
  - `D` / `KeyD`: Right movement (`dx = 1, dy = 0`).
- **Input Buffering and Multi-Key Queue:** Added an input buffer queue and arrival-order key tracking (`activeMovementKeys`) so directional taps during hop transitions are never swallowed.
- **Corner Sliding and Preemption:** If an active directional key encounters a wall boundary, secondary held movement keys immediately take priority without halting player momentum.
- **Browser Event Optimization:** Called `preventDefault()` on all movement keys (`W`, `A`, `S`, `D`, and Arrow keys) to eliminate window scrolling artifacts and browser shortcut contention.
- **Interface Guidance:** Updated title screen guide and in-game tutorial hint box with explicit WASD directional mappings.

---

## 7. Compliance and Verification Summary

1. **Architecture:** Single-file vanilla HTML5, Canvas, and Web Audio API implementation (`index.html`).
2. **Audio Pipeline:** 100% procedurally synthesized Web Audio nodes without external sound files.
3. **Cross-Platform Support:** Compatible with desktop browsers (keyboard), mobile devices, and tablets (virtual D-Pad, touch buttons, swipe gestures).
4. **Deterministic Testing:** 19 automated engine self-tests (`runSelfTests()`) verifying maze connectivity, PRNG determinism, BFS acoustic bounds, and wall collision integrity.
5. **Licensing:** Apache-2.0 License.
