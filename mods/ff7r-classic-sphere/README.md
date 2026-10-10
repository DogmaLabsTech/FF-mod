# FF7 Remake: classic battles + Sphere Grid

Status: Idea. Folders set up 2026-10-09, nothing built.

Game: FINAL FANTASY VII REMAKE INTERGRADE (PC, Steam app 1462040), Unreal Engine 4. You need your own copy.

## Goal
1. Battles play like the 1997 game: turn-based ATB, menu commands, no action combat.
2. Characters grow on an FF10-style Sphere Grid instead of level-ups.

## Folders
- `battle/`: part A. Turn-based mods already exist on Nexus (Turn Based Combat #2192, built on Classic Combat
  - Wait Mode ATB #369). Play them first. Build on one only if its permissions allow; otherwise make our own pak.
- `sphere-grid/`: part B. Nothing like it exists; the biggest and riskiest part. `design/` holds the board
  layout, node types and sphere items before any code.
- `shared/`: stat and ability ID tables both parts use.
- `assets/`: our own art (grid UI, sphere icons). Never game files.
- `build/`: packed output, not kept.
- `showcase/`: clips and screenshots.

Desk research for recon: `RECON-NOTES.md`.

## Rules
- Extracted game files and decompiled code go in `~/ff7r-decomp/`, never in this folder.
- Back up saves (`um backup`) before the first modded launch.
- Single-player only. Never ship game files.

## Order
0. Recon: install the game, `um scan`, try the built-in Classic mode and the existing turn-based mods, extract
   the data (FModel / ff7r-data-editor).
1. Part A: turn-based battles working (likely data/pak only).
2. Part B slice: one character, a 5-node grid, spend AP to unlock +HP. Proves the hard bit (custom menu + stat hook).
3. Part B: the full grid.
