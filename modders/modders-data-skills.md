# skills.txt

Fields in skills.txt that D2R uses differently from Classic, or that only exist in D2R.

## calc3 (Raven-AI summons)

### Table and field
skills.txt, column calc3, on summon skills whose pet uses the Raven AI.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
For a pet with the Raven AI, D2R takes the number of attacks the pet gets from the summoning skill's calc3. If calc3 is empty, the pet is killed on its first AI update, about 2 seconds after the cast.

Put the attack count formula in calc3. It can be a plain number or a formula that reads the skill's level parameters.

### Example
Not yet checked.

### Community notes
Classic 1.13c worked the count out from the AI parameters instead (Param1, plus Param2 for each level after the first), and a negative Param1 made the pet permanent. A Classic port with Raven-style pets will have those parameters set but calc3 empty, so the pets vanish almost at once. Move the formula into calc3.

## srvstopfunc and cltstopfunc

### Table and field
skills.txt, columns srvstopfunc and cltstopfunc.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
The only stop function is 1. D2R uses it on Whirlwind (srvdofunc 76, cltdofunc 45) and on Talic's Whirlwind (srvdofunc 168). It ends the spin and the skill's aura state when the player releases the skill or it gets interrupted.

Any new skill row that uses one of those do functions should get srvstopfunc 1 and cltstopfunc 1 too. Without them, the spin and its state do not end cleanly.

### Example
Not yet checked.

### Community notes
These columns do not exist in Classic 1.13c, so a Classic port has them empty on every row. Copy D2R's values onto any Whirlwind-type skill.

This is only lightly verified. The values come from D2R's own data, but what happens without them has not been tested much.

See also [Function numbers](/en/modders/function-numbers), [Calc formulas](/en/modders/calc-formulas) and [plrmode.txt](/en/modders/data/plrmode).
