# A. Spell Revisions.

[Spell Revisions](https://github.com/Gibberlings3/SpellRevisions) is a spell mod overhaul for the Baldur's Gate saga, and in my view an essential mod, as it overhauls and balances the whole spectrum of available spells, making the game much more enjoyable. Another major plus, is that it is taken into account of by [SCS](https://github.com/Gibberlings3/SwordCoastStratagems), which is in my view *the* essential mod for any Baldur's Gate playthrough, as it is the only usable, and fairly competent at that, AI mod.

## A. 1. State of the mod.

As of this writing (May, 2021), the mod has not had a single commit since April 2020. My list of patches is sitting there since January 2020. This is not a complaint, just an acknowledgment of the situation. The BG community is small with few developers, and the few there are, eventually move on and have better things to do. What this means is that the mod is stuck at the beta v18. It is a solid general principle of life, that if one wants anything fixed, one must do it oneself; if you consult the list of patched spells, there are at this moment (May 2021) over 150 entries. In all fairness, most of the fixes are of the Harmless category, but it is still a very large number.

Bartimaeus has single-handedly tried to patch and fix the mod -- consult the [thread](https://www.gibberlings3.net/forums/topic/29618-sr-revised-v13200-2020-august-22nd). There are however two major problems with his work. First, his mod is copied over and overrides the SR mod, which means that the specific fixes are either documented or get lost in memory's sinkhole. And we all know how documentation goes. The second problem is that Bartimaeus has added on top of his fixes a whole sleuth of tweaks. The problem are not the tweaks themselves; while I can quibble with some of them, I actually like the general direction he has taken with the mod. The problem is rather that we end up without a base mod in a usable state, with no clear separation between what is a fix and what is a tweak.

## A. 2. State of the tools.

There are two main tools used. The first is the WeiDU program, whose most important component is the programming language for batch patching of the game's data files. It is the de-facto tool for modding the game. Having stated, in a rather subdued fashion, my thanks, the plain matter of fact is that programming in WeiDU is an exercise in sustained masochism. The language, as a programming language, is one ginormous clustercrap. The ideal would be to use a well-tested, extensive, newbie-friendly language, [Python](https://www.python.org) say, on top of a solid API, and with cross-compatibility with WeiDU. But ideals are ideals and reality is what it is; while I have started such a project it is far from completion, so WeiDU it is (and then mutter insults at the screen).

The second major tool is the editor [NearInfinity](https://github.com/Argent77/NearInfinity). It is a good editor, very useful for testing and special-purpose file editing. My only complaint, and not a small one sad to say, is that the UI is clunky and not very friendly. The looks are also ugly, but that is a fairly minor gripe.

# B. Patches.

In this section we document the patches for the user's convenience. If there is some patch you think should not be applied, just edit the patches.2da file and turn the value in the patch column from 1 to 0.

Do note that the ultimate authority is the code, which is even commented in reasonable detail; there is always the chance that this doc is not 100% synched with the code, so bear that in mind (if that bothers you, patches are welcomed).

## B. 1. Patch classification.

Since the list of patches is very long, it is useful to classify them, so the user can have a sense of the scale of the problems. We classify the problems in four categories, in increasing degree of severity.

* Harmless: while technically incorrect, the fix does not change the behavior in any noticeable way. A typical example is the power level of summoning effects. The power level is important for the interaction with spell protections, but obviously these are quite irrelevant for summoning spells. The reason these are included are, besides a general OCD desire for completeness and perfectionism, is that when first making the fix it is usually not clear whether the bug is harmless or not.

* Minor: the bug does not affect gameplay; this includes aesthetics, e.g. display strings have wrong saves, whose only effect is that you will probably see a line on the log that should not be there.

* Major: the bug affects gameplay in some way, e.g. important spell opcodes have the wrong save penalties, or the wrong resist_dispel flag or power level, so that they are/are not blocked when they should/should not be, interact incorrectly with spell protections, have the wrong durations, etc. Other major problems include misleading or flat-out wrong spell descriptions.

* Severe: more majorly major than major.

This classification is not a hard and fast rule, it is just meant as a guide. Parallel, there are a few other tags that can be added to a spell patch.

* Implementation: implementation is changed, usually to take advantage of EE features, while keeping the functional behavior intact or mostly intact. A typical example is the new EE features for the sleep and damage opcodes, or using 321 opcodes to make a spell refresh and non-stacking instead of just non-stacking.

## B. 2. Arcane spells.

### Level 1.

1. [Harmless] Grease: delete spurious 324 opcode.
2. [Minor, Implementation] Charm Person: add Display String 1476 = Charmed. Simplify implementation of racial immunities.
3. [Minor] Obscuring Mist: same fixes as Cleric's version.
4. [Harmless] Monster Summoning I: corrected power 6 -> 0 of summoning effect.
5. [Minor] Protection from Petrification: display string "Gaze Reflection" -> "Protection from Petrification".
6. [Major] Expeditious Retreat: resist_dispel is all over the place. Standardized -> 3.
7. [Harmless] True Strike: opcodes' target preset (2) -> self (1). Spurious 321 and 206 opcodes removed.
8. [Major, Implementation] Shield: play sound resist_dispel 1 -> 3. Added protection against TRAP_MAGIC_MISSILE. Changed spell from non stacking to EE refresh. Remove protection against wand03 as spell does not even exist. Add set state WIZARD_SHIELD opcode.

note(s):
* wand03 does not exist in vanilla but IR may introduce it. In vanilla, wand03 item sets up protection by checking state WIZARD_SHIELD.

9. [Minor, Implementation] Sleep: changed implementation to use EE features of sleep code. This meant the deletion of the cast spell on condition and display portrait icon opcodes. All headers but first and last lack display string opcode.
10. [Harmless] Chill Touch: create item opcode amount 0 -> 1.
11. [Major, Implementation] Larloch's Minor Drain: use EE drain features of damage opcode for correct implementation (e.g. correct interaction with mr).
12. [Minor] Spook: display string opcode resist_dispel 0 -> 1.

### Level 2.

1. [Harmless] Blur: power of opcodes 3 -> 2.
2. [Minor] Detect Invisibility: opcode Invisibility Detection resistance_dispel 2 -> 3.
3. [Implementation] Battering Ram: Sleep opcode gained special = 130 (Unconscious) for icon and strref.
4. [Major, Implementation] Know Opponent: fix casting time 9 -> 2 to synch with cleric's version. Description fix. Implementation of golem immunity has a spurious Use Eff and can use instead a 318 opcode. Prevent stacking as it is done in Cleric's version with 321 opcodes.
5. [Major] Luck: detect illusion resistance_dispel 0 -> 3.
6. [Harmless] Resist Fear: reset morale duration 40 -> 0. Removed protection from mummydsp, a non-existent spell.
7. [Major] Melf's Acid Arrow: remove 324 opcode as with it, missile damage is also blocked by acid immunity.
8. [Implementation] Mirror Image: change non-stacking to EE refreshing and added protection against Reflected Image.
9. [Harmless] Monster Summoning II: corrected power of summoning effect.
10. [Harmless] Ghoul Touch: create item opcode amount 0 -> 1.
11. [Harmless] Vocalize: play visual effect opcode power 1 -> 3.
12. [Implementation] Power Word Sleep: Sleep opcode gained EE special = 14 for icon and strref.
13. [Minor] Ray of Enfeeblement: both the first play sound and character pulse opcodes lack save.
14. [Harmless] Chaos Shield: play visual and second protection from spell have target preset (2) instead of self (1).
15. [Implementation] Sound Burst: use EE damage opcode features on aux spell.
16. [Harmless] Glitterdust: remove spurious protection against non-existent sppr314f.

### Level 3.

1. [Harmless] Clairvoyance: save vs. breath bonus power 0 -> 2.
2. [Harmless] Dispel Magic: timing of Remove spell type protections opcode 3 -> 1. Power of Play Visual 3 -> 0 for consistency with other opcodes.
3. [Major] Flame Arrow: same issue with Melf's acid arrow, with fire immunity blocking missile damage, so 324 opcode removed from spell and aux spells.
4. [Implementation] Fireball: use EE damage opcode features.
5. [Minor] Hold Person: added display string "Held" opcode.
6. [Implementation] Lightning Bolt: use EE damage opcode features.
7. [Harmless] Monster Summoning III: summoning eff power 6 -> 0.
8. [Harmless] Non-detection: protection from spell opcodes power 0 -> 3.
9. [Major] Slow: The second Remove Spell Type Protections has a save bonus. The first play sound opcode has no save and timing mode 0 -> 1.
10. [Implementation] Skull Trap: use EE damage opcode features.
11. [Major, Implementation] Vampiric Touch: same fixes as Larloch's minor drain.
12. [Major] Wraitform: resist_dispel -> 3. It also blocks innate abilities, but this is undocumented so it is removed. Display String opcode power 0 -> 3.
13. [Minor, Implementation] Dire Charm: add Display String 14780 = "Dire Charmed". Simplify implementation of racial immunities.
14. [Harmless] Minor Spell Deflection: spurious repeated modify proficiency and set spell state opcodes.
15. [Major] Spell Thrust: made trigger radius equal to explosion. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.

### Level 4.

1. [Major] Ice Sorm: fix duration of movement rate penalty 24 -> 7. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.
2. [Minor] Minor Globe of Invulnerability: remove double fireball protection. Change description to mention correct duration of 2 turns.
3. [Severe] Contagion: Opcode's power 3 -> 4. Casting spell opcodes cast non-existent "sppr311d" -- clone aux spell from cleric version and patch it in.
4. [Minor] Break Enchantment: add remove charmed icon opcodes. Clarified description to mention it removes petrification.
5. [Minor] Greater Malison: Change description to mention correct duration of 2 turns.
6. [Minor] Teleport Field: mention in description it bypasses mr.
7. [Minor] Farsight: clairvoyance opcode power 3 -> 4. Clairvoyance and farsight opcodes resist/dispel -> 2.
8. [Harmless] Monster Summoning IV: summoning eff power 8 -> 0.

### Level 5.

1. [Minor] Shadow Door: opcode 215 resist_dispel 0 -> 3.
2. [Harmless] Monster Summoning V: summoning eff power 6 -> 0.
3. [Minor] Hold Monster: lacks portrait icon and display string opcodes.
4. [Minor] Conjure Lesser Fire Elemental: The first play visual effect opcode (that should play on a successful summon) has the wrong probability 86 - 100 -> 0 - 85.
5. [Harmless] Phantom Blade: create weapon opcode amount 0 -> 1.
6. [Minor] Conjure Lesser Air Elemental: same problem as Conjure Lesser Fire Elemental.
7. [Minor] Conjure Lesser Earth Elemental: same problem as Conjure Lesser Fire Elemental.

### Level 6.

1. [Major] Globe of Invulnerability: fix description that duration is 2 turns. Missing spell protections: Ice Storm.
2. [Harmless] Tenser's Transformation: play visual effect resist/dispel 2 -> 3.
3. [Harmless] Banishment: Use Eff against non-existent row in gender.ids removed from spell and aux spell.
4. [Major] Protection from Magic Energy: contrary to what is stated, no creatures are immune to this spell; fixed description.
5. [Minor] Pierce Magic: incorrect duration of display portrait and play sound opcodes.
6. [Major] True Seeing: fixes as in the cleric version.
7. [Minor] Improved Haste: repeated protection against slow removed and protection against non-existent spwi545 -> changed to spwm164.
8. [Minor] Contingency: three headers why? Removed. Last cast spell opcode for what? It does nothing. Removed.
9. [Minor] Conjure Fire Elemental: same problem as Conjure Lesser Fire Elemental, with wrong probabilities in first visual opcode.
10. [Minor] Conjure Air Elemental: see Conjure Fire Elemental.
11. [Minor] Conjure Earth Elemental: see Conjure Fire Elemental.
12. [Harmless] Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2.
13. [Harmless] Monster Summoning VI: summoning eff power -> 0.

### Level 7.

1. [Harmless] Monster Summoning VII: summoning eff power 6 -> 0.
2. [Harmless] Summon Death Knight: summoning eff power 7 -> 0.
3. [Major] Chaos: Opcodes have save penalty -6 instead of -4 and hold opcode has no save. Add display icon for Hold, Sleep and Berserk effects, and string for Hold. Misses immunity by chaotic commands.
4. [Major] Finger of Death: damage is 2d8 + level not 6d8 as advertized. Here I am going by the implementation instead of the docs.
5. [Major] Power Word Stun: has an undocumented one-round stun with a save -2.

### Level 8.

1. [Minor] Ghostform: play sound opcodes have power 6 -> 8. Misses protection from strings 54337 = "Diseased", 17398 = "Nauseated", 14662 = "Poisoned".
2. [Major] Mind Blank: misses immunity to berserk, protection from icons: 2 = Rigid Thinking, 4 = Berserk, and strings: 14672 = "Charmed", 14780 = "Dire Charmed", 14102 = "Held", 14650 = "Paralyzed", 23744 = "Feebleminded", 14665 = "Petrified", 340 = "Held", 1473 = "Held"
3. [Harmless] Monster Summoning VIII: summoning eff power 6 -> 0.
4. [Harmless] Summon Fiend: summoning eff power 8 -> 0.
5. [Major] Symbol of Weakness: save penalty -6 -> -4.
6. [Major] Horrid Wilting: undocumented protection for elemental creatures; removed it.
7. [Major] Symbol of Death: opcode save penalty -6 -> -4.

### Level 9.

1. [Major] Spell Trap: duration is fixed 2 turns.
2. [Harmless] Gate: summoning eff power 8 -> 0.
3. [Major] Meteor Swarm: second damage opcode save -6 -> -4.
4. [Minor] Wail of the Banshee: Misses display icon deaf opcode.
5. [Harmless] Larloch's Drain: resist_dispel in several self-targeted opcodes 1 -> 3.
6. [Harmless] Black Blade of Disaster: amount 0 -> 1 in create item opcode.
7. [Major] Shapechange: Remove spurious natural abilities. Play visual opcodes power 4 -> 9.
8. [Minor] Freedom: lacks remove icons: 4 = Berserk, 130 = unconscious, 144 = entangle and 145 = grease.
9. [Minor] Bigby's Crushing Hand: add display stun icon opcode to aux spell.

## B. 3. Divine spells.

### Level 1.

1. [Minor, Implementation] Command: play sound opcode lacks save vs. spell and respective save penalty. Resist_dispel of first play visual opcode to dispel/bypass mr. Use EE features on sleep opcode.
2. [Minor] Cure Light Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
3. [Harmless] Resist Fear: sets protection from non-existent spell "mummydsp".
4. [Major] Entangle: duration of opcodes 8 -> 7 to conform with similar cloud spells. See Kjeron's comments in the thread [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions).
5. [Minor] Sanctuary: play sound opcode duration 60 -> 36.
6. [Harmless] Shilellagh: create item opcode amount 0 -> 1.
7. [Minor] Strength of Stone: misses protection from druid version of Gust of Wind.
8. [Harmless] Good Berry: power of opcodes 2 -> 1.
9. [Major] Cause Light Wounds: power of opcodes 4 -> 1. This implies it *does* not bypass MGoI.
10. [Major, Implementation] Sunscorch: change documentation to explicitly mention that a save vs. spell (not vs. breath, as rolled to halve the damage) will avoid blindness. Use EE features of damage opcode.
11. [Major] Regenerate Light Wounds: resist_dispel -> 3. Only headers 7 and higher have Display String "Regenerating" -- added missing opcodes.
12. [Minor] Obscuring mist: remove spurious 284 opcode with probability 0. resist dispel -> 2 for 0 and 164 opcodes, as the penalties are from critters being inside the cloud and thus not blocked by mr or dispellable.

### Level 2.

1. [Major] Chant: Added 321 opcode to prevent stacking saves and removed spurious 321 opcode from aux spell.
2. [Minor, Implementation] Charm Person or Animal: the second 324 opcode is spurious. Simplified racial immunities with 318 opcode. Add Display String 1476 = Charmed.
3. [Major] Find Traps: auxiliary spell duration 20 -> 6 and target 2 (preset) -> 1 (self).
4. [Harmless] Flame Blade: create item opcode amount 0 -> 1.
5. [Major] Hold Person: play sound opcode has a save bonus of -1. Added display string 14102 = "Held" opcode. Remove spurious 321 opcodes targeting know opponent.
6. [Major] Know Opponent: Description changed casting speed 1 round -> 2. Implementation of golem immunity as in wizard's version.
7. [Minor] Slow Poison: add remove intoxicated icon opcode since the spell also cures intoxication. Add remove poisoned icon opcode.
8. [Harmless] Spiritual Hammer: create item opcode amount 0 -> 1.
9. [Minor] Cure Moderate Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
10. [Implementation] Fire Trap: changed implementation to use EE features, namely the save for half flag.
11. [Major] Regenerate Moderate Wounds: power of opcodes 1 -> 2, resist_dispel 0 -> 3. Add Display String "Regenerating" for consistency with Regenerate Light Wounds.
12. [Major] Gust of Wind: sleep opcode resist_dispel 0 -> 1. Add to description that it dispels insect swarms. Sleep opcode gained special = 130 for icon and strref.
13. [Major] Cause Moderate Wounds: power 0 -> 2.
14. [Harmless] Animal Summoning II: summoning eff power 8 -> 0.
15. [Major] Draw upon Divine Might: remove cast spell dvwinded. Winded at the end of DuDM is undocumented, a relic of earlier SR versions.

### Level 3.

1. [Minor] Break Enchantment: add remove charm icon opcodes. Add to description that it also removes petrification.
2. [Harmless] Remove Paralysis: delete spurious prevent portrait opcode.
3. [Harmless] Holy Smite: first header has a spurious Use Eff mask_evil opcode. Holy Smite no longer targets only evils.
4. [Major] Unholy Blight: first header has a spurious Use Eff mask_good opcode. Unholy Blight no longer targets only goody two-shoes. Base thac0 penalty, Lighting Effects and Play Sound opcodes lack saves. All the save penalty opcodes have an undocumented penalty -2 -- all the more because Holy Smite does not have this penalty for its blindness effect.
5. [Minor] Cure Serious Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
6. [Major] Gust of Wind: sleep opcode resist_dispel 0 -> 1. 

note(s):
* contrary to the druid version, it does *not* dispel insect swarms. It also has a *documented* penalty of -2 to the saves.

7. [Major] Summon Insects: panic on failed save vs. poison is not referred to in the description. Added it to description.
8. [Harmless] Animal Summoning III: summoning eff power 8 -> 0.
9. [Major] Contagion: aux spell had 5th disease opcode (slow) probability1 0. The opcode is undocumented, so it is deleted. Remove spurious protection against chaotic commands.
10. [Major] Cause Serious Wounds: opcode's power 4 -> 3.
11. [Harmless] Regenerate Serious Wounds: opcode's power 1 -> 3.

### Level 4.

1. [Minor] Cure Critical Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
2. [Harmless] Animal Summoning IV: summoning eff power 8 -> 0.
3. [Major] Free Action: first header repeated protection against spell spwm164, but missing protection against web so altering the resource in the opcode. Second and subsequent headers miss protection against spells wish slow, spin983 slow and web. All headers missing protection against strings 14650 = "Paralyzed" and 14668 = "Slowed". Remove spurious 240 opcodes.
4. [Minor] Neutralize Poison: misses protection against string 14662 = "Poisoned". Add remove poisoned icon opcode.
5. [Minor] Domination: misses opcode display string 8364 = "Dominated"
6. [Minor] Farsight: errors in several opcode's power and resist_dispel.
7. [Major] Ice Sorm: fix duration of movement rate penalty 24 -> 7. Damage opcode has an undocumented +2 damage.
8. [Harmless] Regenerate Critical Wounds: power of opcodes 1 -> 4.

### Level 5.

1. [Harmless] Animal Summoning V: summoning eff power 8 -> 0.
2. [Harmless] Raise Dead: power of Use Eff opcode 4 -> 5.
3. [Major] True Seeing: immunity to blindness lacks the appropriate protection from icon and string opcodes. resist_dispel on the opcodes is also inconsistent: some are not dispellable, others like the delayed cast spell and aesthetic opcodes are, which makes no sense. Lacks protection from cleric's obscuring mist.
4. [Major] Chaotic Commands: first header lacks 8 opcodes that the other headers have. Several protection against string opcodes missing.
5. [Major] Greater Command: play visual effect power 1 -> 5. Play sound lacks a save and has wrong duration. Delayed cast spell opcode has wrong duration 9 -> 6. It also casts sppr512b instead of the correct sppr512a (one less round).
6. [Minor] Mass Cure: add remove intoxicated icon opcode since the spell also cures intoxication.
7. [Harmless] Repulsion: aux spell sppr515d play visual opcode 215 power 2 -> 5.
8. [Major] Insect Plague: mention that on a failed save vs. poison the victim panics for the round.
9. [Harmless] Mass Regeneration: power of opcodes 0 -> 5.
10. [Severe] Animal growth: Use EFF opcode calls wrong eff. Aux spell dvpr525d has undocumented strength and constitution bonuses opcodes, with wrong duration to boot. Deleted. If the strength bonuses are to be kept then they must be documented; the constitution bonus does not make much sense though, because the hps are already being raised.

### Level 6.

1. [Harmless] Animal Summoning VI: summoning eff power 8 -> 0.
2. [Minor] Heal: add missing remove poisoned icon opcode.
3. [Major] Dolorous Decay: play visual effect has a save penalty -5 -> 4. Damage amount of disease was 2 -> 1. Second display icon has timing mode 4. It seems this should be timing 1. Mention it bypasses mr.
4. [Harmless] Physical Mirror: power of opcodes in aux spell sppr613d 1 -> 6.
5. [Major] Regeneration: the description explicitly mentions the possibility of dispellability but opcodes have resist_dispel 0 -> 3. Description fix that duration is 3 turns.
6. [Harmless] Banishment: same problems as wizard's Banishment.
7. [Harmless] Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2, power 0 -> 6.

### Level 7.

1. [Harmless] Summon Shambling Mound: summoning eff power 8 -> 0.
2. [Harmless] Summon Death Knight: summoning eff power 7 -> 0.
3. [Severe] Nature's Beauty: removed undocumented slay opcode. Added display string "Charmed" opcode.
4. [Major] Finger of Death: damage is 2d8 + level not 6d8 as advertized. Here I am going by the implementation instead of the docs.
5. [Minor] Chaos: Add display icon for Hold, Sleep and Berserk effects, and string for Hold.
6. [Minor] Holy Word: second visual opcode has probability1 0 while first has resist_dispel 2 -> 1. Added missing display icon for stun. Did *not* add display string opcodes.
7. [Major] Regeneration: description explicitly mentions the possibility of dispellability but opcodes have resist_dispel 0 -> 3. Description fix that duration is 3 turns.
8. [Minor] Greater Restoration: added missing remove portrait icons: entangled = 144, grease = 145, poison = 6, panic = 36, berserk = 4.
9. [Major] Unholy Word: second visual opcode has probability1 0 while first has resist_dispel 2 -> 1. Neither the display portrait icon for confusion nor the cast spell scale their maximum levels but rather stay constant (at 0 and 13 respectively). Auxiliary spell implementing slow has a spurious remove illusionary protections opcode. Last play sound opcode, marking the end of slow has duration 0 -> 30.
10. [Harmless] Animal Summoning VII: summoning eff power 8 -> 0.
11. [Major] Creeping Doom: mention that on a failed save vs. poison at -2 the victim panics for the round.
