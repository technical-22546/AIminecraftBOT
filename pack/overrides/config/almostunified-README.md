# AlmostUnified config — starter notes

## Purpose

Unify common cross-mod materials (iron/copper/tin/lead/zinc/osmium/etc. ingots, dusts, nuggets, plates, gears, rods, ores, raw materials, wires) to **one canonical item per tag** across our tech-mod ecosystem. Reduces JEI/EMI clutter and aligns recipes that would otherwise resolve inconsistently.

**Critical preserve rule:** ATM materials (Allthemodium, Vibranium, Unobtainium, Vulpus) and Immersive Ores items **must NOT be unified** — they're the late-game tier separation. Unifying them collapses your monopoly-theme progression.

## Files shipped

- `pack/overrides/config/almostunified-common.json` — primary config

## Schema caveat

**AlmostUnified's config schema has shifted across versions** (1.20.1 → 1.21.1 NF port). Field names may differ in your installed version:

- `mod_priorities` may be `modPriorities` or `tag_owner_order` depending on version
- `ignored_tags` may be `ignoredTags` or `tagOwnerOrder` exclusions
- Loot/duplicates flags vary

**Action:** after first install, compare this file to AU's actual config schema (check `config/almostunified-common.json` AU generates on first boot, or the source repo for current field names). Migrate field names if needed; the *intent* is the structure that matters.

## Mod priority order rationale

1. **minecraft** — vanilla wins where vanilla item exists (gold ingot, iron ingot, diamond, etc.)
2. **create** — for shared materials like brass (Create's brass is the canonical zinc-copper alloy in the pack identity)
3. **mekanism** — deepest material system, most-used item for tech
4. **immersiveengineering** — Gilded-Age industrial aesthetic, second canonical
5. **modern_industrialization** — modern-tier replacement for ingots/plates
6. **industrialforegoing**, **powah**, **thermal** (even though Thermal not shipping; entry as fallback if a Thermal-bridge mod sneaks in via a dep), **tconstruct** (also not shipping), **silent_gear**
7. **ae2**, **rftools**, **actuallyadditions**, **extreme_reactors** — last-priority

## Preserved (ignored) tags

ATM tag family — **never unify**:
- `c:ingots/allthemodium`, `c:ingots/vibranium`, `c:ingots/unobtainium`, `c:ingots/vulpus`
- Same pattern for ores, dusts, nuggets, plates, gems, raw_materials

Special:
- `c:ingots/enderium_atm` — if Immersive Ores' Enderium uses a different tag than Thermal's (Thermal not shipping anyway)
- `create:zinc`, `create:brass` — Create's specific bronze/zinc tagging stays distinct

## Preserved (ignored) item IDs

Pattern matches:
- `allthemodium:.*` — all ATM items
- `immersive_ores:.*` — all Immersive Ores items (until you resolve the Vibranium tag clash via KubeJS in v0)

After KubeJS resolves the Vibranium clash, you can remove the `immersive_ores:.*` ignore pattern and let AU unify Immersive Ores's other ores (Vulpus, Enderium) against the right tag owners.

## Tags unified

Common materials only:
- Ingots, nuggets, dusts, plates, gears, rods of: iron / gold / copper / tin / lead / zinc / osmium / silver / nickel / aluminum / bronze / steel / uranium / platinum / iridium / cobalt / electrum / invar / constantan / enderium / signalum / lumium
- Gems: diamond / emerald / quartz / lapis / ruby / sapphire / peridot / amethyst
- Ores: iron / gold / copper / tin / lead / zinc / osmium / silver / nickel / aluminum / uranium
- Raw materials, wires: iron / gold / copper / tin / lead / zinc / silver / nickel / aluminum

## Operational flags

- `stone_variants: false` — DO unify cobblestone variants → vanilla cobblestone (saves JEI mess)
- `loot_unification: true` — apply unification to loot tables (chest spawns, mob drops)
- `duplicates_unification: false` — leave duplicates alone (safer for first boot)
- `inherit_tags_from_dropped_items: true` — preserve tag-membership of items being unified-away
- `hide_in_jei: true` — non-canonical variants hidden from JEI/EMI

## Verification after first boot

1. Open EMI/JEI in-game
2. Search for "iron ingot" — should see ONE entry, not 6
3. Search for "vibranium" — should see TWO entries (ATM Vibranium + Immersive Ores Vibranium), distinct
4. Search for "allthemodium" — should see ONE ATM canonical
5. Search for "create:brass" — should see Create's brass as canonical

## Tune cadence

- v0 first boot: this config as starting point
- After 30-60 min of in-pack play: check JEI/EMI for residual duplicates; add missing tags to `tags`
- Before Release: confirm no ATM materials accidentally unified, no Immersive Ores items missing post-Vibranium-remap

## Reference

- [AlmostUnified on CurseForge (current NF 1.21.1)](https://www.curseforge.com/minecraft/mc-mods/almostunified)
- [AlmostUnified GitHub (schema reference)](https://github.com/AlmostReliable/almostunified)
