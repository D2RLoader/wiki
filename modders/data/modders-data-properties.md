# properties.txt

One properties.txt function needs a matching setting in itemstatcost.txt.

## func (function 28)

### Table and field
properties.txt, columns func1 to func7, value 28.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
Function 28 sets the stat's layer to the property's parameter. The stat it points at needs Save Param Bits in itemstatcost.txt, or the client crashes when the item drops. The details and both fixes are on the [itemstatcost.txt](/en/modders/data/itemstatcost) page, under Save Param Bits.

### Example
Not yet checked.

### Community notes
Mostly a problem for Classic 1.13c ports, where this setup used to be harmless.
