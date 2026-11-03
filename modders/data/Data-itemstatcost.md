# itemstatcost.txt

How Save Param Bits interacts with properties that put a parameter on a stat.

## Save Param Bits

### Table and field
itemstatcost.txt, column Save Param Bits.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
Save Param Bits is how many bits an item stores for the stat's parameter. Save Bits and Save Add cover the value itself.

Properties that use function 28 ("layer = param") put their parameter into the stat's layer. If the stat has no Save Param Bits, D2R cannot write the item at all, and the client crashes the moment such an item drops.

There are two fixes. If only the value matters, clear the parameter on the function-28 property. Otherwise give the stat some Save Param Bits.

Changing any of the save widths (Save Bits, Save Add, Save Param Bits, and CSvBits/CSvParam for the character save) changes the save format. Existing items and characters will no longer read correctly, so settle these before players have saves.

### Example
Not yet checked.

### Community notes
Classic 1.13c quietly dropped a layer it had no bits for, so a Classic mod can carry this setup without anyone noticing. In D2R it crashes. When porting, look at every property row that uses function 28 and check the stat it points at.

The crash happens on drop, not at load. The game starts fine and the problem only shows up once the item actually rolls.

See also [properties.txt](/en/modders/data/properties) and [Save format (v105)](/en/modders/save-format).
