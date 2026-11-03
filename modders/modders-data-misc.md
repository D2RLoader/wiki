# misc.txt

A signed column in misc.txt that is easy to get wrong in decoded or ported data.

## spellicon

### Table and field
misc.txt, column spellicon.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
spellicon is a signed byte. -1 means the item has no spell icon. If -1 ends up as 255, the item shows a spell cursor that was never meant to be there.

### Example
Not yet checked.

### Community notes
Seen in Classic ports built from decoded data, where -1 came out as 255. Compare against D2R's own rows for items both games share to catch it. The same kind of slip hits coldeffect in monstats.txt.

See also [monstats.txt](/en/modders/data/monstats).
