# Function numbers

Skills, missiles, items and cube recipes pick engine code by number. This page lists which numbers D2R leaves free and which Classic numbers now mean something else.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

## Free slots

These numbers have no function behind them in D2R. A free number does nothing on its own: a plugin has to supply the function before a row can use it. Any number D2R already uses runs D2R's code, whatever your mod meant by it.

| Column | D2R uses | Free |
|-|-|-|
| skills srvdofunc | up to 171 | 172-190 |
| skills cltdofunc | up to 109 | 110-129 |
| missiles pSrvHitFunc | 0-60 | 61-70 |
| missiles pCltHitFunc | up to 65 | 66-80, plus 6 and 7 |
| missiles pSrvDoFunc | 1-45 | 46-54 |
| misc pSpell | -1 to 15 | 16-19 |

For pSrvDoFunc, 46-54 are empty, but where that table ends has not been confirmed, so don't go past 54. For pSpell, 20 and up does nothing: right-clicking the item has no effect.

Before writing new code, check whether D2R already has a function that does the job with different table values. A timed potion is pSpell 3, and an itemless town portal is srvdofunc 109.

## Stop functions

srvstopfunc and cltstopfunc have one value, 1. Whirlwind and Talic's Whirlwind use it. A new skill built on a Whirlwind do function needs stop function 1 in both columns, or the spin doesn't end cleanly when the player releases the key or gets interrupted. See [skills](/en/modders/data/skills).

## Cube recipe ops

In cubemain, ops 1-14 are checks on the player, made when the game looks for a matching recipe (1 and 2 are dates, 3-14 compare player stats). Ops from 15 up are checked against each input item. D2R's own cubemain uses only op 28.

## If you're porting a Classic mod

D2R reused several numbers that were free or meant something else in 1.13c. A row left on its old number runs whatever D2R put there.

| Column | Classic number | What it is in D2R | What to use |
|-|-|-|-|
| srvdofunc | 109 | Itemless Town Portal | 9 (1.13c's monster frenzy was merged into Frenzy) |
| srvstfunc | 64 | Bind Demon's start | 67 |
| cltstfunc | 82 | Past the end of the table | Leave it blank |
| cltdofunc | 22 | Blank | A free client slot (110-129) through a plugin |
| pCltDoFunc | 11 | Kept for other missiles; corpse explosion moved out | 69 for corpse explosion |

A Classic mod's own srvdofunc 153-171 are D2R 3.x skills now, so move them to 172-190. Its cltdofunc 96-99 run D2R's Volcano, Sigil and Ring of Fire client code, so move them to 110-129.

Formulas in these rows are covered on [Calc formulas](/en/modders/calc-formulas). If you're new to D2R modding, start with [your first mod](/en/modders/first-mod).
