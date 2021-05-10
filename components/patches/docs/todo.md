# A. Patches.

## Arcane spells.

### Level 1.

1. Charm Person: add Display String 1476 = Charmed.
2. Larloch's Minor Drain: use EE drain features of damage opcode for correct implementation. Damage opcode with normal parameter and magic damage type, instantaneous mode, duration 60, resist_dispel 3, special flags 8 + 64. Play sound opcodes to mark application (on target) and end of extra hps (on self). Since we have different mr on self and target, these have to be deferred to a subspell so we might as well use vanilla implementation.

### Level 3.

1. Vampiric Touch: see Larloch's Minor Drain.
2. Dire Charm: add Display String "Dire Charmed".
3. Slow has a Remove Spell Type Protections 12 = Combination. Is this correct?
4. Flame Arrow: like Melf's acid arrow, spell also does missile damage, so 324 opcode should be removed from spell and aux spells.
5. Protection from Missiles: all projectiles covered?

### Level 4.

1. Fire Shield does not seem to protect against insects. Should add protection against sectype k1#insect (or something similar)?
2. Greater Malison: uses 321 opcode to avoid stacking. The problem is that it is resist_dispel 0 vs. 1 of other opcodes. Change to protection from spell?
3. Protection from Fire: power of opcodes 5, 3 -> 4. Level 3 -> 4. Though being a level 4 spell, it is coded as a level 5 spell so have to fix durations of first header and add missing headers.
4. Protection from Cold: power of opcodes 5 -> 4. Level 3 -> 4. Same as Protection from Fire.
5. Protection from Electricity: power of opcodes 5 -> 4. Level 5 -> 4. Being coded as a level 5 spell, it misses two headers and the durations of the first header have to be fixed. Description level 4, not 5.
6. Protection from Acid: power of opcodes 5 -> 4. Level 5 -> 4. Same as Protection from Electricity.

## Divine spells.

### Level 1.

1. Sleep: immunities for half-elf and elf missing. Use 324 opcodes with race type and values 19 and 15 with appropriate probabilities.
2. Sunscorch: use EE features of damage opcode.
3. Regenerate Light Wounds: Only headers 7 and higher have Display String "Regenerating". Duration scales with levels contrary to other regeneration spells.

### Level 2.

1. Charm Person or Animal: add Display String 1476 = Charmed?
2. Regenerate Moderate Wounds: add display string "Regenerate" opcode?

### Level 3.

1. Holy Smite: use EE features of damage opcode.
2. Unholy Blight: use EE features of damage opcode.
3. Miscast Magic: cast spell opcode parameters?
4. Call Lightning: use EE features of damage opcode.
5. Glyph of Warding: use EE features of damage opcode.
6. Storm Shield: same problem with Protection from Missiles.
7. Summon Insects: uses Attack Nearest opcode instead of fear. Bug?

### Level 4.

1. Free Action: 240 opcodes are not spurious *if* Free Action removes the corresponding statuses.
