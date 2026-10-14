# FF-mod — agent brief

A starter kit for modding **FINAL FANTASY VII REMAKE INTERGRADE** (PC, UE4):
the 1997 game's turn-based battles plus an FF10-style Sphere Grid for
leveling, built with [Universal Modder](https://github.com/rehan-remade/universal-modder)
(MIT, third-party) as a git submodule.

- **Status: handed off.** Benny doesn't own the game. A friend who does clones
  this repo on his Windows PC and builds the mod with his own Claude Code.
  Nothing is built yet; the next step is recon (step 0 in
  `mods/ff7r-classic-sphere/README.md`). No service runs it, and there are no
  PM runs, issues or board card.
- `CLAUDE.md` holds the working instructions for the person modding (first
  session, where things are) and pulls in `universal-modder/AGENTS.md`.

## Run, test, build

- Setup: `git submodule update --init`, then open Claude Code in the repo and
  type `/start` (`.claude/commands/start.md`). A SessionStart hook in
  `.claude/settings.json` puts `um` (`universal-modder/bin/um`, run with `uv`)
  on PATH.
- Build and test happen on the friend's Windows PC against his own install of
  the game, through the `mod-any-game` skill and `um`. There is no test
  suite. If something here ever gets tests that run on Linux, run them on
  Benny's ROG with `crabbox run -- <cmd>` from the repo root, not on homebase.
- Optional: `FAL_KEY` for the fal MCP server in `.mcp.json` (generated art).

## Key folders

- `mods/ff7r-classic-sphere/`: the mod plan (`README.md`), `RECON-NOTES.md`,
  `MODLOG.md`, and the `battle/`, `sphere-grid/`, `shared/`, `assets/`,
  `showcase/` work areas.
- `universal-modder/`: the submodule, pinned at `8370faa`. Don't edit it.
- `.claude/skills/`: a copy of `universal-modder/skills/` (with its licence).
  Copy it again after `git submodule update --remote`.

## Rules and gotchas

- **Public repo.** Nothing from the game may ever be committed: no game files,
  `.pak`/`.utoc`/`.ucas`, extracted assets or decompiled code (keep those
  outside the repo, e.g. `~/ff7r-decomp/`). `.gitignore` blocks the usual
  ones; `um publish` lints for the rest.
- Single-player only. Back up saves before the first modded launch. Ask
  before installing loaders or touching the game folder.
- Windows line endings break `um` (`$'\r'` / `pipefail` errors): clone with
  `-c core.autocrlf=false`. `CLAUDE.md` has the fix.
- Build on the Nexus turn-based mods only if their permissions allow.
- Default branch is `main`.

## Remote

`origin` is https://github.com/DogmaLabsTech/FF-mod (public).
