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
3. Halt Undead: implementation can be simplified, since we can filter undead with 318 with stat type not match undead.
4. Vampiric Touch and Larloch's Minor Drain: the implementation should now be correct, but aesthetic can be aligned closer to SR.

### Level 4.

1. Oitiluke's Sphere: Why Use Eff on GOODCUTOFF?
2. Protection from elemental energy: go over each of the individual subspells.

### Level 5.

1. Waves of Fatigue: undead and constructs should be immune to the spell per the description.
2. Dispelling Screen and Spell Shield: the way they are set up is that they raise protections against the relevant effects / spells and then the spell removals first try to remove the appropriate protections, then they remove dispelling screen and or spell shield.
3. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not protect against it.

### Level 7.

1. Ruby Ray of Reversal and Khelben's Warding Whip: power of opcodes -> 0.
2. Prismatic Mantle: it has no protection from weapons opcode and yet the Modify Prof opcode seems to indicate otherwise. More generally, the description seems to be a complete mess. It is misleading; blindness always applies to creatures less than 8HD, etc.
3. Delayed Blast Fireball: use EE damage opcode features.
4. Several spells miss or have too many headers (and therefore have wrong durations in first header): Protection from Elements, Summon Nishruu, Control Undead. There may be other examples.
5. Khelben's warding whip: change to delayed cast subspell 4 times.

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
3. Storm Shield: same problem with Protection from Missiles. Non-stacking -> EE refreshing. Add protection against sectype.
4. Regenerate Serious Wounds: extra, unneeded headers.
5. Magic Fang: use Enchantment Bonus 345 opcode in aux spells?

### Level 4.

1. Free Action: 240 opcodes are not spurious *if* Free Action removes the corresponding icon opcodes.
2. Cloak of Fear: immunity to fear blocks the thac0 penalty. Is this correct?

### Level 5.

1. True Seeing: The protection from spell is against the IR cloak's reflected image, shouldn't it be remove resource opcode instead? Uses Force Visible (136) to dispel invis.
2. Magic Resistance: implementation non-stacking -> EE refreshing.
3. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not have it.

### Level 6.

1. False Dawn: all but the first cast spell opcodes have magic resist 3. These are delayed cast spell opcodes so it this means they are dispellable, is this correct?
2. Sol's Searing Orb: can use EE damage opcode features to slightly simplify implementation of item's ranged header.

### Level 7.

1. Fire Storm: use EE features of damage opcode.
2. Sunray: use EE features of damage opcode.
3. Creeping Doom: in the aux spell that actually implements the insect swarm effects all opcodes have power 0. Same problem as other insect spells.