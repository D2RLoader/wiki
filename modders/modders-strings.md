# Strings

How D2R loads a mod's text, and how to add your own with plain string files and numeric ids.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

## Where strings live

Text is in JSON files under two folders. `strings/` is used by HD, and `strings-legacy/` holds the legacy mode text, which is a subset of the same files.

```text
data/local/lng/strings/*.json
data/local/lng/strings-legacy/*.json
```

Each file is an array of records with an `id`, a `Key` and one field per language (13 in total, `enUS` through `zhCN`). Tables point at strings by `Key`, for example items `namestr` or levels `LevelName`. HD layouts use `@Key`. Numeric keys such as `"115"` are valid keys.

## A string file replaces the whole file

If your mod ships a string file, the game uses it instead of its own file with the same name. Anything you leave out is gone, and a missing record shows its key in game. So ship a complete copy of every file you touch: the game's records, your changes and your new records. Do this in both `strings/` and `strings-legacy/` wherever the game has that file.

If you change the text of an existing key, change it everywhere it appears (some keys are in more than one file), and in every language. Filling all 13 languages stops other clients from showing the original text next to your changes.

## String ids

Ids are shared across all files and must be unique. D2R matches string ids on their low 16 bits only, so two ids that differ only above 65535 collide.

Start new ids at the game's next free id (28106 in build 93847, listed in `next_string_id.txt`) and count up. Skip every value whose low 16 bits a game string already uses, and stay below 65536. Use the same id for a key in both `strings/` and `strings-legacy/`. The game already ships some duplicate ids, so don't add more.

```text
data/local/lng/next_string_id.txt
```

## Keep the placeholders

The game fills some strings itself with printf-style formatting, such as tooltip lines and the death message pieces `sysmsg7` to `sysmsg9`. When you override one of these keys, keep the same placeholders (`%d`, `%s`, `%+d`, `%%`) in the same order. A different number of `%s` reads an argument that isn't there and crashes the game. Keys the engine fills in need their placeholders even if your text doesn't use them.

## Using a namespace instead

D2RLoader can put your strings in a namespace, so you don't ship copies of the game's files or pick ids. See [String namespaces](/en/modders/strings/namespaces).

## If you're porting a Classic mod

Use a [namespace](/en/modders/strings/namespaces) for the mod's own keys, and put new text for keys the game already has into one override file. Don't append the old strings with numeric ids, because their low 16 bits collide with the game's strings.

Check the placeholders of every key you override. Classic text for `sysmsg7` to `sysmsg9` with a different `%s` count crashes the client when a player dies.

## Not tested

What classic graphics mode reads from `strings-legacy/` has not been checked.

See also [Your first mod](/en/modders/first-mod) and [excel vs excel\base](/en/modders/excel-banks).
