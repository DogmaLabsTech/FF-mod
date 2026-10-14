# FF-mod

A game-modding repo. The person you're working with owns the game and plays on their own PC. You do the
modding. Universal Modder (the toolkit) is installed for this repo only.

@AGENTS.md

@universal-modder/AGENTS.md

## First session
The user may run `/start`, which walks through all of this. Otherwise:
1. If `universal-modder/` is empty, run `git submodule update --init`. If `um` fails with `$'\r'` or
   `pipefail: invalid option`, the clone has Windows line endings: run
   `git -C universal-modder config core.autocrlf false && git -C universal-modder rm -rq --cached . && git -C universal-modder reset -q --hard`.
   After either fix, ask the user to restart Claude Code so the skills and the `um` PATH hook load.
2. Read `mods/ff7r-classic-sphere/README.md`, `RECON-NOTES.md` and `MODLOG.md`, then start step 0 (recon) with the
   `mod-any-game` skill. Keep `MODLOG.md` up to date as you go.
3. Follow the toolkit's rules above: ask before installing loaders or touching the game folder, back up saves
   first, single-player only, never commit game files or extracted assets.

## Where things are
- `universal-modder/` is a git submodule of rehan-remade/universal-modder (MIT, third-party). Don't edit it.
  - The skills say `bin/um` and `knowledge/`: from this repo's root those are `universal-modder/bin/um`
    (or plain `um`, which the SessionStart hook puts on PATH) and `universal-modder/knowledge/`.
- `.claude/skills/` is a copy of `universal-modder/skills/`, so Claude Code finds them. After updating the
  submodule (`git submodule update --remote`), copy them over again.
- `.claude/settings.json` holds the PATH hook and pre-approves read-only `um` commands. `/start` is in `.claude/commands/`. `.mcp.json` adds the fal server for generated art (needs `FAL_KEY`).
- `mods/<mod-name>/` is where each mod lives, with its own README and MODLOG.
- Extracted game files and decompiled code go outside the repo (e.g. `~/ff7r-decomp/`).
