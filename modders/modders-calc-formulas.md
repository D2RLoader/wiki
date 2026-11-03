# Calc formulas

Many columns in skills, missiles, skilldesc, monpet and the item tables take a formula instead of a number. This page covers how D2R reads those formulas and which codes you can use in them.

Tested on D2R 3.3.0 (build 93847). Other builds have not been checked.

## Functions and conditions

The function list is fixed: min, max, rand, skill, miss, stat, sklvl, sksrc and cond. sklvl takes three arguments, cond takes one or two, and the rest take two.

Named references go in single quotes with a code after a dot:

```text
skill('Fire Bolt'.blvl)
sklvl('Fire Bolt',10.blvl)
miss('Fire Bolt'.lvl)
```

skill and sksrc look up a skill and a skillcalc code; sksrc reads it from the source unit instead of the caster. sklvl does the same at the level you give it. miss looks up a missile and a misscalc code. stat reads an itemstatcost row, with mode 0 for the total, 1 for the base value and 2 for the bonus part.

cond tests one of these conditions:

| Condition | Arguments | Tests |
|-|-|-|
| IsType | 2 | The unit's type |
| IsClass | 2 | The unit's class |
| Desecrated | 1 | Whether the area is desecrated |
| MonsterTestElite | 2 | The monster's elite type |
| ItemIsType | 2 | The item's type |
| ItemIsModType | 2 | The item's modifier type |
| Difficulty | 2 | 0, 1 or 2 for Normal, Nightmare or Hell |
| IsDesecratedZonesEnabled | 1 | Whether desecrated zones are on |

There is no condition for "has state", so a formula can't branch on a state.

## Precedence and the ternary

From lowest to highest: comparisons, add and subtract, multiply and divide, power, negation, then the ternary. The ternary binds tighter than anything else, so this gives 4, not 3:

```text
1+2?3:4
```

Put brackets around the condition and each branch:

```text
(a)?(b):(c)
```

Both branches of a ternary are evaluated, whichever one is picked. A rand in the branch that isn't used still runs. This has not been tested at runtime.

## Mistakes the game won't report

An unknown skill, missile or stat name becomes 0 and the formula still loads. An unknown condition name is treated as IsType, and an unknown condition parameter becomes 0, so `cond('Difficulty','hel')` tests for Normal. Check your spelling; nothing tells you about these. A wrong number of arguments does fail.

## Limits

A formula can be 255 characters long, and a quoted name 32 bytes. Neither limit was probed in game.

## Which codes a formula can use

Skill and skilldesc formulas use the skillcalc codes, missile formulas use misscalc, and monster formulas use moncalc. Item and treasure class formulas take no codes; use stat() or skill() there.

The code tables are read by row number, so never reorder D2R's rows in skillcalc or misscalc. Add new codes after them. A new code still compiles, but the game answers it with 0 unless a plugin handles it.

In the lists below, L is the level. A linear pair gives A + (L-1) * B. A diminishing pair grows towards B with level, starting from A. The meanings of the newer codes, from mael in skillcalc and rad in misscalc, come from the table and haven't been checked in game.

### skillcalc

| Codes | Meaning |
|-|-|
| ln12, ln34, ln56, ln78 | Linear pairs of Param1-8 |
| dm12, dm34, dm56, dm78 | Diminishing pairs of Param1-8 |
| par1-par8 | Param1-8 |
| lvl | Skill level with bonuses |
| blvl | Skill level without bonuses |
| ulvl | Character level |
| edmn, edmx, edln | Elemental min, max and length |
| edns, edxs | Elemental min and max in 1/256ths |
| toht | To-hit |
| mana, mps | Mana cost, mana per second |
| usmc | Mana cost in 1/256ths |
| math, madm, macr | To-hit, damage and crit mastery |
| m1en, m1ex, m1el (also m2, m3) | Elemental min, max and length of skilldesc missile 1, 2, 3 |
| m1eo, m1ey, m2eo, m2ey, me3o, me3y | The same min and max in 1/256ths |
| m1rn, m2rn, m3rn | Range of those missiles |
| enma, exma, edma, enms, exms | Elemental min, max and length with mastery |
| pnma, pxma, pnms, pxms | Physical min and max |
| len | auralencalc |
| rng | aurarangecalc |
| clc1-clc4 | calc1-calc4 |
| ast1-ast6 | aurastatcalc1-6 |
| pst1-pst5 | passivecalc1-5 |
| pets, skpt | petmax, skpoints |
| mael, mapi, manc | Elemental, throwing pierce and throwing consumption mastery |
| par9, pa10, pa11, pa12 | Param9-12 |
| ln91, dm91, ln21, dm21 | Pairs of Param9-10 and Param11-12 |
| clc5, clc6, clc7, clc8, clc9, clc0 | calc5-calc10 |
| pst6-pst9, ps10-ps14 | passivecalc6-14 |
| mair | Item requirements mastery |
| sctk | Channelling tick |
| pa13-pa20 | Param13-20 |
| ln43, ln65, ln87, ln92 | Linear pairs of Param13-20 |
| dm43, dm65, dm87, dm92 | Diminishing pairs of Param13-20 |
| m1nm, m1xm, m2nm, m2xm, m3nm, m3xm | Skilldesc missile min and max with mastery |

### misscalc

| Codes | Meaning |
|-|-|
| par1-par5 | Server params 1-5 |
| cpa1-cpa5 | Client params 1-5 |
| hpa1-hpa3 | Server hit params |
| chp1-chp3 | Client hit params |
| dpa1, dpa2 | Damage function params |
| lvl | Missile level |
| edmn, edmx, edln | Elemental min, max and length |
| edns, edxs | Elemental min and max in 1/256ths |
| damn, damx | Physical min and max |
| dmns, dmxs | Physical min and max in 1/256ths |
| rang | Range |
| sl12, sd12, sl34, sd34 | Linear and diminishing server param pairs |
| cl12, cd12, cl34, cd34 | Linear and diminishing client param pairs |
| shl1, shd1 | Server hit param pair |
| chl1, chd1 | Client hit param pair |
| dl12, dd12 | Damage param pair |
| ccl1, scl1 | Client calc1, server calc1 |
| rad | Radius |
| fnum | Current missile frame |

Range is a formula in D2R. Classic's LevRange column was merged into it, so write the level part into Range yourself.

### moncalc

| Codes | Meaning |
|-|-|
| hpct | Health percentage |
| diff | Difficulty, counted from 1 |
| lvl | Monster level |
| elte | Elite type |
| pcl1-pcl5 | The monpet calcs |

diff counts from 1, while the Difficulty condition counts from 0.

For function columns in the same rows, see [Function numbers](/en/modders/function-numbers). Skill columns are described on [skills](/en/modders/data/skills).
