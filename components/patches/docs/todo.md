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

### Level 5.

1. Cone of Cold: use EE damage opcode features.
2. Fireburst: use EE damage opcode features on aux spell.
3. Mestil's Acid Shield: same problem as Fire Shield.
4. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not have it.

### Level 6.

1. Flesh to Stone: Main spell: the second remove spell type opcode for k1#haste has inconsistent resist_dispel, timing mode limited and a duration. In the aux spell all opcodes have permanent timing, but with a duration 12, which is harmless, but technically incorrect. Contrary to what is explicitly stated in the docs, all opcodes have resist_dispel 1. The limited duration effects still have resist_dispel 1 => still dispellable and do not bypass mr. This includes the delayed cast opcode spell, which implies that the impending petrification can be prevented by a dispel. I assume this is what the developers intended.
2. Chain Lightning: use EE damage opcode features.
3. Disintegrate: use EE damage opcode features.

### Level 7.

1. Ruby Ray of Reversal and Khelben's Warding Whip: power of opcodes -> 0.
2. Prismatic Mantle: it has no protection from weapons opcode and yet the Modify Prof opcode seems to indicate otherwise. More generally, the description seems to be a complete mess. It is misleading; blindness always applies to creatures less than 8HD, etc.
3. Delayed Blast Fireball: use EE damage opcode features.
4. Several spells miss or have too many headers (and therefore have wrong durations in first header): Protection from Elements, Summon Nishruu, Control Undead. There may be other examples.
5. Khelben's warding whip: change to delayed cast subspell 4 times.
6. Spell Thrust (level 3), Secret Word (level 4), Pierce Magic (level 6), Ruby Ray of Reversal (level 7), Khelben's Warding Whip (level 7) are advertized as removing Dispelling Screen, but this is not true. For to remove it, they would have to, for example, have an opcode to remove spell by sectype for k1#dispel which they do not have, nor anything similar -- k1#dispel is set by k1#scre spell, an aux spell of Dispelling Screen that is *not* a spell protection. This is also hinted at the wording of Dispelling Screen, that only high-powered spell protection removal spells bring down dispelling screen. Furthermore, Pierce Shield *explicitly* dispels k1#dispel in the last opcode in the first spell it casts.

### Level 8.

1. Mind Blank: protects against power word stun but not stun.
2. Pierce Shield: all but 1 opcode in the first aux spell have power 0. There is an inconsistency here as spell thrust, secret word and pierce magic have the same 0 power while RRoR and Khelben's Warding Whip have power 7.
3. Moment of Prescience: mention that is not dispellable.
4. Incendiary Cloud: use EE damage opcode features.
5. Horrid Wilting: use EE damage opcode features.
6. Power Word Blind: Protection from obscuring mist and power word blind lack the minimum level. Only the 1 turn version of protection from spells exist, with the exception of Power Word Blind.
7. Bigby's Icy Grasp: in aux spell, opcodes hold and play visual have wrong saves (-4 and 0) -> -2. The resist_dispel in setting up hold are also inconsistent: put all at 1, but this is something of a guess. The play visual and damage opcodes are applied before setting up immunities, is this correct? At any rate, description should also mention that a whole bunch of critters are immune to the hold effect.

### Level 9.

1. Wail of the Banshee: should the slay opcode change position with lighting effects?
2. Spellstrike: power level of opcodes 0?
3. Meteor Swarm: use EE damage opcode features.
4. Imprisonment: kills the victim after 5 rounds. The killing should be advertized in the docs.

## Divine spells.

### Level 1.

1. Sunscorch: use EE features of damage opcode.
2. Regenerate Light Wounds: Duration scales with levels contrary to other regeneration spells.
3. Resist Fear: sets protection from non-existent spell "mummydsp".
4. Strength of Stone: misses protection from druid version of Gust of Wind.

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

1. Free Action: 240 opcodes are not spurious *if* Free Action removes the corresponding icon opcodes.

### Level 5.

1. True Seeing: since resist_dispel on casting opcodes 3 -> 2 should resist_dispel of opcodes in subspell -> 3? The protection from spell is against the IR cloak's reflected image, shouldn't it be remove resource opcode instead?
2. Insect Plague: same issue with Summon Insects.
3. Flame Strike: use EE features of damage opcode.
4. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not have it.

### Level 6.

1. False Dawn: all but the first cast spell opcodes have magic resist 3. But these are delayed cast spell opcodes so it is at least plausible that they be dispellable, even though the subspell opcodes are themselves dispellable.

### Level 7.

1. Fire Storm: use EE features of damage opcode.
2. Sunray: use EE features of damage opcode.
3. Creeping Doom: in the aux spell that actually implements the insect swarm effects all opcodes have power 0. Same problem as other insect spells.