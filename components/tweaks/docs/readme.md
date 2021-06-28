# A. Tweaks.

Contrary to the patches component, tweaks are grouped and applied in bulk, not per-spell. If there is some particular tweak you do not want, just edit the tweaks.2da file and turn the value in the second column from 1 to 0.

## A. 1. Standardize Summoning Duration.

This tweak standardizes duration of summons, both Wizard's Monster Summoning and Druid's Animal Summoning, to 5 turns. In SR they have a duration of 2 to 3 turns so this tweak increases the time they stay around.

## A. 2. Wizard armor spells.

First, in the current SR implementation, the wizard armor spells can be used concurrently. This, of itself, is mostly moot because the ac opcodes set ac so they do not stack, which is why we do not make this a fix. But the extra bonuses of Ghost Armor and Spirit Armor do stack, so we patch in non-stackage. Second, the fact is that Mage Armor is too good, so we drop the ac 3 at level 12 and re-level the other two headers to levels 5 and 9.

## A. 3. Non-combat triggers and contingencies.

This tweak sets the non-combat flag on all trigger and contingency spells, so that you can no longer cast these spells mid-combat. This will hopefully stop some of the more egregious douchebaggery, especially with chain contingency.