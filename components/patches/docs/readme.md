# Patches.

## Arcane spells.

### Level 1.

1. Grease: delete spurious 324 opcode.
2. Monster Summoning I: corrected power 6 -> 0 of summoning effect.
3. Protection from Petrification: display string "Gaze Reflection" -> "Protection from Petrification".
4. True Strike: opcodes' target preset (2) -> self (1). Spurious 321 and 206 opcodes removed.
5. Shield: play sound resist_dispel 1 -> 3. Added protection against TRAP_MAGIC_MISSILE. Changed spell from non stacking to EE refresh. Remove useless protection against wand03 as spell does not even exist.

note(s):
* wand03 item sets up protection by setting state WIZARD_SHIELD.

6. Sleep: changed implementation to use EE features of sleep code. This meant the deletion of the cast spell on condition and display portrait icon opcodes.
7. Chill Touch: create item opcode amount 0 -> 1.