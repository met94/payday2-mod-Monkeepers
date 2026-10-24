# Monkeepers

AI teammates carry and route loot bags like players - a community-maintained
fork of TdlQ's [Monkeepers](http://pd2mods.z77.fr/monkeepers.html), updated for
**PAYDAY 2 248.2** (64-bit, SuperBLT).

Version **87.1** is the first fork release: upstream r87 plus 248.2 fixes for
person-and-bag ziplines and vehicle dropzones.

## Status

- Upstream: TdlQ, last revision r87.
- This repository: fork maintenance by met94.
- Base: upstream r87; first fork release 87.1.
- Verification: static compatibility checks against the 248.2 game Lua; an
  in-game smoke test is still pending.
- Auto-updates come from this repository's GitHub releases, not from
  `pd2mods.z77.fr`. The upstream feed only knows about upstream packages.

## How it works

From the original mod page:

> This mod makes NPC teammates carry bags like players show them. Basically, it
> remembers the position where you take a bag. Then, if you drop that bag at
> least 7 meters from there (the important position is not yours but where your
> bag lands), it creates a transport rule that associates source position with
> where and how to throw bags.
>
> A transport rule is applied to all bags in a radius of 6 meters around the
> source position. If the source area is empty for 90 seconds, the rule is
> destroyed.

Most of the time you do not need to manage routes: play normally and the mod
creates and expires them for you. In stealth, wake bots first with Keepers so
they can take the job.

## Main features

- Automatic bag transport from pickup points to drop zones, including vehicles.
- Per-level bag behaviour: **None**, **Same bag contents**, **Same bag weight**
  (default) or **All**.
- Bots catch bags thrown at them, with a toggle to disable it.
- With Keepers installed, bots use bag-capable ziplines, including the
  person-and-bag `"both"` type, and pick bags up from shelves.
- Vehicle delivery follows 248.2 rules: a vehicle that is not accepting loot is
  ignored, and each vehicle's allowed bag list / unique-loot and bag-value rules
  are respected.
- Host keybind deletes all routes and cancels outstanding carry objectives.
- Keepers integration: put-back-in-place orders and the full loot workflow.

See [MENU_OPTIONS.md](MENU_OPTIONS.md) for the options and per-level modes.

## Requirements

- PAYDAY 2 248.2, 64-bit.
- [SuperBLT](https://superblt.znix.xyz/).
- Keepers is optional but recommended. Without it, bots cannot be ordered in
  stealth, put-back orders do not exist, and shelf/zipline transport is
  unavailable; basic pickup-to-drop routes still work.

## Installation

1. Close PAYDAY 2.
2. Remove or rename an older `Monkeepers` folder in `...\PAYDAY 2\mods\`.
3. Download `Monkeepers_v87.1.zip` from
   [Releases](https://github.com/met94/payday2-mod-Monkeepers/releases/latest)
   and extract it so the result is exactly:
   `...\PAYDAY 2\mods\Monkeepers\mod.txt`
4. Launch the game and open **Mod Options -> Monkeepers**.

If you download the GitHub source ZIP instead, rename the extracted
`payday2-mod-Monkeepers-master` folder to `Monkeepers`.

> Do not install two folders with the same `mod.txt` name. SuperBLT treats the
> name as the dependency key and one copy will win unpredictably.

## Auto-updates and versioning

`mod.txt` points SuperBLT's updater at
`releases/latest/download/meta.json` in this repository.

Versions use `<upstream revision>.<fork release>`:

- `87.1` is the first fork release on top of upstream r87;
- `87.2`, `87.3`, ... are later fork releases on the same upstream base;
- after merging a new upstream revision the base changes (`88.1`).

Upstream integer revisions are never reused, so a future TdlQ release cannot
collide with a fork build. Release tags mirror the version (`v87.1`).

## Changes in 87.1

- Ziplines whose usage type is `"both"` (person and bag) are now treated as
  bag-capable. Previously bots ignored them for bag transport.
- Vehicle dropzones that are not accepting loot are skipped instead of being
  handed bags they will refuse.
- Vehicle dropzone filters mirror the 248.2 `_loot_filter_func`: explicit
  per-carry filters win, `allow_only_filtered` refuses unlisted carries, and
  otherwise unique loot or carries with a bag value are accepted.
- Auto-updates moved from the upstream feed to this repository's GitHub
  releases.

## Compatibility

Monkeepers hooks 12 game files: mission elements, player and route manager,
bag/carry data, cop logic, team AI movement/idle, ziplines and vehicles.

Keepers is the intended companion. Keep Monkeepers at priority **100** and
Keepers at its default **0**. Both mods wrap `MissionScript._create_elements`
and chain correctly, but `TeamAILogicIdle.action_complete_clbk` is a full
override here that Keepers wraps, so the tested and supported order is
Monkeepers first.

Settings are saved to `...\PAYDAY 2\mods\saves\monkeepers.txt`; only per-level
behaviours that differ from the default are stored.

After changing your load order, check `...\PAYDAY 2\mods\logs\`; the expected
line is `Setting up mod 'Monkeepers'` and there should be no `FATAL ERROR`.

## Development

Repository layout:

```text
lua/            route engine, bag/carry hooks, zipline and vehicle adapters
menu/options.txt  SuperBLT options menu definition
loc/            translations
changelog.txt   release notes / player notes
README.md       this file
MENU_OPTIONS.md player-facing option guide
```

Lua syntax gate (LuaJIT is not shipped in this repository):

```powershell
luajit -bl lua\*.lua
```

When changing code that mirrors a game function body, also check free variables
against the matching 248.2 game file.

Release checklist: bump `version` in `mod.txt`, prepend a `Revision N:` block to
`changelog.txt` (newest first, ending with `Player notes:`), build a zip whose
single root folder is `Monkeepers`, and publish it with a `meta.json` asset from
a release tagged `v<version>`. The asset is an array containing `ident`
(`Monkeepers`), `version` (must equal `mod.txt`), `download_url` and optionally
`patchnotes_url`.

## Credits

- Original mod, design and code: **TdlQ**.
- Translations: NewPJzuza (Thai), LR_Daring/Arknights (Chinese), HYGRAWSKY
  (Portuguese), Kilowide (Spanish).
- Fork maintenance 87.1: **met94**.

## License

The original Monkeepers archive (r87) ships no license file, and the original
mod page states none. The fork changes in this repository are released under the
**Unlicense** (public-domain dedication); see [LICENSE](LICENSE).

The original mod name, code, localization files, icons and any retained
original content remain credited to their original authors. No ownership is
claimed over those parts. If you are an original author and want different
terms, open an issue.
