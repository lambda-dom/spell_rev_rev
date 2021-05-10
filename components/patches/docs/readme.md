# A. Patches.

In this section we document the patches for the user's convenience. If there is some patch you think should not be applied, just edit the patches.2da file and turn the value in the patch column from 1 to 0.

Do note that the ultimate authority is the code, which is even commented in reasonable detail; there is always the chance that this doc is not 100% synched with the code, so bear that in mind (if that bothers you, patches are welcomed).

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

### Level 2.

1. Blur: power of opcodes 3 -> 2.
2. Detect Invisibility: opcode Invisibility Detection resistance_dispel 2 -> 3.
3. Battering Ram: Sleep opcode gained special = 14 for icon and strref.
4. Know Opponent: fix casting time 9 -> 2 to synch with cleric's version. Description fix.
5. Luck: detect illusion resistance_dispel 0 -> 3.
6. Resist Fear: harmless, but reset morale duration 40 -> 0.
7. Melf's Acid Arrow: remove 324 opcode as with it, missile damage is also blocked by acid immunity.
8. Mirror Image: change non-stacking to EE refreshing and added protection against Reflected Image.
9. Monster Summoning II: corrected power of summoning effect.
10. Ghoul Touch: create item opcode amount 0 -> 1.
11. Vocalize: play visual effect opcode power 1 -> 2.
12. Power Word Sleep: Sleep opcode gained EE special = 14 for icon and strref.
13. Ray of Enfeeblement: both the first play sound and character pulse opcodes lack save.
14. Chaos Shield: play visual and second protection from spell have target preset (2) instead of self (1).
15. Sound Burst: use EE damage opcode features on aux spell.

### Level 3.

1. Clairvoyance: save vs. breath bonus power 0 -> 2.
2. Dispel Magic: timing of Remove spell type protections opcode 3 -> 1. Power of Play Visual 3 -> 0 for consistency with other opcodes.
3. Fireball: use EE damage opcode features.
4. Hold Person: added display string "Held" opcode.
5. Lightning Bolt: use EE damage opcode features.
6. Monster Summoning III: summoning eff power 6 -> 0.
7. Non-detection: protection from spell opcodes power 0 -> 3.
8. Slow: The second Remove Spell Type Protections has a spurious save bonus. The first play sound opcode has no save and timing mode 0 -> 1.
9. Skull Trap: use EE damage opcode features.
10. Wraitform: dispel_resistance 1 -> 3. It also blocks innate abilities, but this is undocumented so it is removed. Display String opcode power 0 -> 3.
11. Minor Spell Deflection: spurious repeated modify proficiency and set spell state opcodes. The first spell deflection spell opcode also has resource a subspell that is supposed to remove spell deflection effects, but it is spurious. The latter is added and the former changed.
12. Spell Thrust: made trigger radius equal to explosion. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.

### Level 4.

1. Ice Sorm: fix duration of movement rate penalty 24 -> 7. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.
2. Minor Globe of Invulnerability: remove double fireball protection. Change description to mention correct duration of 2 turns.
3. Contagion: Opcode's power 3 -> 4. Casting spell opcodes cast non-existent "sppr311d" -- clone aux spell from cleric version and patch it in.
4. Break Enchantment: add remove charmed icon opcodes. Clarified description to mention it removes petrification.
5. Greater Malison: Change description to mention correct duration of 2 turns.
6. Teleport Field: mention in description it bypasses mr.
7. Farsight: clairvoyance opcode power 3 -> 4. Clairvoyance and farsight opcodes resist/dispel -> 2.
8. Monster Summoning IV: summoning eff power 8 -> 0.

### Level 5.

1. Shadow Door: opcode 215 resist_dispel 0 -> 3.
2. Monster Summoning V: summoning eff power 6 -> 0.
3. Hold Monster: lacks portrait icon and display string opcodes.
4. Conjure Lesser Fire Elemental: The first play visual effect opcode (that should play on a successful summon) has the wrong probability 86 - 100 -> 0 - 85.
5. Phantom Blade: create weapon opcode amount 0 -> 1.
6. Conjure Lesser Air Elemental: same problem as Conjure Lesser Fire Elemental.
7. Conjure Lesser Earth Elemental: same problem as Conjure Lesser Fire Elemental.

### Level 6.

1. Globe of Invulnerability: fix description that duration is 2 turns. Missing spell protections: Ice Storm.
2. Tenser's Transformation: play visual effect resist/dispel 2 -> 3.
3. Banishment: Use Eff against non-existent row in gender.ids removed from spell and aux spell.
4. Protection from Magic Energy: contrary to what is stated, no creatures are immune to this spell; fixed description.
5. Pierce Magic: duration of display portrait and play sound opcodes.
6. True Seeing: fixes as in the cleric version.
7. Improved Haste: repeated protection against slow removed and protection against non-existent spwi545 -> changed to spwm164.
8. Contingency: three headers why? Removed. Last cast spell opcode for what? It does nothing. Removed.
9. Conjure Fire Elemental: same problem as Conjure Lesser Fire Elemental, with wrong probabilities in first visual opcode.
10. Conjure Air Elemental: see Conjure Fire Elemental.
11. Conjure Earth Elemental: see Conjure Fire Elemental.
12. Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2.
13. Monster Summoning VI: summoning eff power -> 0.

### Level 7.

1. Chaos: Opcodes have save penalty -6 instead of -4 and hold opcode has no save. Add display icon for Hold, Sleep and Berserk effects, and string for Hold. Misses immunity by chaotic commands.
2. Finger of Death: damage is 2d8 + level not 6d8 as advertized. Here I am going by the implementation instead of the docs.
3. Power Word Stun: has an undocumented one-round stun with a save -2.

## Divine spells.

### Level 1.

1. Command: play sound opcode lacks save vs. spell and respective save penalty. Resist_dispel of first play visual opcode to dispel/bypass mr. Use EE features on sleep opcode.
2. Cure Light Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
3. Entangle: duration of opcodes 8 -> 7 to conform with similar cloud spells. See Kjeron's comments in the thread [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions).
4. Sanctuary: play sound opcode duration 60 -> 36.
5. Shilellagh: create item opcode amount 0 -> 1.
6. Good Berry: power of opcodes 2 -> 1.
7. Cause Light Wounds: power of opcodes 4 -> 1. This implies it *does* not bypass MGoI.
8. Sunscorch: change documentation to explicitly mention that a save vs. spell (not vs. breath, as rolled to halve the damage) will avoid blindness.
9. Regenerate Light Wounds: resist_dispel -> 3.

### Level 2.

1. Chant: Added 321 opcode to prevent stacking saves and removed spurious 321 opcode from aux spell.
2. Charm Person or Animal: the second 324 opcode is spurious.
3. Find Traps: auxiliary spell duration 20 -> 6 and target 2 (preset) -> 1 (self).
4. Flame Blade: create item opcode amount 0 -> 1.
5. Hold Person: play sound opcode has a save bonus of -1. Added display string 14102 = "Held" opcode. Remove spurious 321 opcodes targeting know opponent.
6. Know Opponent: Description changed casting speed 1 round -> 2.
7. Slow Poison: add remove intoxicated icon opcode since the spell also cures intoxication. Add remove poisoned icon opcode.
8. Spiritual Hammer: create item opcode amount 0 -> 1.
9. Cure Moderate Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
10. Fire Trap: changed implementation to use EE features, namely the save for half flag.
11. Regenerate Moderate Wounds: power of opcodes 1 -> 2, resist_dispel 0 -> 3.
12. Gust of Wind: sleep opcode resist_dispel 0 -> 1. Add to description that it dispels insect swarms. Sleep opcode gained special = 14 for icon and strref.
13. Cause Moderate Wounds: power 0 -> 2.
14. Animal Summoning II: summoning eff power 8 -> 0.

### Level 3.

1. Break Enchantment: add remove charm icon opcodes. Add to description that it also removes petrification.
2. Remove Paralysis: delete spurious prevent portrait opcode.
3. Holy Smite: first header has a spurious Use Eff mask_evil opcode. Holy Smite no longer targets only evils.
4. Unholy Blight: first header has a spurious Use Eff mask_good opcode. Unholy Blight no longer targets only goody two-shoes. Base thac0 penalty, Lighting Effects and Play Sound opcodes lack saves. All the save penalty opcodes have an undocumented penalty -2. I am assuming this is an error and fixed it, all the more because Holy Smite does not have this penalty for its blindness effect.
5. Cure Serious Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
6. Gust of Wind: sleep opcode resist_dispel 0 -> 1. 

note(s):
* contrary to the druid version, it does *not* dispel insect swarms. It also has a *documented* penalty of -2 to the saves.

7. Summon Insects: panic on failed save vs. poison is not referred to in the description. Added it to description.
8. Animal Summoning III: summoning eff power 8 -> 0.
9. Contagion: aux spell had 5th disease opcode (slow) probability1 0. The opcode is undocumented, so it is deleted. Remove spurious protection against chaotic commands.
10. Cause Serious Wounds: opcode's power 4 -> 3.
11. Regenerate Serious Wounds: opcode's power 1 -> 3.

### Level 4.

1. Cure Critical Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
2. Animal Summoning IV: summoning eff power 8 -> 0.
3. Free Action: first header repeated protection against spell spwm164, but missing protection against web so altering the resource in the opcode. Second and subsequent headers miss protection against spells wish slow, spin983 slow and web. All headers missing protection against strings 14650 = "Paralyzed" and 14668 = "Slowed". Remove spurious 240 opcodes.
4. Neutralize Poison: misses protection against string 14662 = "Poisoned". Add remove poisoned icon opcode.
5. Domination: misses opcode display string 8364 = "Dominated"
6. Farsight: errors in several opcode's power and resist_dispel.
7. Ice Sorm: fix duration of movement rate penalty 24 -> 7. Damage opcode has an undocumented +2 damage.
8. Regenerate Critical Wounds: power of opcodes 1 -> 4.

### Level 5.

1. Animal Summoning V: summoning eff power 8 -> 0.
2. Raise Dead: power of Use Eff opcode 4 -> 5.
3. True Seeing: immunity to blindness lacks the appropriate protection from icon and string opcodes. resist_dispel on the opcodes is also inconsistent: some are not dispellable, others like the delayed cast spell and aesthetic opcodes are, which makes no sense. Lacks protection from cleric's obscuring mist.
4. Chaotic Commands: first header lacks 8 opcodes that the other headers have. Several protection against string opcodes missing.
5. Greater Command: play visual effect power 1 -> 5. Play sound lacks a save and has wrong duration. Delayed cast spell opcode has wrong duration 9 -> 6. It also casts sppr512b instead of the correct sppr512a (one less round).
6. Mass Cure: add remove intoxicated icon opcode since the spell also cures intoxication.
7. Repulsion: aux spell sppr515d play visual opcode 215 power 2 -> 5.
8. Insect Plague: mention that on a failed save vs. poison the victim panics for the round.
9. Mass Regeneration: power of opcodes 0 -> 5.
10. Animal growth: Use EFF opcode calls wrong eff. Aux spell dvpr525d has undocumented strength and constitution bonuses opcodes, with wrong duration to boot. Deleted. If the strength bonuses are to be kept then they must be documented; the constitution bonus does not make much sense though, because the hps are already being raised.

### Level 6.

1. Animal Summoning VI: summoning eff power 8 -> 0.
2. Heal: add missing remove poisoned icon opcode.
3. Dolorous Decay: play visual effect has a save penalty -5 -> 4. Damage amount of disease was 2 -> 1. Second display icon has timing mode 4. It seems this should be timing 1. Mention it bypasses mr.
4. Physical Mirror: power of opcodes in aux spell sppr613d 1 -> 6.
5. Regeneration: the description explicitly mentions the possibility of dispellability but opcodes have resist_dispel 0 -> 3. Description fix that duration is 3 turns.
6. Banishment: same problems as wizard's Banishment.
7. Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2, power 0 -> 6.
