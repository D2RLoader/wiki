---
title: Installing a mod
description: Install and start a D2RLoader mod.
published: true
date: 2026-10-13T05:03:32.952Z
tags: 
editor: markdown
dateCreated: 2026-10-13T05:03:30.535Z
---

# Installing a mod

D2RLoader loads a mod from the game's `mods` folder. Each mod has its own folder and a mod archive.

For more player guides, see [/en/players](/en/players). For the full D2RLoader guide, see the [documentation](https://d2rloader.net/docs.html).

## Before you start

- Read the mod author's install notes. Mods can need different files or steps.
- Check that your D2RLoader version supports the mod.
- Back up your offline character and shared stash saves before you change mods or save formats.
- Do not restore a save while that character is in a game.

D2RLoader can make automatic backups before an offline character's first save in each play session. Do not treat this as a reason to skip your own backup. A save from one environment may not work in another.

## Install the mod

1. Find the D2R game folder. It is the folder that contains `D2R.exe`.
2. Open the `mods` folder in that game folder. Create it if needed.
3. Follow the mod author's instructions to place the mod folder there.
4. Check that the mod has a matching archive folder or file and a `modinfo.json` file when its format requires one.

A typical folder-style mod has this shape:

```text
<game>\mods\ModName\
|-- d2rloader\
`-- ModName.mpq\
    `-- data\
```

`ModName.mpq` can be a folder or a packed file. A folder-style mod needs `modinfo.json` inside its `.mpq` folder. The mod author may provide a different layout or extra steps.

## Start the mod

To start a mod directly, start D2RLoader with its name:

```text
D2RLoader.exe -mod ModName
```

Replace `ModName` with the folder name under `mods`. You can also select a detected mod in D2RLoader's Default Mod setting. Apply the setting and restart before you load a character.

Do not switch between Vanilla and a mod while a character is in a game. Use the matching environment when you select or restore a save. Check the save's environment details first.

## If the mod does not appear

- Check the folder name and location.
- Check that the mod archive matches the folder name.
- For a folder-style archive, check for `modinfo.json` inside it.
- Follow the mod author's instructions. Some mods need files that D2RLoader does not create.

See [Modder guides](/en/modders) if you make a mod.
