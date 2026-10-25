# ECHO MAZE 🦇🔊
> **"Light costs noise. The clock is ticking."**

A tactical stealth maze game built for the **ODS Weekly Dev Jam**.

🎮 **Play Live Online:** [https://vaibhavsrv.github.io/echo-maze/](https://vaibhavsrv.github.io/echo-maze/)

---

## ⚡ Game Mechanics
- **1-Block Tactical Movement**: Discrete grid hopping with ease-out tweening and responsive squash-and-stretch physics.
- **The Pitch-Black Void Trail**: Everywhere you move, the path behind you collapses into an abyssal pitch-black void.
- **The Corner Cop Patrol**: Sci-fi police patrol drone spawns in outer corners with 360° sweeping volumetric red & blue siren searchlights and high-speed emergency strobe lightbars.
- **Anti-Clockwise Countdown Dial**: A cyber radial dial sweeps counter-clockwise with mechanical audio ticking and emergency lockdown upon hitting 0.
- **Acoustic Gate Clues**: Dynamic golden compass needle rotating on the player's perimeter ring pointing directly to the gate, accompanied by drifting golden echo wisps.
- **Synthesized Web Audio**: 100% procedurally synthesized audio (sonar pings, footsteps, cop siren chirps, clock ticks, dynamic proximity heartbeat, and resonant portal hums). No external audio assets required.
- **Cross-Device Ready**: Full support for desktop keyboard (WASD / Arrows), touch D-Pad, and swipe gestures on mobile devices (iOS, Android, Tablets).

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

Run self-tests in console by appending `?debug=1` to the URL.
