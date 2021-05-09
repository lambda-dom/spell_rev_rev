# spell_rev_rev README

Revisions (patches and tweaks) for Spell Revisions.

## A. Installation.

First, download and install the gr-lib WeiDU library -- consult the [readme](https://github.com/lambda-dom/gr-lib) for more details. Then install the mod just like you would install any other WeiDU mod.

## B. Requirements.

[Spell Revisions](https://github.com/Gibberlings3/SpellRevisions) is obviously necessary; use the latest v4 beta 18 release. The mod uses EE features, so it does not work on classical editions or platforms like BGT that rely on it. It is also tested with the game patched to version v2.6.

The mod is pure WeiDU, so it should be safe from operating system variations. While it was tested in linux, using a case-insensitive ext4 partition -- see [here](https://www.gibberlings3.net/forums/topic/28516-the-linux-users-guide-to-installing-mods-on-the-enhanced-editions/) for details on how to set it up -- it should work just fine on Windows or MacOS. If it does not, patches are welcome.

## C. Contributing.

If you want to contribute patches, an overview of the code is useful. The mod is designed to only patch spells that have entries in spell.ids. Each such patched spell has an entry in patches.2da with the symbolic name, a patch flag (that the user can toggle for fine-grain selection of patches) and a tra reference for the new spell description. The code iterates through the table, and for each entry, if patch is enabled copies the new description if any (e.g. tra reference is different from 0) and applies the patch function with the same spell symbolic name. The function is found in the relevant tpa file, organized by arcane/divine and level for ease of maintenance. Everything is done in the patch function, including the copying of any needed resources, patching of auxiliary resources, etc.

## D. Tools used.

* [WeiDU](https://github.com/WeiDUorg/weidu)
* [Near Infinity](https://github.com/Argent77/NearInfinity)
* [Visual Studio Code](https://code.visualstudio.com/)
* [BGforge MLS VScode extension](https://github.com/BGforgeNet/VScode-BGforge-MLS)