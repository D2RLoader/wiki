# plrmode.txt

The mode codes in plrmode.txt (and monmode.txt) are looked up by skills, so they must match D2R.

## Code

### Table and field
plrmode.txt, column Code. monmode.txt has the same column and the same rule.

### Source
Community research notes.

### Verified versions
D2R 3.3.0 (build 93847). D2RLoader version not recorded.

### Values and limits
Keep D2R's Code column exactly as it is. The two that matter are Sequence, which is SQ, and Knockback, which is KB.

The Token column says GH for both of those, the same as Get Hit. That is fine. The game looks modes up by Code, not Token.

Skills refer to Sequence as SQ in their anim and seqtrans columns. If Code says GH instead, SQ finds no mode and the client crashes the first time someone uses Whirlwind or another sequence skill. monmode.txt works the same way for monsters.

### Example
Not yet checked.

### Community notes
This is a porting trap. Classic 1.13c spells the Sequence mode GH, so a Classic plrmode.txt or monmode.txt, or a decoded copy of one, has GH where D2R has SQ and KB. Do not copy those rows over D2R's.

The skills side has the same cause. Skills whose anim or seqtrans says GH play the knockback frames and never actually start. Change them to SQ.

See also [skills.txt](/en/modders/data/skills).
