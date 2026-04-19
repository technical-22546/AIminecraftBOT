# Create Remastered — Nether/End Overhaul Verification (NF 1.21.1, April 2026)

_Layer-2 verification (F3) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstream A._

## Stardust Labs core pair (recommended)

**Incendium** — `v5.4.12` for NeoForge 1.21.x, released March 29, 2026. Mature, server-side, **8 custom Nether biomes + 9 structures using only vanilla blocks**. Stable on NF 1.21.1.

**Nullscape** — `v1.2.19` released March 30, 2026 (NF 1.21.x). End height extended to 384; desolation-preserving terrain. Stable and actively maintained. Official **Stellarity × Nullscape compat datapack** exists if you later layer Stellarity.

Both are worldgen-only / datapack-style mods, so DH compatibility is essentially "it just works" — DH generates LODs from whatever chunk gen produces. **No known DH-specific breakage reports.**

## Nether alternatives (evaluated, not chosen)

- **BetterNether Neoforge** (Reijin2312 unofficial port) — `v21.0.19` released March 14, 2026 for NF 1.21.1. Active, but unofficial port adds risk for Release. **Known historical conflict with Incendium**: Incendium's dimension JSON overrides the Nether biome source. Solution requires datapack edit or **Nether Biomes Compat** mod. **Do not ship both together.**
- **Even Better Nether** — NF 1.21.1 builds (1.1.1 and 1.3.0 jars on CF). Lighter-weight, fewer worldgen collisions.
- **Nether Depths Upgrade** — `v3.1.8` on NF 1.21-1.21.1 (April 2025). Additive only (lava fish, potion, enchant). **Safe alongside Incendium** — optional add.
- **Blazing Neutrons** — no hits, likely not a real mod or discontinued.
- **YUNG's Better Nether Fortresses** `v3.1.5` NF 1.21.1 — structure-only, **stacks cleanly with Incendium** — optional add.

## End alternatives (evaluated, not chosen)

- **Stellarity** — NF 1.21.1 builds as of April 11, 2026 (v4.2.0+mod). **23 biomes including reworked vanilla.** Active. Conflicts with Nullscape unless Stellarity × Nullscape compat datapack is used.
- **End Remastered** — progression/ritual mod (16 custom eyes), **not a worldgen overhaul** — non-conflicting with Nullscape if you ever want a ritual layer.
- **Extended End** — Feb 2025, NF 1.21.1-1.21.4. Adds End ores/wood; additive.
- **End: Rebellion / Enhanced End / OuterEnd** — no hits on NF 1.21.1 as of April 2026.

## AllTheModium / Create friction analysis

- **Vibranium ore is tagged to Crimson Forest and Warped Forest.** Piglich spawning likewise keys off Warped Forest.
- **Incendium keeps vanilla crimson/warped biomes available** — it adds 8 new biomes alongside rather than replacing the two forests. So **Vibranium and Piglich generation remains intact** in practice. This is widely reported working in ATM-adjacent packs.
- **BetterNether** historically shrinks the vanilla forest footprint more aggressively, which has caused thin Vibranium/Piglich availability reports — **another reason to pick Incendium over BetterNether**.
- **Create itself has no direct Nether/End friction** — only relevant Create angle is ensuring player access to Nether wastes for netherrack-based kinetic farms, which Incendium preserves.

## Decision

**Lock Incendium + Nullscape for Create Remastered Release.**

- Maturest, most actively maintained pair on NF 1.21.1 (both updated within the last month).
- Datapack-style → DH/perf overhead is zero.
- Preserves vanilla crimson/warped forests → ATM Vibranium + Piglich remain accessible.
- Modpack-industry default for a reason.

**Do NOT additionally bundle:**
- BetterNether (Incendium conflict)
- Stellarity (Nullscape overlap without the compat addon)

**Safe optional adds:**
- Nether Depths Upgrade (additive lava fish/potion/enchant)
- YUNG's Better Nether Fortresses (structure-only)

**Falling back to vanilla Nether/End is unnecessary** — no blocker issues found for the monopoly theme.

## Sources

- [Incendium on Modrinth](https://modrinth.com/mod/incendium)
- [Incendium on CurseForge](https://www.curseforge.com/minecraft/mc-mods/incendium)
- [Nullscape on CurseForge](https://www.curseforge.com/minecraft/mc-mods/nullscape)
- [BetterNether Neoforge (Reijin2312 port)](https://www.curseforge.com/minecraft/mc-mods/betternether-neoforge)
- [BetterNether Neoforge GitHub](https://github.com/Reijin2312/BetterNether_Neoforge)
- [BetterNether x Incendium compat issue #406](https://github.com/paulevsGitch/BetterNether/issues/406)
- [Nether Biomes Compat](https://www.curseforge.com/minecraft/mc-mods/nether-biomes-compat)
- [Nether Depths Upgrade](https://modrinth.com/mod/nether-depths-upgrade)
- [YUNG's Better Nether Fortresses 1.21.1 NF](https://modrinth.com/mod/yungs-better-nether-fortresses/version/1.21.1-NeoForge-3.1.5)
- [Stellarity](https://www.curseforge.com/minecraft/mc-mods/stellarity)
- [Stellarity x Nullscape Compatibility](https://modrinth.com/datapack/stellarity-x-nullscape)
- [End Remastered](https://www.curseforge.com/minecraft/mc-mods/endremastered)
- [Extended End](https://modrinth.com/mod/extended-end/version/0.9)
- [Distant Horizons 2.2.0-a 1.21.1 NF/Fabric](https://modrinth.com/mod/distanthorizons/version/2.2.0-a-1.21.1)
- [AllTheModium ores guide (SiriusMC wiki)](https://wiki.siriusmc.net/books/modpack-guides-and-tutorials/page/allthemodium-ores-guide-atm10)
- [ATM-6 Vibranium spawn issue #183](https://github.com/AllTheMods/ATM-6/issues/183)
