# Recon notes: FF7 Remake Intergrade (PC)

Desk research from 2026-10-09, done without the game. Treat every line as a lead, not a fact: check it
against the real install and record what you find in `MODLOG.md`. Tags: **[confirmed]** = sourced and
cross-checked, **[likely]** = one source or inferred, **[unknown]** = not found.

## Engine and files
- Square Enix's modified **UE 4.18**, with unversioned properties (a 4.25+ feature). Stock tools set to plain
  4.18 fail. [confirmed] [Gildor](https://www.gildor.org/smf/index.php/topic,6925.msg42030.html), [UE4SS #595](https://github.com/UE4SS-RE/RE-UE4SS/issues/595)
- Project name `End`. The real exe is `End\Binaries\Win64\ff7remake_.exe` (root `ff7remake.exe` is a stub).
  Steam build 1.0.0.7 (Feb 2026). [confirmed/likely] [RE notes](https://github.com/Gilleece/ff7-remake-vr/blob/main/docs/re/engine.md)
- Paks in `End\Content\Paks\` (`pakchunk0*`). Plain .pak, no IoStore expected. [likely]
- Paks are encrypted. The AES key is public and documented in the data-editor wiki's UE Viewer section.
  Don't commit it here. [confirmed] [wiki](https://github.com/jordanbtucker/ff7r-data-editor/wiki/Getting-Started)
- No signature checks documented; unsigned paks in `~mods` load. [likely]
- No Denuvo, no anti-cheat. [confirmed] [DSOGaming](https://www.dsogaming.com/?p=160539)

## Tools
- **FModel / CUE4Parse:** pick the `GAME_FinalFantasy7Remake` profile, not plain 4.18. [confirmed] [EGame.cs](https://github.com/FabianFG/CUE4Parse/blob/master/CUE4Parse/UE4/Versions/EGame.cs)
- **Extract:** spiritovod's patched UModel (`umodel_FFVII_intergrade`) or QuickBMS `ff7r.bms`. A full extract
  is ~200 GB: dump only `End\Content\GameContents\DataObject\` first. [confirmed] [wiki](https://github.com/jordanbtucker/ff7r-data-editor/wiki/Getting-Started)
- **Pack:** FluffyQuack's UnrealPak bundle (`UnrealPak-With-Compression.bat`); put the pak in
  `End\Content\Paks\~mods\` (create it). `_P` suffix and repak support: [unknown].
- **Data editor:** ff7r-data-editor **v1.0.0-b.8** (v2.x is Rebirth-only). Edits `.uexp` values in place, no
  added rows, CSV import/export. Beta. [confirmed] [releases](https://github.com/jordanbtucker/ff7r-data-editor/releases)
- **UE4SS:** RE-UE4SS ships a `Final Fantasy 7 Remake` custom game config (copy it in). Use a current
  nightly/experimental build: a post-hook crash on the latest patch was fixed after 3.0.1. No FF7R Lua mods
  published yet. [confirmed] [configs](https://github.com/UE4SS-RE/RE-UE4SS/tree/main/assets/CustomGameConfigs), [#1173](https://github.com/UE4SS-RE/RE-UE4SS/issues/1173)
- **FFVIIHook** (emoose, `xinput1_3.dll`): dev console + loose files. It disables itself on exe builds it
  doesn't know; support for the 2026 build is [unknown]. Don't stack it with UE4SS's proxy DLL without checking.
  [Nexus 74](https://nexusmods.com/finalfantasy7remake/mods/74)
- Reflected game classes exist (e.g. `EndBattleAPI.GetBattleSceneID`), callable via ProcessEvent. [likely]
  [engine-module.md](https://github.com/Gilleece/ff7-remake-vr/blob/main/docs/engine-module.md)

## Data tables (under `End\Content\GameContents\DataObject\`)
- Known: `Resident\Equipment` (weapon stats), `BattleCharSpec`, `BattleAICharaSpec`, `EndGameMode`. [confirmed]
- Not found: the tables for ATB, level growth, weapon skill cores (SP) and materia AP. Finding them is recon's
  main job. [unknown]

## Part A: battles (prior art on Nexus)
- **Turn Based Combat #2192** (BrazilRules, v0.9, Jul 2026): 1 ATB per attack, ATB capped at 1 bar,
  instant spells, enemies wait their turn; Normal and Accurate (Limits cost ATB) versions. Built on #369;
  a data-edit pak that clashes with other battle mods. Permissions look open for modifying (re-upload
  banned): read them on the page. [likely] [Nexus](https://www.nexusmods.com/finalfantasy7remake/mods/2192)
- **Classic Combat - Wait Mode ATB #369** (Syynx, v3.20): ATB fills over time, attacks don't fill it.
  Formal permissions say ask first, a pinned comment says "100% open": get the author's okay before reusing
  anything. [likely] [Nexus](https://www.nexusmods.com/finalfantasy7remake/mods/369)
- Others to look at: #876, #946, #240, #1352, #2254. "True Wait Mode" is a 1997-game mod, not Remake.
- Built in: Classic difficulty (auto-attacks, you pick commands). Not truly turn-based.
- Likely route: play #2192, then make our own data-edit pak for the gaps.

## Part B: Sphere Grid
- Nothing like it exists for Remake or Rebirth. UI mods so far only swap textures. [unknown/likely]
- Cooking new UMG screens for this 4.18 fork is unproven. A Rebirth (4.26) editor project exists
  ([FF7R2UProj](https://github.com/narknon/FF7R2UProj)), not one for Remake.
- So the step 2 test must answer: can we show a custom screen (cooked UMG pak, or built at runtime from UE4SS
  Lua/C++), and can we change a character's stats from it?

## Saves and settings
- Steam: `Documents\My Games\FINAL FANTASY VII REMAKE\Steam\<id>\`. Epic: `...\FINAL FANTASY VII REMAKE\EOS\`.
  Files `ff7remake000.sav`-`030.sav` (000 = autosave). Format undocumented. [confirmed/likely]
- Config: `...\Saved\Config\WindowsNoEditor\Engine.ini`. DX12 stutters; `-d3d11` is common. [likely]

## Gotchas
- Patches break hook mods (2023, Feb 2024, Dec 2024; the Jan 2026 patch is a risk). Record the exe build.
- Battle mods that edit the same tables overwrite each other: one battle pak at a time.

## Check first on the real install
1. Exe build/timestamp; does the current UE4SS nightly load with the FF7R config?
2. Which DataObject tables hold ATB, growth, skill cores, materia AP (dump with FModel's FF7R profile).
3. Does our own packed pak load (compression, `_P` naming, mount point)?
4. Custom UI: cooked UMG vs runtime-built (decides how step 2 is done).
5. Current Nexus permissions for #2192 and #369.

Hubs: [Nexus forum](https://forums.nexusmods.com/forum/9001-vii-remake/), [Gildor thread](https://www.gildor.org/smf/index.php/topic,6925.0.html),
OpenFF7R Discord (named in the wiki; reportedly quieter now).
