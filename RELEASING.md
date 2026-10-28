# Making a release

The RAC-RS Launcher finds updates through this repository's **latest GitHub release**. It asks `https://api.github.com/repos/AsobiCodes/arsenal-mastery/releases/latest`
(no login, one request per launcher start), compares the release tag with the version of the installed mod, and downloads the asset
`arsenal-mastery-<version>.zip`. So a release must follow these rules, or the launcher will not offer it (it says why in the mod's Details):

| Rule | Why |
|---|---|
| Tag `v<version>`, for example `v0.3.4` (the `v` is optional for the launcher, but keep it) | the launcher compares this with the installed version |
| `"version"` in `mod/omni-mod.json` equals the tag without the `v` | the launcher refuses a package whose manifest says another version |
| Version is `MAJOR.MINOR.PATCH` (a `-beta.1` style suffix is a pre-release: sorts below the plain version) | same rules as the mod loader |
| The release has the asset `arsenal-mastery-<version>.zip` | the name is built from the mod id and the version |
| `omni-mod.json` sits at the **root of the zip** (or inside exactly one top folder) | the launcher unpacks the zip as the mod's folder |
| The release is not a draft and not marked "pre-release" | those are ignored |
| The asset has a SHA-256 digest (GitHub adds it itself to every uploaded asset) | the launcher refuses a download it cannot check |
| The zip holds no `plugin` entry in `omni-mod.json` | a script mod update may not bring program files |
| `"update": { "github": "AsobiCodes/arsenal-mastery" }` stays in `omni-mod.json` | this is how the launcher knows where to look |

## Steps (Windows PowerShell, from the repository folder)

1. Change the mod in `mod\`. Bump `"version"` in `mod\omni-mod.json` (say `0.3.4`).
2. Add the entry for the version at the top of `CHANGELOG.md`.
3. Make the zip. Do **not** use `Compress-Archive` (Windows PowerShell 5.1 writes `perks\blaster.luau` with a backslash into the zip; other
   tools then unpack a file called `perks\blaster.luau`). `tar.exe` (part of Windows 10 and later) writes proper zips:
   ```powershell
   $v = '0.3.4'
   Push-Location mod
   tar.exe -a -c -f "..\arsenal-mastery-$v.zip" *
   Pop-Location
   ```
   (On Linux or macOS: `cd mod && zip -r ../arsenal-mastery-0.3.4.zip .`)
4. Check the zip: `omni-mod.json` is listed without a folder in front of it.
   ```powershell
   tar.exe -tf "arsenal-mastery-$v.zip" | Select-String 'omni-mod.json'
   ```
5. Write the notes of this version into a file `notes.md` (copy the top section of `CHANGELOG.md`), then publish with the GitHub CLI:
   ```powershell
   gh release create "v$v" "arsenal-mastery-$v.zip" --title "Arsenal Mastery $v" --notes-file notes.md --latest
   ```
   (Or on github.com: Releases > Draft a new release > choose or create the tag `v0.3.4` > attach the zip > Publish release. Do not leave it as a draft.)
6. Check what the launcher will see:
   ```powershell
   gh api repos/AsobiCodes/arsenal-mastery/releases/latest --jq '{tag: .tag_name, draft: .draft, pre: .prerelease, assets: [.assets[] | {name, size, digest}]}'
   ```
   The tag is `v0.3.4`, the asset is `arsenal-mastery-0.3.4.zip`, and `digest` starts with `sha256:`.
7. Open the RAC-RS Launcher with an older Arsenal Mastery installed, go to the Mods page: the row shows **Update to 0.3.4**.

Delete the zip and `notes.md` afterwards (`.gitignore` keeps them out of the repository anyway). Never replace the asset of a published release
under the same version: players who have it would not get the change. Publish the next version instead.

## What the launcher does with the zip

Download to a staging folder next to `mods` (never inside it: the game would load a second copy), SHA-256 check against the digest, unpack
(size and file-count limits, no path tricks), check that the package is the same mod id at the announced version, move the old mod folder to
`mod_backups\arsenal-mastery-<old version>` and the new one in its place. If anything fails before the swap, nothing is changed.

## Testing without GitHub

The launcher can read releases from a folder instead of GitHub (for tests): `RAC-RS Launcher.exe --mod-releases <folder>` reads
`<folder>\<owner>\<repo>\latest.json` (same fields as the GitHub API: `tag_name`, `body`, `assets[]` with `name`, `size`, `browser_download_url`
(a file name relative to that folder) and `digest`).
