# String namespaces

D2RLoader can keep your mod's strings in a namespace of their own, so you don't ship copies of the game's string files or manage string ids. For how the game loads text without a namespace, see [Strings](/en/modders/strings).

Tested on D2R 3.3.0 (build 93847) with D2RLoader 1.3.1. Other versions have not been checked.

## Turning it on

Add a top-level `namespace` entry to your mod's `d2rloader/metadata.json`, next to `schemaVersion`:

```text
{ "schemaVersion": 1, "namespace": "mymod", ... }
```

Without it, your strings load the normal way. If you add or change the namespace later, rebuild your BINs. If you pack the mod into an MPQ, it needs a `(listfile)` so the loader can find string files with names of your choosing.

## Your string files

String files can have any name under `data/local/lng/strings/`. You don't copy the game's files, and you don't pick ids. A record needs only a `Key` and the languages you have:

```text
data/local/lng/strings/mymod/strings.json

[
  { "Key": "MySword", "enUS": "Blade of Testing", "deDE": "..." }
]
```

Every plain record in your files belongs to your namespace, so its full key is `mymod:MySword`. An `id` on such a record is ignored. This also applies to a plain record whose key the game already has: it becomes `mymod:<key>` and the game's own text stays as it was.

## Changing the game's text

To override a game string, prefix its key with `d2r:`. Only the languages you supply change; the rest keep the game's text.

```text
{ "Key": "d2r:strCancel", "enUS": "Nope" }
```

The key must exist in the game's `strings/` files. An override of a key the loader doesn't know stops it at startup. About 14 keys exist only in `strings-legacy/` and can't be overridden this way. In HD mode a `d2r:` record placed only in `strings-legacy/` was simply ignored.

Keep all overrides in one file and define each key once. A key defined twice in your mod stops the loader. Placeholder rules from the [Strings](/en/modders/strings) page still apply to overrides.

## Referring to strings in tables

In table cells (item `namestr`, monstats `NameStr`, levels `LevelName`, skilldesc and itemstatcost string columns, affix and unique names, and so on) an unqualified key always means the game's string. Your own strings must be written with the namespace:

```text
namestr
mymod:MySword
```

This includes numeric keys. If your namespace has a key `704`, a cell reading `704` means `d2r:704`, not yours. Write `mymod:704`. A cell naming a namespaced key that no file defines stops startup with a string reference error.

No string column was found where a namespaced key fails. Column width is the one limit (see below).

In HD layouts the loader's docs give `@Key` for your own strings and `@d2r:Key` for the game's. Layout references to namespaced strings have not been tried in game.

## Unique and set item references

Once a unique or set item's `index` cell reads `mymod:MyRing`, everything that names that item by index must use the same full form:

```text
cubemain input 1:      mymod:MyRing
cubemain output:       "mymod:MyRing,qty=2"
treasureclassex Item1: mymod:MyRing
```

With the bare name the reference matches nothing. A recipe input then matched any item and the wrong recipe ran, and a treasure class entry dropped nothing.

## Missing languages

The loader doesn't need all 13 languages, but there is no English fallback. A key with no text in the player's language shows "Missing string" where the text should be. Fill every language you care about, copying English into the others if you have no translation. This was only tested in an `enUS` game.

## String numbers

The game addresses strings by two-byte numbers. A namespaced key has no number of its own. When a table references it, the loader gives it a free number from the space the game's strings leave unused, in table load order.

These numbers are stable for the same data, but they shift as soon as the set of referenced keys changes. Never store them or write them into anything. Keys that no table references take no number at all and can still be looked up by key.

About 53,000 numbers are free. If the pool fills, or a table names a key that doesn't exist, the game stops at startup with an error saying a localization key is missing or the string reference pool is full.

## Columns that can't hold a namespaced key

Hireling `NameFirst` and `NameLast` are a range of string ids, not keys. They can't name namespaced strings, so they can only use the game's own names.

Some name fields, such as affix names and the `index` column of uniqueitems and setitems, store the key in a fixed-size field of about 32 bytes, and the whole `mymod:key` has to fit. A 36-character namespaced key in uniqueitems `index` loaded without an error, so the exact limit is not known. Keep these keys and your namespace name short.

## strings-legacy

With a namespace, the file in `strings/` was enough: your strings also showed in classic graphics, without a copy in `strings-legacy/`.

Whether the same `d2r:` record in both `strings/` and `strings-legacy/` counts as a duplicate is unclear. The loader treats both folders as one set of files and refuses duplicate keys, but in one HD test the game started with the same override in both folders and showed the `strings/` text. Keeping overrides in `strings/` only avoids the question.

## Not tested

- What classic graphics mode reads from `strings-legacy/`, in particular whether it shows your `d2r:` overrides.
- Languages other than `enUS`.
- `@Key` and `@d2r:Key` references in HD layouts.
- Namespaced text in an item tooltip or monster name bar (the chain behind them resolved, but the screens were not read).
- Whether a long key in a fixed-size name field is stored whole or cut.

See also [Your first mod](/en/modders/first-mod).
