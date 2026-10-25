# ECHO MAZE — Iterative Prompt Log & Dev Journey

This document records the exact, authentic prompts and iterative design evolution that built **ECHO MAZE** for the **ODS Weekly Dev Jam**.

---

## 📜 Dev Jam Prompt Evolution Log

### 🔹 Iteration 1: Fixing Visual Monotony
**User Prompt:**
```text
i think the screen should not be totally black, it shows boredom
```
**Outcome & Implementation:**
- Transformed the unvisited maze from pitch darkness into an atmospheric cyber-luminous grid.
- Added luminous cyan contour borders (`rgba(0, 243, 255, 0.2)`), linear-gradient floor tiles, and glowing tactical floor coordinate crosshairs.

---

### 🔹 Iteration 2: Core Survival Loop & Cop Patrol
**User Prompt:**
```text
now the path is totally visible, i think we should add a monster/cop starts from any top corner(left,right, left bottom,right bottom) and we have some time ticking anti-clockwise and we need to go the gate with in the span of time, and wherever i started moving the path will be black, there should be a small clue for the gate,
the movement should be of 1 block, and add some good and crazy animations
```
**Outcome & Implementation:**
- **1-Block Movement**: Replaced smooth continuous movement with responsive 1-block grid hopping, complete with squash-and-stretch bounce animations.
- **The Abyssal Void Trail**: Stepped-on paths collapse permanently into pitch black (`#000000`) with void purple crumb particles (`rgba(147, 51, 234)`), cutting off backtrack safety.
- **Corner Police Patrol**: Outer corner drone with sweeping 360° volumetric siren cones, dual-color rotating lightbars, and siren growls.
- **Anti-Clockwise Dial**: Mechanical countdown timer ticking down anti-clockwise to emergency lockdown.
- **Resonant Compass**: Golden needle clue rotating on the player's perimeter ring pointing directly to the exit beacon.

---

### 🔹 Iteration 3: Suspense Blackout Thriller Cycle
**User Prompt:**
```text
look, we are gonna a suspension we can black out the path for some seconds and then again visible paths, then again after few seconds(random) black out

this will increase the thriller
```
**Outcome & Implementation:**
- **Electrical Blackout Thriller System**: High-voltage electrical power cuts periodically plunge the maze into total darkness.
- **Warning Sparks & Screen Tremors**: Electric flickering warning phase before the cut, accompanied by screen shake and procedural Web Audio pitch-drops (`playPowerCut`).
- **Tactical Sensory Navigation**: In the blackout, visible floors vanish—players must rely on the golden compass clue, sound pings, and cop siren light cones.

---

### 🔹 Iteration 4: 15 Unique Levels & Swarming Cops
**User Prompt:**
```text
as we will moving to the higher level the cops should also increase in numbers, also add some more difficult and unique levels last 15 levels and as the level increases the thriller also increase and the dificulty too(add some unique levels)
```
**Outcome & Implementation:**
- **Expanded to 15 Distinct Handcrafted Levels**: Dimensions scale from 11×11 to 33×33.
- **Multi-Cop Corner Spawning**: Cops scale from 1 to 5 units, swarming from all 4 corners (Top-Left, Top-Right, Bottom-Left, Bottom-Right) and interior corridors.
- **Thriller Scaling**: Blackout dark intervals lengthen (up to 7.7s) and visible windows shrink as levels advance.
- **Specialized Units**: Sleeper Drones, Hound Cops (footstep hearing), Decoy Bells (`E`), Counter-Sonar Pulse Scanners, and Apex Hunters (Speed 3.9).

---

### 🔹 Iteration 5: Sequential Ironman Roguelike Rule
**User Prompt:**
```text
the level should be cross at the sequence like 1->2->3->4 if someone fails at 4 he needs to restart from 1
```
**Outcome & Implementation:**
- **Strict Linear Sequence**: Progression follows $1 \to 2 \to 3 \dots \to 15$.
- **Permadeath Failure Reset**: If captured or timed out at *any* stage (e.g. Level 4), the campaign fails and resets immediately to **Level 1** (`this.levelIndex = 0`).
- **High-Stakes HUD & Modal Feedback**: Captured and Timeout screens dynamically display `"Failed at Level X — restart from Level 1!"` with `RESTART FROM LEVEL 1 (R)` buttons.

---

## 🔍 Verification & Audit
- **Git Commit History**: Verified via commit logs on `vaibhavsrv/echo-maze`.
- **Portal Chat Export**: Complete raw chat session transcript preserved in `.gemini/antigravity-ide/brain/ca3a3b86-a3d7-4590-9c0b-fc1ca750261c/.system_generated/logs/transcript.jsonl`.
- **In-Engine Verification**: 19 automated self-tests running live at `https://vaibhavsrv.github.io/echo-maze/?debug=1`.
