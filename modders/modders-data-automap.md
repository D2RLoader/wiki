# automap.txt

The automap cel columns have a hard upper limit in D2R.

## Cel1-Cel4

### Table and field
automap.txt, columns Cel1, Cel2, Cel3 and Cel4.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
The highest valid cel is 1498. D2R's automap image has 1,499 cels, and D2R's own rows stop at 1498.

A higher value does not fail at load. The cel gets stored in the character's map memory, and the next time the game saves that memory it loops forever. In practice the client hangs after a few area changes, and the log keeps repeating a line about reading the character's map file.

Clamp any cel above 1498 to -1 (no cel).

### Example
Not yet checked.

### Community notes
This mostly bites Classic ports, whose automap.txt can point at cels that D2R's image does not have. The hang only shows up once a player has uncovered the affected tiles and changed area a few times, so a quick test in town will not catch it. Check every Cel column after a port.

See also [levels.txt](/en/modders/data/levels).
