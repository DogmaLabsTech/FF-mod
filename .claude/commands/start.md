---
description: First session for the FF7 Remake mod. Checks the setup, backs up saves, then starts recon.
---

Set up and start the FF7 Remake mod (`mods/ff7r-classic-sphere/`). Go step by step and tell the user what
you found at each one. Stop and ask before installing anything or touching the game folder.

1. **Toolkit.** If `universal-modder/` is empty, run `git submodule update --init`. Run `um --version`
   (or `universal-modder/bin/um --version`).
   - If it fails with `$'\r'` or `pipefail: invalid option`, the clone has Windows line endings. Fix it:
     `git -C universal-modder config core.autocrlf false && git -C universal-modder rm -rq --cached . && git -C universal-modder reset -q --hard`
   - If you had to init or fix the submodule, ask the user to restart Claude Code (so the skills and the PATH
     hook load) and run `/start` again.
2. **Tools.** Check `git --version` and `uv --version`. If uv is missing, give the user the install command
   from https://docs.astral.sh/uv/ and wait. Say whether `FAL_KEY` is set (optional, for generated art).
3. **Game.** Run `um scan --list`, then `um scan "FINAL FANTASY VII REMAKE"`. If it isn't found, ask for the
   install folder and pass it to `um scan`. Note the version, store (Steam/Epic) and save folder in `MODLOG.md`.
4. **Backup.** Run `um backup create "<save folder>" --name ff7r-saves` and write the restore command into `MODLOG.md`.
5. **Windows tools.** Offer `um win setup` (PowerShell tools + ffmpeg for screenshots and clips). Run it only on a yes.
6. **Recon.** Read `mods/ff7r-classic-sphere/README.md`, `RECON-NOTES.md` and `MODLOG.md`, then do step 0
   with the `mod-any-game` skill. Check every claim in `RECON-NOTES.md` against the real install.
7. **Report.** End with a short summary: what works, what's blocked, and the plan for step 1 (battles).
   Ask the user before you start it.
