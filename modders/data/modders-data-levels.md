# levels.txt

Two levels.txt fields that can close the game or corrupt map memory when a mod adds new levels.

## Position

### Table and field
levels.txt, column Position.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
With Position set to 1, the game picks a random entry room from the maze's preset list when a player arrives. That only works if the maze has entry rooms. A new maze level that D2R does not ship has none, so Position 1 on it closes the game as soon as a player enters, with no crash report.

Leave Position empty on new maze levels (DrlgType 1). The player then arrives at the warp tile instead.

### Example
Not yet checked.

### Community notes
The game just closes, with no error and no report, so this one is easy to blame on something else. If a new maze closes the game on entry, check Position first.

## Layer

### Table and field
levels.txt, column Layer.

### Source
Community research notes.

### Verified versions
Not yet checked.

### Values and limits
Keep Layer below 100. Each character's map memory (the part of the automap you have already uncovered) is stored in a table with 100 slots, and Layer is used as the slot number directly. A Layer of 100 or more reads and writes past the end of that table.

If you really need more layers, the only way around it is a plugin that keeps its own map memory for the higher layers.

### Example
Not yet checked.

### Community notes
This limit comes from reading how the map memory is stored. Nobody has actually seen a crash from a high Layer yet, so treat it as a likely problem rather than a confirmed one. Since the result would be memory corruption, the symptoms could be random and show up far from the cause.

See also [automap.txt](/en/modders/data/automap).
