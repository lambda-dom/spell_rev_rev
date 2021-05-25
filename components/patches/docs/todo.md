# A. Patches.

General points:

1. In summoning spells, the Use Eff has the required duration but the eff itself is usually something like 2400 and may even have timing = 1. This is harmless, but synch up the durations?
2. Off by one probability errors are everywhere. The engine rolls a number in the interval 0-99 so that 100 is 99 and a range like 0-30 means 31% chance not 30%.
3. Have to patch scrolls as well.

## Arcane spells.

### Level 2.

1. Sound Burst: damage bypass MI? AoE damage, so tentatively yes, but it would mean a nerf to enemy mages.

### Level 3.

1. Slow has a Remove Spell Type Protections 12 = Combination. Is this correct?
2. Protection from Missiles: all projectiles covered?
3. Halt Undead: implementation can be simplified, since we can filter undead with 318 with stat type not match undead.

### Level 4.

1. Otiluke's Sphere: Why Use Eff on GOODCUTOFF?

### Level 5.

1. Waves of Fatigue: undead and constructs should be immune to the spell per the description.
2. Dispelling Screen and Spell Shield: the way they are set up is that they raise protections against the relevant effects / spells and then the spell removals first try to remove the appropriate protections, then they remove spell shield.
3. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not protect against it.

### Level 7.

1. Prismatic Mantle: it has no protection from weapons opcode and yet the Modify Prof and Set Spell State opcodes seem to indicate otherwise. The description is also misleading; blindness always applies to creatures less than 8HD, etc. Can use EE damage opcode features in condition spell.

### Level 8.

1. Mind Blank: protects against power word stun but not stun.

### Level 9.

1. Wail of the Banshee: should the slay opcode change position with lighting effects?
2. Imprisonment: the implementation is still imperfect, as e.g. the kill and or freedom opcodes could be blocked.

## Divine spells.

### Level 3.

1. Miscast Magic: State Miscast_Magic -> Wild_Magic?
2. Magic Fang: use Enchantment Bonus 345 opcode in aux spells?

### Level 4.

1. Free Action: 240 opcodes are not spurious *if* Free Action removes the corresponding icon opcodes.
2. Cloak of Fear: immunity to fear blocks the thac0 penalty. Is this correct?

### Level 5.

1. True Seeing: The protection from spell is against the IR cloak's reflected image, shouldn't it remove resource opcode instead? Uses Force Visible (136) to dispel invis.
2. Magic Resistance: implementation non-stacking -> EE refreshing.
3. Feeblemind: lacks display string 23744 = "Feebleminded", but most mind shield protections do not have it.

### Level 6.

1. False Dawn: all but the first cast spell opcodes have magic resist 3. These are delayed cast spell opcodes so it this means they are dispellable, is this correct?
2. Sol's Searing Orb: can use EE damage opcode features to slightly simplify implementation of item's ranged header.

### Level 7.

1. Sunray: killing undead is done via 1000 magic damage opcode. It could still be blocked by magic damage immunity; insta-kill opcodes probably do not work as undead are usually immune to them.
2. Creeping Doom: in the aux spell that actually implements the insect swarm effects all opcodes have power 0, same as Insect Plague.