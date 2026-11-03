# HD assets

How D2R's HD pictures, models and UI layouts are stored, and what a mod has to add so the game finds new ones.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

## Sprite files

HD UI pictures (items, panels, skill icons, buttons) are `.sprite` files. Each one has a `.lowend.sprite` next to it for the low graphics setting, with the same header and layout at half the pixel size.

A sprite is a 0x28-byte header followed by the pixels. Numbers are little endian.

| Offset | Type | Value |
|-|-|-|
| 0x00 | char[4] | `SpA1` |
| 0x04 | u16 | version, 31 |
| 0x06 | u16 | width of one frame (the full width for a single-frame sprite) |
| 0x08 | u32 | total width (frame count times the frame stride for a strip) |
| 0x0c | u32 | height |
| 0x10 | u32 | 0, purpose unknown |
| 0x14 | u32 | frame count |
| 0x18 | u32 | 0, purpose unknown |
| 0x1c | u32 | 0, purpose unknown |
| 0x20 | u32 | pixel data size (width times height times 4) |
| 0x24 | u32 | 4 |
| 0x28 | | RGBA8 pixels, row by row, top to bottom |

A sprite can be at most 16384 px wide. When you scale art up or down, weight the colour by alpha (bilinear with alpha weighting works) or the edges come out dark.

## Item pictures

An inventory cell is 98 px in HD and 49 px in the lowend sprite. Classic cells are 28 px, so classic art scales by 3.5. An item's sprite is `invwidth * 98` wide and `invheight * 98` high, with the picture centred.

Item sprites live under `data/hd/global/ui/items/<weapon|armor|misc>/<folder>/<name>.sprite`, plus the lowend copy. The asset name is the path under the weapon, armor or misc folder without the extension, for example `axe/hand_axe`.

Three files in `data/hd/items/` map items to pictures. Each is a JSON array of one-key objects.

| File | Key | Value |
|-|-|-|
| `items.json` | base item code | `{ "asset": "axe/hand_axe" }` |
| `uniques.json` | uniqueitems `index`, lower-cased, spaces and hyphens turned into `_`, apostrophes removed | `{ "normal": ..., "uber": ..., "ultra": ... }`, one picture per base tier |
| `sets.json` | setitems `index`, same rules | same as uniques |

Every item code, unique and set item needs an entry that points at a sprite that exists. Without one the game keeps asking for a default picture every frame and then crashes, for example on a vendor screen. Until you have art, point new items at a D2R picture of the same type.

Items always draw at 98 px per cell, whatever the grid's `cellSize` says. Making `cellSize` smaller to fit a bigger grid only makes the items overlap. Keep `cellSize` at 98 and give the grid's `rect` a `scale` instead. A drawn cell is then `98 * scale` px, so a 12 by 12 grid in the space of a 10 by 10 one needs a scale of 10/12 (about 0.83) or less. Give equipment slots the same scale.

## Skill icons

Each class has one HD skill icon sheet, such as `data/hd/global/ui/spells/amazon/amskillicon.sprite`. Skills with no class (Attack, Throw, item charges and oskills) all share `data/hd/global/ui/spells/submenu/skillicon.sprite`. Mercenary skills use `hireables/hrskillicon`.

A sheet is one horizontal strip with two frames per icon, normal then pressed. skilldesc `IconCel` is the frame number, so it is always even. Icons are 130 px with a 132 px stride, and D2R draws them at the sprite's own pixel size, without scaling them to the slot.

That is where the limit comes from: 16384 px divided by 132 px per frame and 2 frames per icon leaves room for 62 icons. You can fit more by making the frames smaller, but then the icons look small. The shared sheet is the one that runs out first, because every class-less skill reads it by `IconCel`, in classic and HD alike. If your mod adds many item skills, decide which ones keep an icon.

## New models and textures

`.model`, `.skeleton`, `.animations` and `.texture` files work at new paths as plain files in your mod folder, so you don't have to overwrite a D2R unit. But the game finds models and textures through two index files, not by looking at the file system. A mod's copy of either index replaces the game's whole file, so build both from the installed game's files at build time and add your paths. For each new entry, copy the entry of a D2R unit of the same kind.

`data/hd/model_lod_desc.bin` lists models. Its key is the 32-bit FNV-1a hash (offset basis 0x811c9dc5, prime 0x1000193) of the lower-cased game path, such as `data/hd/character/enemy/<unit>/torso.model`. A model that isn't listed still loads without an error, but it is never drawn, and the log says it failed to find a LOD for the model.

The file is little endian. It starts with three pairs of `u32 count, i32 offset`, where each offset is relative to its own position. The first array holds 44-byte entries sorted by key:

| Field | Type |
|-|-|
| key | u32 |
| bounding box min | f32[3] |
| bounding box max | f32[3] |
| LOD mask (bit k+1 = LOD k), then 3 padding bytes | u8 |
| LOD count | u32 |
| streamOffset | u32 |
| stream size | u32 |

`streamOffset` is relative to its own position and points into the second array, which in turn points into the third. Adding an entry moves the second and third arrays, so you have to rewrite the `streamOffset` of every entry, not just the new one. A stale offset makes the game die during its first preload.

A listed model is opened as `<base>_lodN.model` for each LOD in its mask. Animations are read from `combined.animations` in the clip's folder, not from single `.animation` files. Unit JSON, `monsters.json` and state machine JSON files are not indexed, so any path works for them.

`data/hd/texture_desc_cache.json` lists textures by `filename`. A texture that isn't listed is read with the wrong format or size, and you get colour noise or a lost GPU device.

## Layout JSON

The HD UI is described by `data/global/ui/layouts/*hd.json`. The classic UI uses the same names without `hd`. A mod replaces a layout by shipping a file with the same name. Start from D2R's own file and change only the widgets you need, since the panel code looks widgets up by name.

The files are JSON with `//` comments and trailing commas. Edit them as text or with a lenient parser; a strict JSON library will drop the comments or refuse the file.

- `"basedOn": "<Other>HD.json"` inherits another layout. Children listed again under the same name override or extend the inherited ones.
- Values that start with `$`, such as `$ItemCellSize` or `$LeftPanelRect`, are variables from `_profilehd.json`. The low-vision and Asian profiles have their own files (`_profilelv.json`, `_profileasian.json`).
- `rect` is `{ "x", "y" }` with optional `width`, `height` and `scale`. `scale` scales the widget and everything in it. Coordinates are relative to the parent.

Panels are 1162 by 1507 HD pixels. The controller layouts under `controller/` are based on the PC ones and also refer to widgets by name, so when you rename, move or resize a widget, make the same change there.
