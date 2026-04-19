# AllTheModium on NeoForge 1.21.1 — Verification (April 2026)

_Layer-2 verification (F2) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstreams A + B (questbook Chapter 8) + G._

## 1. Current version

- Latest: **`allthemodium-3.0.0_mc_1.21.1.jar`** released Jan 16, 2026.
- Prior stable: 2.9.6 / 2.9.3.
- NF 1.21.1 is the **actively maintained branch** (used by ATM10).

## 2. Dimension tier-gate / progression

- **Allthemodium ore** spawns in the Overworld deep layers → craft Allthemodium tools/armor.
- **Vibranium**: found in the **Nether** (in vanilla crimson/warped forests — keep these intact when picking Nether overhaul).
- **The Other**: Vibranium-tier portal — built like a Nether portal but frame is **Vibranium blocks**, lit with a **Piglich Heart** (dropped by **Piglich** miniboss in the Nether). Hostile dimension; flight recommended; contains **Unobtainium Smithing Templates** and Unobtainium ore.
- **The Beyond**: endgame void-skyblock — **2×2 water pool surrounded by flowers, throw a diamond in to form the portal**. Requires Unobtainium-tier gear to survive.

This is the spine for Questbook Chapter 8 (ATM access).

## 3. Companion / addon mods (NF 1.21.1)

- **No dedicated "Unobtainium/Vibranium/Allthemodium Extras" mods exist** on CurseForge/Modrinth for 1.21.1.
- ATM10 (the All The Mods 10 pack itself) bundles most extension content via KubeJS/datapack recipes.
- Real addons that extend ATM materials:
  - **All The Arcanist Gear** — ATM-tier spellbooks
  - **Enchanted: Wands & Tomes**
  - **Iron Furnaces** — ATM furnace tiers (note: open NeoForge bug #188)
  - **Silent Gear** — built-in ATM integration

## 4. Create ecosystem integration

- **No first-party Create recipes ship in Allthemodium.**
- Integration in ATM10 is handled via **pack-level KubeJS/datapack recipes** (Create crushing/milling/mixing for ATM ores).
- **Create: Numismatics has no direct ATM hooks** — custom recipes required to tie coins to ATM materials.
- **Adds to Workstream B+G work queue**: Create-mill recipes for Allthemodium / Vibranium / Unobtainium ores; optional Numismatics recipes for ATM-material trade goods.

## 5. Conflicts / performance notes

- **Sinytra Connector**: ATM ships native NeoForge — Connector not needed for ATM itself. Known Connector gotcha: mods marked Fabric+Forge but not NeoForge get rejected (not an ATM issue).
- **Distant Horizons**: ATM dimensions work with DH on 1.21.1, but **The Beyond's void terrain causes LOD artifacts**. **Disable DH per-dimension for The Beyond.**
- **Performance**: Piglich spawning + large ore vein scans are the usual hotspots. Tune `piglich` config and worldgen frequency in big packs.

## 6. Decision

**Locked into the Create Remastered stack.** Confirmed Vibranium + Piglich preserved by Incendium (see report 14), so Nether dimension overhauls don't break ATM progression.

## Sources

- [Allthemodium on CurseForge](https://www.curseforge.com/minecraft/mc-mods/allthemodium)
- [Allthemodium 3.0.0 / 2.9.6 / 2.8.x files (NF 1.21.1)](https://www.curseforge.com/minecraft/mc-mods/allthemodium/files/all)
- [All The Guides — Allthemodium (progression)](https://allthemods.github.io/alltheguides/atm9/allthemodium/)
- [All the Mods 10 Dimensions Guide](https://all-themods.com/dimensions/)
- [GPortal ATM10 Wiki](https://www.g-portal.com/wiki/en/minecraft-all-the-mods-10/)
- [All The Arcanist Gear (ATM-tier addon)](https://www.curseforge.com/minecraft/mc-mods/all-the-arcanist-gear)
- [Iron Furnaces ATM compat bug #188](https://github.com/Qelifern/IronFurnaces/issues/188)
- [Sinytra Connector compat archive 1.21.1](https://connector.sinytra.org/compatibility/archive/2.0.0-beta.8+1.21.1)
- [Create: Numismatics](https://modrinth.com/mod/numismatics/versions)
