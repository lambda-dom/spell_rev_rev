# A. Spell Revisions.

[Spell Revisions](https://github.com/Gibberlings3/SpellRevisions) is a spell mod overhaul for the Baldur's Gate saga, and in my view an essential mod, as it overhauls and balances the whole spectrum of available spells, making the game much more enjoyable. Another major plus is that it is taken into account of by [SCS](https://github.com/Gibberlings3/SwordCoastStratagems), which is in my view *the* essential mod for any Baldur's Gate playthrough, as it is the only usable, and very competent at that, AI mod.

## A. 1. State of the mod.

As of this writing (May 2021), the mod has not had a single commit since April 2020. My list of patches is sitting there since January 2020. This is not a complaint, just an acknowledgment of the situation. The BG community is small with few developers, and the few there are eventually move on and have better things to do. What this means is that the mod is stuck at the beta v18. It is a solid general principle of life, that if one wants anything fixed, one must do it oneself; if you consult the list of patched spells, there are at this moment (late May 2021) over 220 entries. In all fairness, most of the fixes are of the Harmless category, but it is still a very large number.

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

note(s):
* scroll descriptions patched via STRING_SET.

### Level 1.

1. [Harmless] Grease: delete spurious 324 opcode.
2. [Minor, Implementation] Charm Person: add Display String 1476 = Charmed. Simplify implementation of racial immunities.
3. [Minor] Obscuring Mist: same fixes as Cleric's version. Corresponding scroll has incorrect inventory and header icons. Fixed bam.

note(s):
* The scroll icon marked as fixed in [Spell Revisions](https://github.com/Gibberlings3/SpellRevisions).

4. [Minor] Monster Summoning I: corrected power 6 -> 0 of summoning effect. Corresponding scroll has incorrect inventory and header icons.

note(s):
* The scroll icon marked as fixed in [Spell Revisions](https://github.com/Gibberlings3/SpellRevisions).

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
2. [Minor] Detect Invisibility: opcode Invisibility Detection resist_dispel 2 -> 3.
3. [Minor] Horror: play visual opcode resist_dispel 3 -> 1.
4. [Implementation] Battering Ram: Sleep opcode gained special = 130 (Unconscious) for icon and strref.
5. [Major, Implementation] Know Opponent: fix casting time 9 -> 2 to synch with cleric's version. Description fix. Implementation of golem immunity has a spurious Use Eff and can use instead a 318 opcode. Prevent stacking as it is done in Cleric's version with 321 opcodes.
6. [Major] Luck: detect illusion resist_dispel 0 -> 3.
7. [Harmless] Resist Fear: reset morale duration 40 -> 0. Removed protection from mummydsp, a non-existent spell.
8. [Major] Melf's Acid Arrow: moved 324 opcode after missile damage so as not to block it by acid immunity.
9. [Implementation] Mirror Image: change non-stacking to EE refreshing.
10. [Harmless] Monster Summoning II: corrected power of summoning effect.
11. [Harmless] Ghoul Touch: create item opcode amount 0 -> 1.
12. [Harmless] Vocalize: play visual effect opcode power 1 -> 3.
13. [Implementation] Power Word Sleep: Sleep opcode gained EE special = 14 for icon and strref.
14. [Minor] Ray of Enfeeblement: both the first play sound and character pulse opcodes lack save.
15. [Harmless] Chaos Shield: play visual and second protection from spell have target preset (2) instead of self (1).
16. [Implementation] Sound Burst: use EE damage opcode features on aux spell.
17. [Harmless] Glitterdust: remove spurious protection against non-existent sppr314f.

### Level 3.

1. [Harmless] Clairvoyance: save vs. breath bonus power 0 -> 2.
2. [Major] Dispel Magic: timing of Remove spell type protections opcode 3 -> 1. Power of Play Visual 3 -> 0 for consistency with other opcodes. More importantly, it first removes dispel screen and *then* dispels. This is incorrect, as it violates Dispelling Screen description. Reordered the opcodes.
3. [Major] Flame Arrow: same issue with Melf's acid arrow, with fire immunity blocking missile damage, so 324 opcode removed from spell and aux spells and moved to an aux subspell.
4. [Implementation] Fireball: use EE damage opcode features.
5. [Minor] Hold Person: added display string "Held" opcode.
6. [Implementation] Lightning Bolt: use EE damage opcode features.
7. [Harmless] Monster Summoning III: summoning eff power 6 -> 0.
8. [Harmless] Non-detection: protection from spell opcodes power 0 -> 3.
9. [Major] Slow: The second Remove Spell Type Protections has a save bonus (harmless) and duration of 60 -> timing instant. The first has resist_dispel = 3 -> 1 for consistency with second opcode. Remove haste icon probability 0 -> 100. The first play sound opcode has no save and timing mode 0 -> 1.
10. [Implementation] Skull Trap: use EE damage opcode features.
11. [Major, Implementation] Vampiric Touch: same fixes as Larloch's minor drain.
12. [Major] Wraitform: resist_dispel -> 3. It also blocks innate abilities, but this is undocumented so it is removed. Display String opcode power 0 -> 3.
13. [Minor, Implementation] Dire Charm: add Display String 14780 = "Dire Charmed". Simplify implementation of racial immunities.
14. [Harmless] Minor Spell Deflection: spurious repeated modify proficiency and set spell state opcodes.
15. [Major] Spell Thrust: made trigger radius equal to explosion. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.

### Level 4.

1. [Major] Ice Sorm: fix duration of movement rate penalty 24 -> 7. See Kjeron's comments in the [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions/) thread.
2. [Major] Minor Globe of Invulnerability: remove double fireball protection. Change description to mention correct duration of 2 turns.
3. [Severe] Contagion: Opcode's power 3 -> 4. Casting spell opcodes cast non-existent "sppr311d" -- clone aux spell from cleric version and patch it in.
4. [Minor] Break Enchantment: add remove charmed icon opcodes. Clarified description to mention it removes petrification.
5. [Major] Emotion, Despair: ac penalty opcode in first header has wrong duration.
6. [Major] Greater Malison: change description to mention correct duration of 2 turns. Undocumented thac0 bonus +1 (??) removed.
7. [Severe] Otiluke's Sphere: the same spell is cast on enemies and friends -> renaming and patching. In the aux spells themselves, they have different numbers of opcodes and, contrary to the description, mr *is* bypassed on enemies. So used the friendly version, as it has the largest number of protections, and patched the missing ones as best as possible. Then used this as the enemy version, changing resist_dispel 3 -> 1.
8. [Major] Fire Shield: lacks protection against insects: added protection against sectype k1insect and insect spells.
9. [Minor] Teleport Field: mention in description it bypasses mr.
10. [Severe] Protection from Elemental Energy: not working, as not referencing the correct 2da resource. The subspells Protection from Electricity and Acid have wrong descriptions and spell level.
11. [Harmless] Monster Summoning IV: summoning eff power 8 -> 0.
12. [Minor] Farsight: clairvoyance opcode power 3 -> 4. Clairvoyance and farsight opcodes resist/dispel -> 2.
13. [Implementation] Vitriolic Sphere: use EE damage opcode features. Corresponding scroll has incorrect inventory and header icons.

### Level 5.

1. [Harmless] Summon Shadow: summoning eff power corrections 6 -> 0.
2. [Implementation] Cone of Cold: use EE damage opcode features.
3. [Harmless] Monster Summoning V: summoning eff power 6 -> 0.
4. [Minor] Shadow Door: opcode 215 resist_dispel 0 -> 3.
5. [Major] Mental Domination: first 174 play sound opcode lacks save. Save penalty is -4 not the documented -2.
6. [Minor] Hold Monster: lacks portrait icon and display string opcodes.
7. [Minor] Dispelling Screen: added protection against dispelling animation to account for reordering opcodes in dispel magic.
8. [Major] Breach: Mind Blank missing from the specific protections list. Moment of Prescience missing from the combat protections list.
9. [Minor] Conjure Lesser Fire Elemental: The first play visual effect opcode (that should play on a successful summon) has the wrong probability 86 - 100 -> 0 - 85. Summoning eff power correction 5 -> 0.
10. [Harmless] Phantom Blade: create weapon opcode amount 0 -> 1.
11. [Minor] Conjure Lesser Air Elemental: same problem as Conjure Lesser Fire Elemental. Summoning eff power correction 5 -> 0.
12. [Minor] Conjure Lesser Earth Elemental: same problem as Conjure Lesser Fire Elemental. Summoning eff power correction 5 -> 0.
13. [Harmless] Spell Deflection: repeated 233 opcode.
14. [Harmless, Implementation] Fire Burst: spurious repetition of protection from immunity. Removed it from main spell. Use EE damage opcode features.
15. [Major] Mestil's Acid Sheath: same problem as Fire Shield.
16. [Minor] Waves of Fatigue: fixed bam.

### Level 6.

1. [Harmless] Invisible Stalker: summoning eff power -> 0.
2. [Major] Globe of Invulnerability: fix description that duration is 2 turns. Missing spell protections: Ice Storm.
3. [Harmless] Tenser's Transformation: play visual effect resist/dispel 2 -> 3.
4. [Major] Flesh to Stone: Main spell: the second remove spell type opcode for k1#haste has inconsistent resist_dispel, timing mode limited and a duration. The cast spell opcode is resist_dispel = 1 -> 2. In the aux spell all opcodes have permanent timing, but with a duration 12, which is harmless, but technically incorrect. Contrary to what is explicitly stated in the description, all opcodes have resist_dispel 1.
5. [Harmless] Banishment: Use Eff against non-existent row in gender.ids removed from spell and aux spell. Fixed bam.
6. [Major] Protection from Magic Energy: contrary to what is stated, no creatures are immune to this spell; fixed description.
7. [Major] Pierce Magic: incorrect duration of display portrait and play sound opcodes. Mind Blank is incorrectly listed as dispelled.
8. [Major] True Seeing: fixes as in the cleric version.
9. [Harmless] Monster Summoning VI: summoning eff power -> 0.
10. [Minor] Protection from Magic Weapons: first protection opcode against magical weapons of enchantment has 0 duration. Deleted it.
11. [Minor] Improved Haste: repeated protection against slow removed and protection against non-existent spwi545 -> changed to spwm164.
12. [Implementation] Chain Lightning: use EE damage opcode features in the two aux spells.
13. [Harmless] Contingency: Last cast spell opcode for what? It does nothing (it says so even in the title). Removed.
14. [Minor] Conjure Fire Elemental: same problem as Conjure Lesser Fire Elemental, with wrong probabilities in first visual opcode. Summoning eff power 6 -> 0.
15. [Minor] Conjure Air Elemental: see Conjure Fire Elemental. Summoning eff power 6 -> 0.
16. [Minor] Conjure Earth Elemental: see Conjure Fire Elemental. Summoning eff power 6 -> 0.
17. [Harmless] Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2. Summoning eff power 3 -> 0. Fixed bam.
18. [Harmless] Summon Nishruu: summoning eff power 8 -> 0.

### Level 7.

1. [Harmless] Greater Spell Deflection: spurious repeated modify proficiency opcode.
2. [Major] Ruby Ray of Reversal: Mind Blank is incorrectly listed as dispelled. Opcodes have power 7 which is inconsistent with other spell protection removals. Changed to 0.
3. [Major] Khelben's Warding Whip: duration of second remove portrait icon opcode incorrect. Mind Blank is incorrectly listed as dispelled. Aux spell dispelling Spell Shield lacks delayed opcodes. Opcodes have power 7 which is inconsistent with other spell protection removals. Changed to 0.
4. [Harmless] Monster Summoning VII: summoning eff power 6 -> 0.
5. [Harmless] Summon Death Knight: summoning eff power 7 -> 0. Fixed bam.
6. [Major] Simbul's Spell Sequencer: aux spell opcode 258 resist_dispel 1 -> 2.
7. [Major] Chaos: Opcodes have save penalty -6 instead of -4 and hold opcode has no save. Add display icon for Hold, Sleep and Berserk effects, and string for Hold. Misses immunity by chaotic commands.
8. [Implementation] Delayed Blast Fireball: use EE damage opcode features.
9. [Major] Finger of Death: damage is 2d8 + level not 6d8 as advertized. Here I am going by the implementation instead of the docs.
10. [Major] Power Word Stun: has an undocumented one-round stun with a save -2.
11. [Harmless] Mordenkainen's Sword: summoning eff power 7 -> 0.
12. [Harmless] Summon Efreet: summoning eff power 6 -> 0.
13. [Harmless] Summon Djinni: summoning eff power 6 -> 0.
14. [Harmless] Limited Wish: summoning eff power 8 -> 0.

### Level 8.

1. [Minor] Ghostform: play sound opcodes have power 6 -> 8. Misses protection from strings 54337 = "Diseased", 17398 = "Nauseated", 14662 = "Poisoned".
2. [Major] Mind Blank: misses immunity to berserk, protection from icons: 2 = Rigid Thinking, 4 = Berserk, and strings: 14672 = "Charmed", 14780 = "Dire Charmed", 14102 = "Held", 14650 = "Paralyzed", 23744 = "Feebleminded", 14665 = "Petrified", 340 = "Held", 1473 = "Held"
3. [Major] Pierce Shield: Mind Blank is incorrectly listed in the spell protections list. Moment of Prescience missing from the combat protections list. Last remove spell protection type power 8 -> 0.
4. [Harmless] Monster Summoning VIII: summoning eff power 6 -> 0.
5. [Harmless] Summon Fiend: summoning eff power 8 -> 0.
6. [Major] Simbul's Spell Trigger: aux spell opcode 258 resist_dispel 1 -> 2.
7. [Major] Incendiary Cloud: fire immunity blocks the other effects, which is not correct per the description: move damage and fire immunity check to a subspell. Resist_dispel of opcodes 2 -> 1.
8. [Major] Symbol of Weakness: save penalty -6 -> -4.
9. [Major] Horrid Wilting: undocumented protection for elemental creatures; removed it. Use EE damage opcode features.
10. [Major] Maze: resist_dispel 1 -> 2.
11. [Minor] Power Word Blind: Protection from cleric's obscuring mist missing. Only the 1 turn version of protection from spells exist, with the exception of Power Word Blind.
11. [Major] Symbol of Death: opcode save penalty -6 -> -4.
12. [Major] Bigby's Icy Grasp: cast spell opcodes power 9 -> 8. Description altered to mention that elementals and incorporeal creatures are immune to the hold effect. Resist_dispel and saves of hold effects are jumbled.

### Level 9.

1. [Major] Spell Trap: duration is fixed 2 turns. Level 1 spell deflection opcode lacks resource field => does not call aux spell when spell trap exhausted as it should.
2. [Minor] Spellstrike: power 9 -> 0.
3. [Harmless] Gate: summoning eff power 8 -> 0.
4. [Major] Imprisonment: kills the victim after 5 rounds. Reword the description to advertize this important fact.
5. [Major] Meteor Swarm: second damage opcode save -6 -> -4. Use EE damage opcode features.
6. [Minor] Wail of the Banshee: Misses display icon deaf opcode.
7. [Harmless] Larloch's Drain: resist_dispel in several self-targeted opcodes 1 -> 3.
8. [Harmless] Black Blade of Disaster: amount 0 -> 1 in create item opcode.
9. [Major] Shapechange: Remove spurious shapeshift into elemental abilities. Play visual opcodes power 4 -> 9.
10. [Minor] Freedom: lacks remove icons: 4 = Berserk, 130 = unconscious, 144 = entangle and 145 = grease.
11. [Minor] Bigby's Crushing Hand: add display stun icon opcode to aux spell.

## B. 3. Divine spells.

note(s):
* scroll descriptions patched via STRING_SET.

### Level 1.

1. [Minor, Implementation] Command: play sound opcode lacks save vs. spell and respective save penalty. Resist_dispel of first play visual opcode to dispel/bypass mr. Use EE features on sleep opcode.
2. [Minor] Cure Light Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
3. [Harmless] Resist Fear: remove protection from non-existent spell "mummydsp".
4. [Major] Entangle: duration of opcodes 8 -> 7 to conform with similar cloud spells. See Kjeron's comments in the thread [SR fixes questions](https://www.gibberlings3.net/forums/topic/30963-sr-fixes-questions).
5. [Minor] Sanctuary: play sound opcode duration 60 -> 36.
6. [Harmless] Shilellagh: create item opcode amount 0 -> 1.
7. [Minor] Strength of Stone: misses protection from druid version of Gust of Wind.
8. [Harmless] Good Berry: power of opcodes 2 -> 1.
9. [Harmless] Cause Light Wounds: power of opcodes 4 -> 1.
10. [Major, Implementation] Sunscorch: change documentation to explicitly mention that a save vs. spell (not vs. breath, as rolled to halve the damage) will avoid blindness. Use EE features of damage opcode.
11. [Major] Regenerate Light Wounds: resist_dispel -> 3. Only headers 7 and higher have Display String "Regenerating" -- added missing opcodes.
12. [Minor] Obscuring mist: remove spurious 284 opcode with probability 0. resist dispel -> 2 for 0 and 164 opcodes, as the penalties are from critters being inside the cloud and thus not blocked by mr or dispellable. Fixed bam.

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
13. [Harmless] Cause Moderate Wounds: power 0 -> 2.
14. [Harmless] Animal Summoning II: summoning eff power 8 -> 0.
15. [Major] Draw upon Divine Might: remove cast spell dvwinded. Winded at the end of DuDM is undocumented, a relic of earlier SR versions.

### Level 3.

1. [Harmless] Animate Dead: summoning eff power 3 -> 0 for foolish consistency.
2. [Implementation] Call Lightning: use EE features of damage opcode.
3. [Major] Dispel Magic: same fixes as wizard's remove magic.
4. [Implementation] Glyph of Warding: use EE features of damage opcode.
5. [Minor] Break Enchantment: add remove charm icon opcodes. Add to description that it also removes petrification.
6. [Harmless] Remove Paralysis: delete spurious prevent portrait opcode.
7. [Harmless, Implementation] Holy Smite: first header has a spurious Use Eff mask_evil opcode. Holy Smite no longer targets only evils. Use EE features of damage opcode.
8. [Major, Implementation] Unholy Blight: first header has a spurious Use Eff mask_good opcode. Unholy Blight no longer targets only goody two-shoes. Base thac0 penalty, Lighting Effects and Play Sound opcodes lack saves. All the save penalty opcodes have an undocumented penalty -2 -- all the more because Holy Smite does not have this penalty for its blindness effect. Use EE features of damage opcode.
9. [Minor] Cure Serious Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
10. [Major] Gust of Wind: sleep opcode resist_dispel 0 -> 1. Sleep opcode gained special = 130 (Unconscious) for icon and strref. Fixed bam.

note(s):
* contrary to the druid version, it does *not* dispel insect swarms. It also has a *documented* penalty of -2 to the saves.

11. [Major, Implementation] Summon Insects: panic on failed save vs. poison is not referred to in the description. Added it to description. Use EE features of damage opcode -- there is a very minor functional change: originally, the damage opcode would hit twice if the critter failed the save and now, with the new implementation, it only hits once (whether the critter fails the save or not). This would mean less chances of disrupting spellcasting but since spellcasting failure is already set at 100%, this is moot. Uses Attack Nearest [247] opcode instead of fear. Most likely a bug, because: (1) probability is 0 (2) The next display opcode explicitly outputs "Panic" and (3) According to IESDP, 247 sets berserk status. Why would insects cause berserk? Changing it to panic. Play visual opcode duration 7 -> 6.
12. [Harmless] Animal Summoning III: summoning eff power 8 -> 0.
13. [Major] Contagion: aux spell had 5th disease opcode (slow) probability1 0. The opcode is undocumented, so it is deleted. Remove spurious protection from chaotic commands.
14. [Harmless] Cause Serious Wounds: opcode's power 4 -> 3.
15. [Harmless] Regenerate Serious Wounds: opcode's power 1 -> 3.
16. [Major] Spike Growth: resist_dispel of display portrait opcode inconsistent with corresponding damage opcodes. Same for movement penalty opcode.
17. [Minor] Storm Shield: add spell type protection against k1insect.

### Level 4.

1. [Minor] Cure Critical Wounds: add remove intoxicated icon opcode since the spell also cures intoxication.
2. [Harmless] Animal Summoning IV: summoning eff power 8 -> 0.
3. [Major] Free Action: first header repeated protection against spell spwm164, but missing protection against web so altering the resource in the opcode. Second and subsequent headers miss protection against spells wish slow, spin983 slow and web. All headers missing protection against strings 14650 = "Paralyzed" and 14668 = "Slowed". Remove spurious 240 opcodes.
4. [Minor] Neutralize Poison: misses protection against string 14662 = "Poisoned". Add remove poisoned icon opcode.
5. [Minor] Domination: misses opcode display string 8364 = "Dominated".
6. [Harmless] Call Woodland Beings: summoning eff power 4 and 8 -> 0.
7. [Minor] Farsight: errors in several opcode's power and resist_dispel.
8. [Major] Cloak of Fear: 324 opcode giving immunity on resist fear moved to aux spell.
9. [Major] Ice Sorm: fix duration of movement rate penalty 24 -> 7. Damage opcode has an undocumented +2 damage.
10. [Harmless] Regenerate Critical Wounds: power of opcodes 1 -> 4.

### Level 5.

1. [Harmless] Animal Summoning V: summoning eff power 8 -> 0.
2. [Implementation] Flame Strike: use EE features of damage opcode.
2. [Harmless] Raise Dead: use Eff does nothing (calls non-existent eff); removed.
3. [Major] True Seeing: immunity to blindness lacks the appropriate protection from icon and string opcodes. resist_dispel on the opcodes is also inconsistent: some are not dispellable, others like the delayed cast spell and aesthetic opcodes are, which makes no sense. Lacks protection from cleric's obscuring mist.
4. [Major] Chaotic Commands: first header lacks 8 opcodes that the other headers have. Several protection against string opcodes missing.
5. [Major] Greater Command: play visual effect power 1 -> 5. Play sound lacks a save and has wrong duration. Delayed cast spell opcode has wrong duration 9 -> 6. It also casts sppr512b instead of the correct sppr512a (one less round). Use EE features of sleep code.
6. [Minor] Mass Cure: add remove intoxicated icon opcode since the spell also cures intoxication.
7. [Harmless] Repulsion: one of the cast spell opcodes power 3 -> 5. Aux spell sppr515d play visual opcode 215 power 2 -> 5.
8. [Major] Insect Plague: mention that on a failed save vs. poison the victim panics for the round. Same issues as Summon Insects.
9. [Major] Mass Regeneration: power of opcodes 0 -> 5. Resist_dispel 0 -> 3.
10. [Severe] Animal growth: Use EFF opcode calls wrong eff. Aux spell dvpr525d has undocumented strength and constitution bonuses opcodes, with wrong duration to boot. Deleted. If the strength bonuses are to be kept then they must be documented; the constitution bonus does not make sense though, because the hps are already being raised.

### Level 6.

1. [Harmless] Aerial Servant: summoning eff power 8 -> 0.
2. [Harmless] Animal Summoning VI: summoning eff power 8 -> 0.
3. [Minor] Blade Barrier: clarified description by mentioning that only non-party members suffer damage.
4. [Harmless] Conjure Fire Elemental: summoning eff power 6 -> 0.
5. [Implementation] Fire Seeds: use EE damage opcode features on aux spell.
6. [Minor] Heal: add missing remove poisoned icon opcode.
7. [Minor] False Dawn: power corrections on opcodes of aux spell sppr609e.
8. [Major] Dolorous Decay: play visual effect has a save penalty -5 -> -4. Damage amount of disease was 2 -> 1. Second display icon has timing mode 4. It seems this should be timing 1. Mention in description it bypasses mr.
9. [Harmless] Physical Mirror: power of opcodes in aux spell sppr613d 1 -> 6.
10. [Major] Regeneration: the description explicitly mentions the possibility of dispellability but opcodes have resist_dispel 0 -> 3. Description fix that duration is 3 turns.
11. [Harmless] Banishment: same problems as wizard's Banishment. Fixed bam.
12. [Harmless] Conjure Air Elemental: summoning eff power 6 -> 0.
13. [Harmless] Conjure Earth Elemental: summoning eff power 6 -> 0.
14. [Harmless] Animate Skeleton Warrior: Use Eff resist/dispel 3 -> 2, power 0 -> 6.

### Level 7.

1. [Harmless] Summon Shambling Mound: summoning eff power 8 -> 0. Fixed bam.
2. [Harmless] Summon Death Knight: summoning eff power 7 -> 0.
3. [Severe] Nature's Beauty: removed undocumented slay opcode. Added display string "Charmed" opcode.
4. [Implementation] Fire Storm: use EE features of damage opcode.
5. [Minor] Symbol of Weakness: inconsistency between resist_dispel of disease and other opcodes. Given the description, we change other opcodes -> 2.
6. [Minor, Implementation] Sunray: use EE features of damage opcode. Resist_dispel of first cast spell opcode -> 2. Display string resist_dispel 0 -> 1.
7. [Major] Finger of Death: damage is 2d8 + level not 6d8 as advertized. Here I am going by the implementation instead of the docs.
8. [Minor] Chaos: Add display icon for Hold, Sleep and Berserk effects, and string for Hold.
9. [Minor] Holy Word: second visual opcode has probability1 0 while first has resist_dispel 2 -> 1. Added missing display icon for stun. Did *not* add display string opcodes.
10. [Major] Regeneration: description explicitly mentions the possibility of dispellability but opcodes have resist_dispel 0 -> 3. Description fix that duration is 3 turns.
11. [Minor] Greater Restoration: added missing remove portrait icons: entangled = 144, grease = 145, poison = 6, panic = 36, berserk = 4.
12. [Major] Unholy Word: second visual opcode has probability1 0 while first has resist_dispel 2 -> 1. Neither the display portrait icon for confusion nor the cast spell scale their maximum levels but rather stay constant (at 0 and 13 respectively). Auxiliary spell implementing slow has a spurious remove illusionary protections opcode and doubled k1#haste. Last play sound opcode, marking the end of slow has duration 0 -> 30.
13. [Harmless] Animal Summoning VII: summoning eff power 8 -> 0.
14. [Major, Implementation] Creeping Doom: mention that on a failed save vs. poison at -2 the victim panics for the round. Same issues as Summon Insects.
15. [Major, Implementation] Earthquake: use EE damage opcode features. Sleep opcode gained icon = 130. Save penalty of second tremor -2 -> -4 and of third tremor 0 -> -2.
