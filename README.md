# Arsenal Mastery

Ratchet & Clank 3 style weapon levels for **Ratchet & Clank 1**, as a script mod for [RAC-RS](https://github.com/AsobiCodes/rac-rs) (the PC port).
Every weapon, and the wrench, earns XP from the damage it deals and grows from V1 to V5 (V8 in New Game+, V99 in Endless). The enemies are
rebalanced to match. Optional: nanotech levels (health that grows like in Ratchet & Clank 3), gold weapons at V5, Mega weapons at the gold weapon
cases in New Game+, and Survival ammo. XP is saved per save slot.

Everything the mod does, and every setting, is described in [mod/README.md](mod/README.md).

**You need** RAC-RS 0.4.0 or newer (it has the OmniLoader mod loader built in), and your own copy of the game. On any other loader the mod logs one
line and does nothing.

## Install with the RAC-RS Launcher (recommended)

Arsenal Mastery comes with RAC-RS 0.4.0 and later, switched off. Open the launcher, go to **Mods**, and switch it on. If you removed it, the
**Get Arsenal Mastery** button on the Mods page downloads it again from this repository (it is added switched off).

**Updates:** when a newer version is published here (under Releases), the Mods page shows **Update to X.Y.Z** on the mod's row, and **What's new**
opens the release notes. One click downloads the release, checks it, and replaces the mod's folder. The old version is kept in the `mod_backups`
folder next to `mods` (the newest three are kept). Your settings (`mods\settings.json`), your on/off list and your XP in the save slots are not
touched. The new version is used the next time the game starts. The launcher only looks for updates while the Mods page is open, once per start.

## Install by hand (no launcher, or on Linux)

1. Download `arsenal-mastery-<version>.zip` from the [latest release](https://github.com/AsobiCodes/arsenal-mastery/releases/latest). Do not use
   "Code > Download ZIP": that is the whole repository, not the mod.
2. Unpack it into a new folder `mods/arsenal-mastery` inside your RAC-RS folder (the one with `VERSION`, `player` and `mods` in it; make `mods`
   if it is not there). The file `omni-mod.json` must end up at `mods/arsenal-mastery/omni-mod.json`, not one folder deeper.
   The folder name does not matter to the game, it finds the mod by the `"id"` inside `omni-mod.json`.
3. Switch it on: open `mods/active.txt` (make it if it is missing) and add the line `arsenal-mastery`. The file looks like this:
   ```
   # Enabled mods, one id per line, highest priority first.
   arsenal-mastery
   ```
   A line `# Load mods: off` in that file is the "Load mods" master switch: while it is there, no script mod loads. Remove the line to load them again.
   If you also use the launcher, switch the mod on in its Mods page instead: it writes the list of the selected profile and copies it to
   `mods/active.txt` when you press Play.
4. Settings are optional. They live in `mods/settings.json` under `"arsenal-mastery"` (see [mod/README.md](mod/README.md#settings) for the
   keys); a missing or wrong value uses its default. Edit it while the game is closed.
5. Start the game. The game's log then has a line starting with `[am] ready:`.

The paths are the same on Linux (with `/`); use the RAC-RS folder you run the game from.

**Update by hand:** download the new zip, delete the files of the old `mods/arsenal-mastery` folder (or move the folder away as a backup, but
not into `mods`: the game would load it as a second copy), and unpack the new zip in its place. `mods/settings.json` and `player/omni`
(your XP) are not part of the mod folder and stay.

## Releases and versions

Versions are `MAJOR.MINOR.PATCH`. A release `v0.3.4` carries the file `arsenal-mastery-0.3.4.zip`, and `"version"` in `omni-mod.json` says
`0.3.4`. See [CHANGELOG.md](CHANGELOG.md) for what changed, and [RELEASING.md](RELEASING.md) for how a release is made.

## Licence

MIT, see [LICENSE](LICENSE). The mod contains no game data.
