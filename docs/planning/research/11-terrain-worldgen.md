# Midengard / Middgard / Middangard — Identification + Wide-Vista Worldgen Stack for NF 1.21.1

_Layer-2 re-research (R6) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstream A._

## 1. Midengard identification — CONFIRMED (but it's two projects)

The user's "Midengard" is almost certainly one of two Midgard-styled worldgen mods on CurseForge, both using the Old-English "Middangeard" root:

| Project | CurseForge slug | Core approach | Status |
|---|---|---|---|
| **Middgard** | `/mc-mods/middgard` | Overhauls vanilla biomes + forests; 13 new biomes; custom large trees (pine/aspen/maple); rides on **Tectonic** for base terrain shape | Beta; currently **1.20.1 Forge** (midgard-1.0-1.20.1.jar) |
| **Middangard** | `/mc-mods/middangard` | Same description text — appears to be the same author's relaunch/rename; latest file `midgard-0.1-1.20.1.jar` | Beta 0.1, recent (late March 2026) |

**Both are 1.20.1 Forge-only right now — neither has a NeoForge 1.21.1 build.** The description on both is identical ("Midgard aims to change the way biomes generate… 13 new biomes… three new wood types… Tectonic provides the base terrain shape although this may change"). Given the Tectonic dependency and the author's note that Tectonic-dependence "may change," Middgard/Middangard is effectively a **Tectonic biome overlay** at this stage — so for your pack you can approximate it by running **Tectonic + a biome overlay** on NF 1.21.1 until the author ports. **Release: very recent, still 0.1 beta.** Wide-vista aesthetic comes directly from the Tectonic base.

## 2. Comparison — wide-vista worldgen on NeoForge 1.21.1

| Mod | NF 1.21.1 | Style | DH compat | Aeronautics fit | Density / spacing |
|---|---|---|---|---|---|
| **Tectonic** v3.0.19 | Yes (March/July 2025 builds on CF) | Continent-scale landmasses, tall mountain ranges (Y>300), deep oceans, underground rivers | Excellent — used in the "Distantly Optimized" DH pack | Good: continents give huge flat-ish plateaus for airship pads, but high peaks mean ceiling choices matter | Vanilla structure spacing preserved; at 192-block empire radius several continents will read as one vista |
| **Terralith** 2.6.1 | Yes (NeoForge build March 2026) | ~100 vanilla-block biomes, modest terrain shaping | Excellent (explicitly shown in DH docs/gallery) | Very good — gentler relief than Tectonic | Vanilla spacing; adds its own minor structures |
| **Lithosphere** 1.3 | Yes (Forge + 3 other loaders incl. NeoForge, MC 1.21.1) | Cinematic mountains, large coastal beaches/cliffs, bigger biomes | Works but it replaces default `noise_settings`, so **stacking with other overworld worldgen datapacks breaks** | Great airship visuals (big continents + archipelagos) | Vanilla spacing; solo use recommended |
| **WWOO (William Wythers)** v2.5.5+ | Yes on NeoForge for 1.21.1-1.21.8; v2.6.4 for 1.21.11 | Vanilla-friendly biome overhaul with sub-biomes | Good (vanilla blocks, no novel noise) | Good | Vanilla spacing |
| **Oh The Biomes We've Gone** 2.1.2+ | Yes, NeoForge 1.21.1 | 50+ biomes, BYG successor | Works with DH (both on 1.21.1 NF) | Mixed — some biomes have very tall/dense canopy | Adds BYG-style structures, denser than vanilla |
| **Biomes O' Plenty** | Yes | Classic biome pack | Fine | Fine | Vanilla |
| **TerraForged** | **No NF 1.21.1** stable — only "Reterraforged" fork has 1.20.1/1.21.5 builds; no 1.21.1 NeoForge | — | — | — | — |
| **Lithosphere-adjacent "Larion"** | 1.21.x build exists | Newer competitor, smaller | Untested widely | Untested | — |
| **Incendium / Nullscape** | Yes (Stardust Labs, data-pack-in-mod) | Nether / End overhauls | Irrelevant to overworld vistas but complete the pack | N/A | N/A |
| **Middgard/Middangard** | **No** (1.20.1 Forge only) | Tectonic-based biome overlay | — | — | — |

## 3. Recommendation

**Primary stack for NF 1.21.1 + DH + Create: Aeronautics + compact empires:**

- **Tectonic 3.0.19** (the wide-vista backbone — this is what Middgard sits on anyway)
- **Terralith 2.6.x (NF 1.21.1)** — biome variety on top; DH-proven
- **Structory + Towns & Towers** — landmark structures at civilised spacing for empire territory markers
- **YUNG's Better Dungeons / Better Strongholds / Better Desert Temples** — for 1.21.1 NeoForge; quality beats quantity for a 192-block footprint
- **Incendium + Nullscape** — round out Nether/End
- **Distant Horizons 2.3.4-b or 2.3.6-b (1.21.1 neo/fabric)** — current stable lines

If you want the exact Middgard aesthetic, watch the Middgard/Middangard CurseForge pages for a NeoForge 1.21.1 port; meanwhile Tectonic + Terralith is a faithful visual substitute.

**Alt pick:** swap Tectonic for **Lithosphere** if you want tighter, more cinematic coastlines for airship skyboxes — but run it **alone** on the overworld noise side (it replaces `noise_settings`).

## 4. Risks / performance

- **Lithosphere** is known-clashy with other overworld-noise datapacks — pick one overworld noise mod, not two.
- **Alex's Caves** breaks DH LOD rendering unless `biome_ambient_light_coloring=false` (not in your list but worth noting since it often gets paired).
- **The Lost Cities** is documented DH-incompat unless you disable DH worldgen — skip for an airship pack.
- **OTBWG** has tall/dense canopy biomes that will irritate airship docking; cherry-pick or disable those biomes.
- **Tectonic** + tall mountains (>Y300) + Aeronautics: raise world ceiling headroom where possible and prefer continent interiors as empire spawn zones; peaks are scenery, not landing pads.
- **~192-block empire footprint:** Tectonic's continent-scale means several empires will share one continent — good for "shared vista" framing, but vanilla village spacing (~32 chunks min) can leave some empires with zero villages in 192 blocks. Add **Towns & Towers / Repurposed Structures** and tune `min_distance` down in their configs.

## 5. Open questions

- Does the Middgard/Middangard author intend a NeoForge 1.21.1 port? (No public roadmap found; CurseForge page hasn't been fetched directly — rate-limited.)
- Is Middgard and Middangard the same author (looks like a rename)? CF pages are near-identical but filenames differ.
- Confirm Create: Aeronautics 1.0.2 for 1.21.1 NeoForge runs cleanly with Tectonic's deep oceans (deepslate-layer oceans may affect anchor physics) — untested in public docs.
- Terralith 2.6.1 NF build on 1.21.1 specifically (vs 1.21.5+) — GitHub releases should confirm; the issue Stardust-Labs-MC/Terralith#86 about NeoForge 1.20.2+ false-flag is resolved in 2.6.x but worth sanity-checking.

## Sources
- [Middgard — CurseForge](https://www.curseforge.com/minecraft/mc-mods/middgard)
- [Middangard — CurseForge files](https://www.curseforge.com/minecraft/mc-mods/middangard/files/all)
- [Middangard midgard-0.1-1.20.1.jar](https://www.curseforge.com/minecraft/mc-mods/middangard/files/7824272)
- [Tectonic v3.0.19 NeoForge 1.21.1](https://www.curseforge.com/minecraft/mc-mods/tectonic/files/7367622)
- [Tectonic — CurseForge](https://www.curseforge.com/minecraft/mc-mods/tectonic)
- [Tectonic GitHub (Apollounknowndev)](https://github.com/Apollounknowndev/tectonic)
- [Terralith — Stardust Labs](https://www.stardustlabs.net/terralith)
- [Terralith releases](https://github.com/Stardust-Labs-MC/Terralith/releases)
- [Terralith #86 NeoForge compat](https://github.com/Stardust-Labs-MC/Terralith/issues/86)
- [Lithosphere — CurseForge](https://www.curseforge.com/minecraft/mc-mods/lithosphere)
- [William Wythers' Overhauled Overworld — Modrinth](https://modrinth.com/mod/wwoo)
- [Oh The Biomes We've Gone 2.1.2 NeoForge](https://modrinth.com/mod/oh-the-biomes-weve-gone/version/2.1.2-NeoForge)
- [Distant Horizons 2.3.4-b 1.21.1 neo/fabric](https://modrinth.com/mod/distanthorizons/version/2.3.4-b-1.21.1)
- [Distant Horizons 2.3.6-b 1.21.1](https://modrinth.com/mod/distanthorizons/version/2.3.6-b-1.21.1)
- [Distantly Optimized modpack (Tectonic + DH)](https://modrinth.com/modpack/distantly-optimized)
- [Create + Distant Horizons modpack](https://www.curseforge.com/minecraft/modpacks/create-distant-horizons)
- [DH + Lost Cities compat issue #693](https://gitlab.com/jeseibel/distant-horizons/-/issues/693)
- [DH + Still Life worldgen incompat](https://www.answeroverflow.com/m/1403594941850189924)
- [Incendium — Stardust Labs Wiki](https://stardustlabs.miraheze.org/wiki/Incendium)
- [Nullscape — Stardust Labs Wiki](https://stardustlabs.miraheze.org/wiki/Nullscape)
- [Create Aeronautics 1.0.2 1.21.1](https://modrinth.com/mod/create-aeronautics/version/HY8u0JqC)
- [Reterraforged (TerraForged successor)](https://www.9minecraft.net/reterraforged-mod/)
