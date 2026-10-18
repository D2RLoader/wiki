---
title: Make your first mod
description: Create and test a folder-style mod.
published: true
date: 2026-10-13T05:03:33.755Z
tags: 
editor: markdown
dateCreated: 2026-10-13T05:03:32.959Z
---

# Make your first mod

This guide shows the basic D2RLoader folder layout for a softmod. A softmod uses game data files without a plugin DLL.

For more modder guides, see [/en/modders](/en/modders). For the full D2RLoader guide, see the [documentation](https://d2rloader.net/docs.html).

## Before you start

- Install D2R and D2RLoader.
- Work on a copy of an existing mod if you want to keep it safe.
- Back up offline saves before you test changes that can affect saved characters.
- Read the current D2RLoader documentation. Commands and features can change between versions.

## Create the mod folder

Make a folder under the game's `mods` folder. Use that folder name as the mod name:

```text
<game>\mods\MyMod\
|-- d2rloader\
`-- MyMod.mpq\
```

The `.mpq` path can be a folder while you edit the mod. This layout is a folder-style MPQ. Put game data under it, using the game's data paths. For example, Excel TXT files go here:

```text
mods\MyMod\MyMod.mpq\data\global\excel\Example.txt
```

Do not copy files from another mod unless its license allows it.

## Add D2RLoader files

At the main menu or character-select screen, press `Ctrl + \`` to open the console. Enter `d2rl init MyMod`. This command creates D2RLoader integration files for the named mod.

The `d2rloader` folder holds loader files, such as metadata, settings, plugins, and JSON patches. Do not put game data in this folder. Keep game data in `MyMod.mpq`.

## Test your changes

Start the mod with `D2RLoader.exe -mod MyMod`. Replace the name if your folder uses another name.

For a folder-style MPQ, edit the TXT files. Enter `d2rl compile MyMod` in the console to compile them into BIN files. This command writes the BIN files beside their TXT sources. Do not use `-txt` as a replacement for this command. That start option tells the game to load its Excel tables from TXT files.

Test with a character that you can safely lose. A mod can change the data used by a save. Do not use a valued character to test a new save format or an unfamiliar mod.

When the basic mod works, use `d2rl pack MyMod` to package the folder-style MPQ. Read the console help before you use optional pack flags.

## Keep the mod safe

- Back up the mod before you use `init`, `update`, `compile`, or `pack` on an existing project. These commands can create or replace files in the target mod.
- Keep a separate backup of your offline characters and shared stash.
- Match each save to the Vanilla or mod environment that owns it. Do not restore a save while its character is in a game.

The in-game `help` and `d2rl` commands show the commands supported by your installed version.
