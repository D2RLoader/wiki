# Save format (v105)

The layout of a retail D2R character save (`.d2s`, version 105), for modders who want to read saves or edit test characters.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

Back up your saves before you edit one, and test on a character you can afford to lose. Do not edit or restore a save while its character is in a game.

D2RLoader can be set to use a different save format (see [Installing a mod](/en/players/installing-a-mod)). The layouts on this page don't apply to saves written in that format.

## Sections

All numbers are little endian. The file is a fixed header and three fixed-size sections, then variable-length sections one after another.

| Offset | Size | Section |
|-|-|-|
| 0x000 | 403 | Header |
| 0x193 | 298 | Quests: `Woo!`, u32 6, u16 0x12a, then 48 u16 quest words per difficulty |
| 0x2bd | 80 | Waypoints: `WS`, u32 1, u16 0x50, then per difficulty u16 0x102, 112 waypoint bits and 8 zero bytes |
| 0x30d | 52 | NPCs: u16 0x7701, u16 0x34, then 3 u64 intro flags and 3 u64 congratulation flags |
| 0x341 | varies | `gf`, character stats |
| next | 2 + n | `if`, then one byte per class skill |
| next | varies | `JM`, u16 item count, the items |
| next | varies | `JM`, corpse count, then per corpse three u32 and a `JM` item list |
| next | varies | `jf`, then a `JM` item list only if the character has a mercenary |
| next | 3 or more | `kf`, a presence byte (0 or 1), then the golem's item if present |
| next | 6 or more | `lf` section, when present |

Some header fields:

| Offset | Field |
|-|-|
| 0x00 | u32 signature 0xaa55aa55 |
| 0x04 | u32 version, 105 |
| 0x08 | u32 file size |
| 0x0c | u32 checksum |
| 0x10 | u32 weapon set (bit 0 set when switched) |
| 0x18 | u8 class |
| 0x1a | u8 number of class skills |
| 0x1b | u8 level |
| 0x9f | mercenary: u32 flags (0x10000 = dead), u32 seed, u16 name, u16 type, u32 experience |
| 0xf8 | u8 game type (3 = Warlock-capable) |
| 0x12b | character name, NUL terminated |

The game counts the mercenary as present when its experience, seed or name is not zero, and only then reads the item list after `jf`.

The `lf` section is u16 1, `lf`, a presence byte and a zero byte. When the presence byte is 1, a 0x5c-byte record follows whose first u16 is the length of the data after it. The game writes this section only for game type 3.

## Checksum

Write the file size at 0x08 first. Then set the four checksum bytes at 0x0c to zero and run over every byte of the file: rotate a 32-bit value left by one bit and add the byte (unsigned). Write the result at 0x0c.

## Stats

The `gf` list is bit-packed, least significant bit first. Each stat is a 9-bit id, then a parameter in the stat's itemstatcost `CSvParam` bits (if it has any), then the value in its `CSvBits` bits. The id 0x1ff (511) ends the list, and the list is padded to a whole byte.

Values are stored as the game holds them, not as shown on screen. Life, mana and stamina keep their fractional part (shifted left by 8).

The `if` section has one byte of hard points per class skill, in the order of the class's skills by skill id. The count comes from header byte 0x1a, which is 30 for every retail class.

## Items

Items are bit-packed, least significant bit first, and each item is padded to a whole byte. The count after `JM` counts top-level items only. Socketed items follow their parent at location 6 and don't add to the count. They are only one level deep.

Every item starts like this:

| Field | Bits |
|-|-|
| flags | 32 |
| version | 3 |
| location | 3 |
| body slot | 4 |
| x | 4 |
| y | 4 |
| page | 3 |
| item code | 4 Huffman-coded characters, space padded |

Items on the ground use 16-bit x and y instead of the slot, grid and page fields. Flag 0x200000 marks a compact item, which stops after the code and a few type-specific fields. Gold uses a width bit, then a 12-bit amount (32 bits when the amount is above 0xfff), then a 1-bit flag. Ears carry class (3 bits), level (7) and a name of 8-bit characters instead of a code. An item can also carry a 128-bit realm id, as a presence bit and four 32-bit words.

Other items continue with:

| Field | Bits |
|-|-|
| sockets filled | 3 |
| seed | 32 |
| item level | 7 |
| quality | 4 |
| picture | 3, when its flag bit is set |
| auto affix | 11, when its flag bit is set |
| inferior or superior quality | 3 |
| magic prefix and suffix | 11 each |
| set or unique id | 12 |
| rare or crafted names | 8 each, two of them |
| rare or crafted affixes | up to six, 11 each |
| runeword | 16 (a 12-bit id and 4 more bits) |
| personalized name | 8-bit characters, terminated |
| tome | 5 |

Defense (stat 31), max durability (73) and durability (72) use their itemstatcost `Save Bits`, stored as value plus `Save Add`. Durability is only there when max durability is above zero. Stackable items have a 9-bit quantity. Socketed items (flag 0x800) store the socket count in stat 194's `Save Bits`, which is 4 in retail. Set items have a 5-bit list mask.

Item properties use the same scheme as character stats: a 9-bit id, `Save Param Bits`, then `Save Bits` holding value plus `Save Add`, ending with 0x1ff. Some damage stats come in groups, and the following members are written straight after the first one with no id of their own: 17/18, 48/49, 50/51, 52/53, 54/55/56 and 57/58/59. The base list, set bonus lists and runeword list are kept separate.

## What has been checked

Real saves from the game read and write back byte for byte. They held starter gear, a unique, a runeword with socketed runes, ethereal and socketed items, stacks, runes, scrolls and potions.

Not checked against real saves:

- The 11-bit auto affix width. No save with an auto affix has been read.
- Magic, rare and set items, ears, mercenary items and corpses. These are covered only by synthetic tests.
- Edited saves. A save changed this way read back correctly, but the game has not loaded one yet.
