# Patches.

## Arcane spells.

### Level 1.

1. Grease: delete spurious 324 opcode.
2. Monster Summoning I: corrected power 6 -> 0 of summoning effect.
3. Protection from Petrification: display string "Gaze Reflection" -> "Protection from Petrification".
4. True Strike: opcodes' target preset (2) -> self (1). Spurious 321 and 206 opcodes removed.
5. Shield: play sound resist_dispel 1 -> 3. Added protection against TRAP_MAGIC_MISSILE. Changed spell from non stacking to EE refresh. Remove protection against wand03 as spell does not even exist.

note(s):
* wand03 does not exist in vanilla but IR may introduce it. In vanilla, wand03 item sets up protection by toggling state WIZARD_SHIELD.

6. Sleep: changed implementation to use EE features of sleep code. This meant the deletion of the cast spell on condition and display portrait icon opcodes.
7. Chill Touch: create item opcode amount 0 -> 1.

## Divine spells.

### Level 1.

1. Command: play sound opcode lacks save vs. spell and respective save penalty. Resist_dispel of first play visual opcode to dispel/bypass mr. Use EE features on sleep opcode.
2. Cure Light Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
3. Entangle: duration of opcodes 8 -> 7 to conform with similar cloud spells. See Kjeron's comments in the thread [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions).
4. Sanctuary: play sound opcode duration 60 -> 36.
5. Shilellagh: create item opcode amount 0 -> 1.
6. Good Berry: power of opcodes 2 -> 1.
7. Cause Light Wounds: power of opcodes 4 -> 1.
8. Sunscorch: change documentation to explicitly mention that a save vs. spell (not vs. breath, as rolled to halve the damage) will avoid blindness.
9. Regenerate Light Wounds: resist_dispel -> 3.