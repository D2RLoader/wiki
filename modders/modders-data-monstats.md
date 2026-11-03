# monstats.txt

A signed column in monstats.txt that is easy to get wrong in decoded or ported data.

## coldeffect

### Table and field
monstats.txt, column coldeffect, plus its Nightmare and Hell columns.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
coldeffect is a signed byte: how much a chill changes the monster's speed, in percent. -20 means chilled monsters are 20% slower. Write negative values as negative numbers.

If the value ends up unsigned, -20 becomes 236 and chilled monsters speed up by 236%.

### Example
Not yet checked.

### Community notes
This shows up in Classic ports built from decoded data, where the decoder printed the signed byte as unsigned. spawnx and spawny in the same table are signed too (-2 came out as 254).

A quick way to find these slips is to compare every numeric column against D2R's own rows for monsters both games share. Values from 128 to 255 are not always wrong, though. Sizes and coordinates can legitimately be that big.

See also [misc.txt](/en/modders/data/misc).
