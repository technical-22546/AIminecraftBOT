# ATM10 Blueprint + 4-Mod Verification (2026-04-18)

_Layer-2 research (R-ATM10). Summary folded into `../warm-iverson.md` Workstreams A + G._

## 1. Verification Results (NF 1.21.1)

| Mod | Author | Status | Latest Version | Notes |
|---|---|---|---|---|
| **Modern Industrialization** | azzamaran (Team) | **SHIPS** — native NeoForge (Fabric dropped post-1.20.4) | **v2.4.2** (22 Mar 2026) | No fork needed. Official addons on NF 1.21.1: MI Sound Addon (1.2.0), MI Tweaks, **Industrialization Overdrive** (1.8.1, machine expansion), **Extended Industrialization** (Create↔MI bridge, present in ATM10). No standalone "MI Compat: AE2" mod — Applied Mekanistics/Applied Flux/Soulplied Energistics cover that. |
| **Integrated Dynamics suite** | kroeser / CyclopsMC | **ALL 5 SHIP** | ID 1.26.1 (also 1.30.5-1519) / Tunnels 1.8.28 / Terminals 1.6.15 / Crafting + Scripting all on 1.21.1 | Requires Cyclops Core + Common Capabilities (2.11.1/2.11.3). **Known issue: AE2 Spatial IO + ID crash (GH #1410)** — disable spatial transport of ID networks. |
| **Extreme Reactors** | ZeroNoRyouki | **SHIPS** | ExtremeReactors2-1.21.1-2.4.28 (17 Dec 2025) | Drop-in swap for Bigger Reactors. Native FE power taps; bridges Create/Mek/MI without extra compat. |
| **Easy Villagers** | henkelmax | **SHIPS** | 1.21.1-1.1.22 (Modrinth) / 1.21.1-1.1.39 (CurseForge — latest) | Major MineColonies QoL — carry villagers in hand/cart. No Mineflayer conflicts. |

**All four verified shipping. No forks required.**

## 2. ATM10 Blueprint (~450 mods, categorized)

Sourced from `AllTheMods/ATM-10` repo `MOD_ISSUES.md`, pack version 6.6 (2026-04-07).

### Tech / Automation / Power
Mekanism (+ Generators, Tools, Covers, More Machine), Modern Industrialization, Extended Industrialization, Industrialization Overdrive, Oritech, Create, Create Crafts & Additions, Create: Aquatic Ambitions, Create: Bells & Whistles, Create: Dragons Plus, Create: Enchantment Industry, Create: Hypertubes, Immersive Engineering, PneumaticCraft: Repressurized, Industrial Foregoing (+ Souls, MIFA), Railcraft Reborn, Steve's Carts, **Draconic Evolution**, Brandon's Core, Powah! (Rearchitected), Flux Networks, Modular Machinery Reborn (+ Ars, Mekanism), Productive Metalworks, Productive Bees, Productive Trees, Modular Bees, Ender IO, RFTools (Base/Builder/Power/Storage/Utility), Compact Machines, Hyperbox, Super Factory Manager, Laser Bridges & Doors, LaserIO, Modern Dynamics, Modular Routers, Pipez, Mekanistic Routers, Ranged Pumps, Generator Galore, Iron Jetpacks, Extreme Reactors.

### Storage / Logistics
Applied Energistics 2 (+ Wireless Terminals, JEI, Network Analyser, Import/Export Card, Crafting Tree, Infinity Booster), Advanced AE, Applied Flux, Applied Mekanistics, ExtendedAE, Expanded AE, Immersive Energistics, Ars Énergistique, ME Requester, MEGA Cells, EnderDrives, Extra Disks, ExtraStorage, Polymorphic Energistics, Soulplied Energistics, Refined Storage (+ integrations), Refined Types, Sophisticated Storage (+ in Motion, Create integration), Sophisticated Backpacks (+ Create integration), Sophisticated Core, Functional Storage, Pocket Storage, DimStorage, Ender Storage, Entangled, Cable Tiers, Item Collectors, Trash Cans, Tool Belt, Shrink., Tempad.

### Magic — **STRIKE ALL per no-magic policy**
Ars Nouveau, Ars Additions, Ars Controle, Ars Creo, Ars Elemancy, Ars Elemental, Ars Ocultas, Ars Polymorphia, Ars Technica, Ars Unification, All The Arcanist Gear, All the Wizard Gear, StarbuncleMania, Iron's Spells 'n Spellbooks, Iron's Gems 'n Jewelry, Occultism (+ KubeJS), EvilCraft, Forbidden and Arcanus, Mahou Tsukai, Theurgy (+ KubeJS), Roots Classic, Nature's Aura, Relics (+ Artifacts Compat), Reliquary Reincarnations, **Apothic Enchanting** (keep other Apoth modules), Gateways to Eternity, Hostile Neural Networks, Blue Flame Burning, Potions Master, Not Enough Glyphs.

### Worldgen / Dimensions / Structures
Allthemodium, **The Twilight Forest, The Aether, The Undergarden, Eternal Starlight, Deeper and Darker, The Bumblezone** — off-theme dims, strike. **Keep:** IceAndFire CE (strike — dragon-magic), L_Ender's Cataclysm (strike — boss-magic), Nullscape, Regions Unexplored, OTBWG, OTYG, Terralith (via Lithostitched), Formations (Overworld/Nether), Repurposed Structures, Structory (+ Towers), Dungeon Crawl, Explorify, MES/MNS/MSS/MVS, Mo' Structures, Illager Warship, ChoiceTheorem's Overhauled Village, Enderman Overhaul, Creeper Overhaul.

### Colony / Villagers — keep all
MineColonies, Structurize, Domum Ornamentum, BlockUI, MultiPiston, Byzantine Styles Pack, Stylecolonies, **Easy Villagers**, No Villager Death Messages.

### Agriculture / Food — keep most
Farmer's Delight, Pam's HarvestCraft 2 (Crops/Food Core/Food Extended/Trees), Cooking for Blockheads, Farming for Blockheads, Botany Pots (+ Trees, Mystical Agri compat), **Mystical Agriculture, Mystical Agradditions, Mystical Customization — borderline (auto-farming may trivialize economy; keep with config tuning)**, Sushi Go Crafting, Storage Delight, Spice of Life: Carrot Edition, Mama's Herbs and Harvest, Mama's Merrymaking, Harvest with ease, Reap Mod, TreeHarvester, Wheat Feeder.

### Building / Decoration — keep all
Chipped, Chisel Reborn, Rechiseled (+ Chipped, Create), Macaw's Bridges/Doors/Fences&Walls/Furniture/Holidays/Lights/Paths/Roofs/Stairs/Trapdoors/Windows, Handcrafted, FramedBlocks, Supplementaries, Amendments, Bibliobiomes/Bibliocraft/Bibliowoods Legacy, MrCrayfish's Furniture Refurbished, Factory Blocks, Simply Light, Redstone Pen, Little Big Redstone, Construction Sticks, Cloud Glass, Connected Glass, ConnectedTexturesMod, Fusion, Glassential Renewed, Chroma Carvings, Everything is Copper, Camol, Rainbows!, Dyenamics (+ Friends), Showcase Item, OpenBlocks Elevator, Travel Anchors.

### Adventure / Combat / Gear
Silent Gear (+ Metalworks), Silent's Gems, Artifacts, Enigmatic Legacy-adjacent (Relics et al. — magic, strike), Baubley Heart Canisters, Cosmetic Armor Reworked, Cat Jammies, Jonn's Trophies, Equipment Compare, Colorful Hearts, Overloaded Armor Bar, Corail Tombstone, **Security Craft (redundant w/ FTB Chunks claims — strike)**, Mob Grinding Utils, Hardened Armadillos, Gravitational Modulating Additional Unit, Two-Handed Weapons (via Apoth), Target Dummy.

### QoL / UI / Maps — keep most
JourneyMap, Jade, JEI (skip if we choose EMI), Just Enough Archaeology, Just Enough Breeding, Just Enough Mekanism Multiblocks, Just Enough Professions, Just Zoom, Smithing Template Viewer, GuideME, Patchouli, Modonomicon, Waystones, Explorer's Compass, Nature's Compass, Oracle Index, Almanac Lib, Akashic Tome, Controlling, Inventory Essentials, Inventory Tweaks - ReFoxed, Mouse Tweaks, Crafting Tweaks, Crafting on a stick, KeyBind Bundles, KeybindsPurger, Rebind Narrator, Dark Mode Everywhere, Legendary Tooltips, Enchantment Descriptions, Searchables, AppleSkin, Polymorph, Clumps, AttributeFix, Better Advancements, Toast Control, Simple Backups, Crash Assistant, Crash Utilities, FancyMenu, PackMenu, Drippy Loading Screen, Bad Wither No Cookie Reloaded, Hey Berry SHUT UP, Let Me Despawn, Fireproof Boats, NetherPortalFix, Get It Together Drops!, Measurements, Prism.

### Performance / Libs / Shaders — keep all
Embeddium, Iris, Oculus-adjacent (implicit), ImmediatelyFast, FerriteCore, ModernFix, AllTheLeaks, Nolijium, Connectivity, ServerCore, Cobweb, Cupboard, Memory Settings, Model Gap Fix, FlickerFix, Load My F***ing Tags, FastFurnace, FastSuite, FastWorkbench, Monocle, Balm, Bookshelf, Placebo, Framework, Iceberg, Caelus API, Curios API, Cloth Config, Architectury, CodeChicken Lib, Titanium, CorgiLib, PolyLib, MonoLib, OctoLib, EdivadLib, DeimosLib, EpheroLib, JamLib, Lionfish API, Atlas API, Scalable Cat's Force, Fzzy Config, Omega Config, Resourceful Config, Resourceful Lib, Cristel Lib, GeckoLib, SmartBrainLib, McJtyLib, SilentLib, Konkrete, Kotlin for Forge, owo-lib, **playerAnimator, Not Enough Animations — Mineflayer caveat; verify before shipping**, Cucumber Library, Common Capabilities, Cyclops Core, TerraBlender, Moonlight Lib, AlmostUnified, Open Loader, Athena, Jupiter, Glodium, Luminax, Pylons, Crystalix, Trigon, Tesseract API (NF), Sawmill. Shaders: BSL, Complementary Reimagined/Unbound, Euphoria Patches, MakeUp Ultra Fast.

### Scripting / Quests / Server
KubeJS, KubeJS Tweaks, Rhino, Ponder for KubeJS, Quests Lang Splitter, FTB Quests, FTB Library, FTB Teams, FTB Ranks, FTB Chunks, FTB Essentials, FTB Filter System, FTB JEI Extras, FTB Ultimine, FTB XMod Compat, All The Tweaks, Better Compatibility Checker, NeoAuth, No Chat Reports, CC: Tweaked, Advanced Peripherals, More Red (+ CC:T compat).

### Integrated Dynamics suite — all 5 in ATM10
Integrated Dynamics, Integrated Tunnels, Integrated Terminals, Integrated Crafting, Integrated Scripting.

## 3. Overlap Analysis — ATM10 as Blueprint for Create Remastered

### Already in ATM10 that matches our strawman → **keep wholesale**
Create + all ATM10 Create addons (Crafts & Additions, Enchantment Industry, Bells & Whistles, Hypertubes, Dragons Plus, Aquatic Ambitions), Modern Industrialization, Integrated Dynamics suite, Extreme Reactors, Easy Villagers, MineColonies + Structurize + Domum + BlockUI + MultiPiston, Mekanism (+ addons), AE2 + addons, JourneyMap, Jade, JEI/EMI, FTB Quests stack, Waystones, Sophisticated Storage/Backpacks, Farmer's Delight, Pam's HC2, Chipped, Macaw's full suite, FramedBlocks, Supplementaries, Iris/Embeddium/ModernFix/FerriteCore, KubeJS, Apotheosis (modules-only), Corail Tombstone, Silent Gear.

### Must ADD on top — Create Remastered monopoly layer (NOT in ATM10)
- **Create: Aeronautics** (core identity mod — airship physics)
- **Create Railways Navigator** (rail UI on top of Create trains)
- **Minecolonies: War 'N Taxes** (taxation/warfare political layer)
- **SDMShop** (admin shop + currency anchor)
- **Tectonic** (continent-scale worldgen)
- **Terralith** datapack (if not via Lithostitched)
- **Guard Villagers** (militia layer beyond Minecolonies guards)
- **Joe AI external stack**: Mindcraft-CE (Node) + Baritone + custom MCP-over-RCON bridge + voice (Whisper/Piper) + Discord bot

**Caveat flagged for re-verify:** R-ATM10 agent claimed VS2 is a hard dep of Create: Aeronautics. **This appears incorrect** — Create: Aeronautics is a Create-native physics implementation independent of VS2 (per R3 NF+Sinytra+Mineflayer compat report which listed Aeronautics 1.0.2 safe without VS2). VS2 was a hard dep for Create: Interactive, which is a different mod (and dropped). **Action item: confirm Aeronautics 1.0.2 dependency tree before locking VS2 in or out.**

### STRIKE from ATM10 (magic + off-theme dimensions + Mineflayer-hostile + redundant)

**All magic mods (~25):** Ars Nouveau, Ars Additions, Ars Controle, Ars Creo, Ars Elemancy, Ars Elemental, Ars Ocultas, Ars Polymorphia, Ars Technica, Ars Unification, Ars Énergistique, All The Arcanist Gear, All the Wizard Gear, StarbuncleMania, Not Enough Glyphs, Iron's Spells 'n Spellbooks, Iron's Gems 'n Jewelry, Occultism (+ KubeJS), EvilCraft, Forbidden and Arcanus, Mahou Tsukai, Theurgy (+ KubeJS), Roots Classic, Nature's Aura, Relics (+ Artifacts Compat), Reliquary Reincarnations, Apothic Enchanting (keep other Apoth modules), Gateways to Eternity, Hostile Neural Networks, Blue Flame Burning, Potions Master.

**Magic-adjacent endgame strike:**
- Draconic Evolution — endgame magitek; let MI/Extreme Reactors be the apex.
- IceAndFire Community Edition — dragons + magic.
- L_Ender's Cataclysm — boss-magic.

**Off-theme dimensions (~6):**
- The Aether, Twilight Forest, Eternal Starlight, Deeper and Darker, The Bumblezone, The Undergarden.
- (Keep Nether + End + ATM's The Other + The Beyond from F2 verify.)

**Mineflayer-hostile (verify):**
- Not Enough Animations / playerAnimator — historically brittle with Mineflayer pathfinder state.
- Cosmetic Armor Reworked — Curios slot packet quirks.

**Redundant:**
- Security Craft — redundant with FTB Chunks claim layer.

**Other:** Vivecraft if present, Tropicraft / Upgrade Aquatic if present.

### Net pack-size estimate
- ATM10 baseline: ~500
- Magic strike: −25
- Off-theme dimensions: −6
- Magic-adjacent endgame: −3
- Redundant/Mineflayer-hostile: −3 to −5
- **Strikes total: ~37–40**
- Monopoly layer adds: ~8
- **Create Remastered final: ~470 mods** (very close to ATM10 footprint, re-aimed at Create-centric colony politics)

## 4. Mineflayer compatibility flags

Test packet shape before shipping for: Not Enough Animations, Cosmetic Armor Reworked, Curios (Curios slot packets historically confused Mineflayer). No Chat Reports = fine. FTB Teams chat = fine. Iris/Embeddium = client-only, no bot impact.

## Sources

- [Modern Industrialization on CurseForge](https://www.curseforge.com/minecraft/mc-mods/modern-industrialization)
- [Modern Industrialization on Modrinth](https://modrinth.com/mod/modern-industrialization)
- [Industrialization Overdrive 1.8.1+1.21.1](https://modrinth.com/mod/industrialization-overdrive/version/1.8.1+1.21.1)
- [MI Sound Addon](https://www.curseforge.com/minecraft/mc-mods/modern-industrialization-sound-addon/files/7319383)
- [MI Tweaks on Modrinth](https://modrinth.com/mod/mi-tweaks)
- [Integrated Dynamics 1.26.1 for NF 1.21.1](https://modrinth.com/mod/integrated-dynamics/version/1.21.1-1.26.1)
- [Integrated Dynamics 1.30.5-1519 on CF](https://www.curseforge.com/minecraft/mc-mods/integrated-dynamics/files/7629475)
- [Integrated Tunnels 1.8.28 for NF 1.21.1](https://modrinth.com/mod/integrated-tunnels/version/1.21.1-1.8.28)
- [Integrated Terminals 1.6.15 for NF 1.21.1](https://modrinth.com/mod/integrated-terminals/version/1.21.1-1.6.15)
- [IntegratedDynamics Issue #1410 — AE2 Spatial IO crash](https://github.com/CyclopsMC/IntegratedDynamics/issues/1410)
- [Common Capabilities 2.11.1 for NF 1.21.1](https://modrinth.com/mod/common-capabilities/version/1.21.1-2.11.1)
- [Extreme Reactors on CurseForge](https://www.curseforge.com/minecraft/mc-mods/extreme-reactors)
- [Extreme Reactors on Modrinth](https://modrinth.com/mod/extreme-reactors)
- [Easy Villagers 1.21.1-1.1.22 on Modrinth](https://modrinth.com/mod/easy-villagers/version/neoforge-1.21.1-1.1.22)
- [Easy Villagers 1.21.1-1.1.39 on CurseForge](https://www.curseforge.com/minecraft/mc-mods/easy-villagers/files/7202441)
- [All the Mods 10 on CurseForge](https://www.curseforge.com/minecraft/modpacks/all-the-mods-10)
- [All the Mods 10 mod list (categorized)](https://all-themods.com/mod-list/)
- [AllTheMods/ATM-10 GitHub](https://github.com/AllTheMods/ATM-10)
- [AllTheMods/ATM-10 MOD_ISSUES.md](https://raw.githubusercontent.com/AllTheMods/ATM-10/main/MOD_ISSUES.md)
- [All The Guides — ATM10](https://allthemods.github.io/alltheguides/atm10/)
