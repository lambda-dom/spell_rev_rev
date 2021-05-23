# spell_rev_rev README

Revisions (patches and tweaks) for Spell Revisions.

## A. Installation.

First, download and install the gr-lib WeiDU library -- consult the [readme](https://github.com/lambda-dom/gr-lib) for more details. Then install the mod just like you would install any other WeiDU mod.

## B. Requirements.

[Spell Revisions](https://github.com/Gibberlings3/SpellRevisions) is obviously necessary; use the latest v4 beta 18 release. The mod uses EE features, so it does not work on classical editions or platforms like BGT that rely on it. It was also tested with the game patched to version v2.6, so if you have an earlier version you might see some differences.

As far as load order, it should be installed right after Spell Revisions. The mod was also tested with all SR components installed with the exception of Update NPC Schoolbooks, which is irrelevant as far as this mod is concerned, and Spell Protections protect against AoE spells. I do not use use this component myself, but I expect it should not introduce any major complications -- if you find that it does, patches are welcomed.

The mod is pure WeiDU, so it should be safe from operating system variations. While it was tested in linux, using a case-insensitive ext4 partition -- see [here](https://www.gibberlings3.net/forums/topic/28516-the-linux-users-guide-to-installing-mods-on-the-enhanced-editions/) for details on how to set it up -- it should work just fine on Windows or MacOS. If it does not, patches are welcomed.

## C. Components.

There are currently, two components in this mod.

### C. 1. Patches.

The patches component patches the spells introduced by Spell Revisions. User-convenient documentation for the patches applied can be found in the [readme](components/patches/docs/readme.md), while a list of to-dos can be found in the [todo](components/patches/docs/todo.md) file. Consult this file before submitting an issue or patch.

### C. 2. Tweaks.

The tweaks component tweaks the spells introduced by Spell Revisions. The difference between a tweak and a patch can be blurry; suffice to say that we have been as conservative as possible when it comes to patching SR. Any change of gameplay functionality counts as a tweak and is in this component. The changes tend to be fairly low key, in part because SR is already just fine as it is, in part so as not to needlessly confuse SCS. Most of them are borrowed almost verbatim from [SR Revised](https://www.gibberlings3.net/forums/topic/29618-sr-revised-v13200-2020-august-22nd). For fuller documentation on each tweak consult the [readme](components/tweaks/docs/readme.md).

## D. Contributing.

If you want to contribute, an overview of the code is useful.

### D. 1. Patches component.

The patches component is designed to only patch spells that have entries in the spell.ids file. Each such patched spell has an entry in the patches table .2da file. The columns of the table are the symbolic name, a patch flag (that the user can toggle for fine-grain selection of patches) and a tra reference for the new spell description. The code iterates through the table, and for each entry, if the patch flag is enabled it copies the new description if any (e.g. tra reference is different from 0) and applies the patch function with the same spell symbolic name. The function is found in the relevant tpa file, organized by arcane/divine and level for ease of maintenance. Everything is done in the patch function, including the copying of any needed resources, patching of auxiliary resources, etc.

## E. Tools used.

* [WeiDU](https://github.com/WeiDUorg/weidu)
* [Near Infinity](https://github.com/Argent77/NearInfinity)
* [Visual Studio Code](https://code.visualstudio.com/)
* [BGforge MLS VScode extension](https://github.com/BGforgeNet/VScode-BGforge-MLS)