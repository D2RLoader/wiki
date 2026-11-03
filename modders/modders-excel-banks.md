# excel vs excel\base

Since 3.0 D2R keeps two sets of tables, one per game mode. This page covers how they differ and how to add rows without breaking either one.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

## Two banks

Reign of the Warlock reads its tables from `excel/`. Classic and Lord of Destruction read theirs from `excel/base/`. In build 93847 that is 184 tables in `excel/` and 177 in `excel/base/`.

```text
data/global/excel/        Reign of the Warlock
data/global/excel/base/   Classic and Lord of Destruction
```

There is no fallback between them. Each mode reads only its own folder, so a table you change in one bank is unchanged in the other. Which bank a given character loads was not confirmed in game. Until you know, write both banks.

The one exception is `sounds` and `soundenviron`. Every mode reads them from `excel/`.

## Keep ids the same in both banks

Row ids must match across the two banks. A character upgraded from Lord of Destruction to Reign of the Warlock keeps its row numbers.

The banks don't have the same row counts. In build 93847 skills has 429 rows in `excel/` and 427 in `excel/base/`, and missiles, skilldesc, levels and overlay differ too. Start your new rows after the larger bank's count, so both banks use the same ids.

## How rows are counted

Every row counts except rows whose first cell is `Expansion`. A row with a blank key is still a record. monsounds, montype and the affix tables start with one, and skipping it shifts every row after it by one. The game's own `*ID`-style comment columns follow the same count, so you can check against them.

## Indexed tables are append-only

In indexed tables the row number is the id. That includes skills, states, itemstatcost, missiles, monstats, monstats2, overlay, properties, levels and objects. The game's code refers to its own added rows by number, so never reorder or delete them. Add new rows after the game's last row.

To reuse a game row, edit it in place and keep its key unless nothing refers to it. If ids do move, every reference to them has to be remapped. A wrong reference gives the wrong skill or monster, not an error.

skilldesc is an exception. It was sorted alphabetically and is referenced by name only.

## Item types are tested by number

itemtypes is looked up by code, but the game's code also checks some types by row number. Keep the game's rows at the game's numbers.

- 58 jewl
- 68 h2h
- 73 pelt
- 75 rune
- 77 hpot
- 81 apot, 82 wpot
- 83 scha, 84 mcha
- 96 gem4

If these move, the checks hit the wrong item types.

## Rows that are saved

uniqueitems, setitems and runes are keyed tables, so row order mostly doesn't matter. But their row numbers are stored in saves, so keep existing rows where they are. See [Save format](/en/modders/save-format).

## If you're porting a Classic mod

A Classic mod has a single `excel/` folder. Write its tables into both banks with the same row ids.

D2R added rows a Classic mod doesn't have, such as the Warlock item type `warl` at row 67. A merge that puts the mod's rows first shifts every id after it, which is how the item type checks above end up on the wrong rows. Put each D2R-only row at its D2R number, number the mod's rows around it, then remap the references.

See also [Strings](/en/modders/strings) and [Your first mod](/en/modders/first-mod).
