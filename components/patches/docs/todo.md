# A. Patches.

General points:

1. In summoning spells, the Use Eff has the required duration but the eff itself is usually something like 2400 and may even have timing = 1. This is harmless, but synch up the durations?
2. Off by one probability errors are everywhere. The engine rolls a number in the interval 0-99 so that 100 is 99 and a range like 0-30 means 31% chance not 30%.

## Arcane spells.

### Level 2.

1. Sound Burst: damage bypass MI? AoE damage, so tentatively yes.

### Level 3.

1. Slow has a Remove Spell Type Protections 12 = Combination. Is this correct?
2. Protection from Missiles: all projectiles covered?
3. Spell Thrust: why is the real work being deferred to and split between two subspells?
4. Halt Undead: implementation can be simplified, since we can filter undead with 318 with stat type not match undead.
5. Vampiric Touch and Larloch's Minor Drain: the implementation needs to be refined, as it currently stands it is not correct.

### Level 4.

1. Fire Shield does not seem to protect against insects. Should add protection against sectype k1#insect (or something similar)?
2. Protection from Fire: power of opcodes 5, 3 -> 4. Level 3 -> 4. Though being a level 4 spell, it is coded as a level 5 spell so have to fix durations of first header and add missing headers.
3. Protection from Cold: power of opcodes 5 -> 4. Level 3 -> 4. Same as Protection from Fire.
4. Protection from Electricity: power of opcodes 5 -> 4. Level 5 -> 4. Being coded as a level 5 spell, it misses two headers and the durations of the first header have to be fixed. Description level 4, not 5.
5. Protection from Acid: power of opcodes 5 -> 4. Level 5 -> 4. Same as Protection from Electricity.

### Level 5.

1. Cone of Cold: use EE damage opcode features.
2. Fireburst: use EE damage opcode features on aux spell.
3. Mestil's Acid Shield: same problem as Fire Shield.
4. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not protect against it.

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

### Level 3.

1. Miscast Magic: cast spell opcode has a save, but the aux spell opcodes already have it. State Miscast_Magic -> Wild_Magic?
2. Summon Insects: Why the extra level of indirection with the cast spell opcode in the main spell?
3. Storm Shield: same problem with Protection from Missiles. Non-stacking -> EE refreshing.
4. Regenerate Serious Wounds: extra, unneeded headers.

### Level 4.

1. Free Action: 240 opcodes are not spurious *if* Free Action removes the corresponding icon opcodes.

### Level 5.

1. True Seeing: since resist_dispel on casting opcodes 3 -> 2 should resist_dispel of opcodes in subspell -> 3? The protection from spell is against the IR cloak's reflected image, shouldn't it be remove resource opcode instead?
2. Insect Plague: same issue with Summon Insects.
3. Flame Strike: use EE features of damage opcode.
4. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not have it.
5. Protection line of spells: some of these are on sppr3, so have to go through them.

### Level 6.

1. False Dawn: all but the first cast spell opcodes have magic resist 3. But these are delayed cast spell opcodes so it is at least plausible that they be dispellable, even though the subspell opcodes are themselves dispellable.

### Level 7.

1. Fire Storm: use EE features of damage opcode.
2. Sunray: use EE features of damage opcode.
3. Creeping Doom: in the aux spell that actually implements the insect swarm effects all opcodes have power 0. Same problem as other insect spells.