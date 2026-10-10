# FF-mod

FINAL FANTASY VII REMAKE with the original game's turn-based battles and FF10's Sphere Grid for leveling,
built by your own Claude Code with [Universal Modder](https://github.com/rehan-remade/universal-modder).

## You need
- A Windows PC with **FINAL FANTASY VII REMAKE INTERGRADE** (Steam or Epic). Mods use your own copy; this
  repo never contains game files.
- [Claude Code](https://claude.com/claude-code), plus Git for Windows (Claude Code uses its Git Bash).
- [uv](https://docs.astral.sh/uv/getting-started/installation/), which runs the toolkit's `um` command.
- Optional: a [fal API key](https://fal.ai/dashboard/keys) as `FAL_KEY` for generated art (Sphere Grid icons
  and so on). Claude asks to approve the fal server the first time; say no if you don't have a key.

## Start
```bash
git -c core.autocrlf=false clone --recursive https://github.com/DogmaLabsTech/FF-mod.git
cd FF-mod
claude
```
Run it in Git Bash or PowerShell. The `-c core.autocrlf=false` part keeps the toolkit's scripts working on
Windows. Then type **`/start`** (or just say "Start the FF7 Remake mod"). It reads the plan in `mods/ff7r-classic-sphere/` and begins
with recon: scanning your install, trying the existing turn-based mods, and pulling the game's data.

Cloned without those flags? `/start` notices and fixes it.

## The plan
See [mods/ff7r-classic-sphere/README.md](mods/ff7r-classic-sphere/README.md). In short:
0. Recon. 1. Turn-based battles (others have done most of this). 2. A tiny Sphere Grid test (5 nodes, one
character). 3. The full Sphere Grid.

## Ground rules
Single-player only, saves backed up before the first modded launch, and nothing from the game is ever
committed or shared. Claude asks before it installs anything into your game folder.
