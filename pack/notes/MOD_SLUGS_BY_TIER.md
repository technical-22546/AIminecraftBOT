# Create Remastered — Mod slugs for CurseForge assembly

Per-tier mod list with likely CurseForge slugs. Slugs help you rapid-add via CF Studio's mod search. **Verify each against the live CF page before committing version pins** — slugs sometimes shift across major version bumps.

Format: `mod name` → `cf-slug` *(notes)*

## Tier 0 — Locked (37)

**Core:**
- Create → `create`
- Create: Aeronautics → `create-aeronautics`
- Sable *(Aeronautics physics-lib dep)* → `sable` (verify, may be bundled with Aeronautics)
- KubeJS → `kubejs`
- KubeJS Custom Events → `kubejs-custom-events`

**Quests:**
- FTB Quests → `ftb-quests-forge` *(NeoForge variant)*
- FTB XMod Compat → `ftb-xmod-compat`
- FTB Quests Optimizer → `ftb-quests-optimizer`
- Quests Freeze Fix → `ftb-quests-freeze-fix`

**Economy:**
- Create: Numismatics → `numismatics`
- SDMShop → `sdmshop` *(verify CF presence; may be Modrinth-primary)*
- Stock Market → `stockmarket`
- Auction House Plus → `auction-house-plus`
- FTB Chunks → `ftb-chunks-forge`
- FTB Teams → `ftb-teams-forge`

**Create ecosystem:**
- Create: The Factory Must Grow → `create-industry`
- Create Crafts & Additions → `createaddition`
- Create Railways Navigator → `create-railways-navigator`
- Create: Additional Logistics → `create-additional-logistics`

**NPC/Empire:**
- Minecolonies → `minecolonies`
- Minecolonies: War 'N Taxes → `minecolonies-war-n-taxes`
- Guard Villagers → `guard-villagers`
- CreatureChat *(or Player2 AI NPC — pick v0)* → `creaturechat`

**ATM:**
- AllTheModium → `allthemodium`
- Iron Furnaces → `iron-furnaces`
- Silent Gear → `silent-gear`

**Worldgen primary:**
- Tectonic → `tectonic`
- Terralith *(if not via Lithostitched in ATM10)* → `terralith` (datapack via Modrinth) — verify
- Lost Cities → `the-lost-cities`

**Nether/End:**
- Incendium → `incendium`
- Nullscape → `nullscape`
- Nether Depths Upgrade → `nether-depths-upgrade`
- YUNG's Better Nether Fortresses → `yungs-better-nether-fortresses`

**Performance baseline:**
- ModernFix → `modernfix`
- FerriteCore → `ferritecore`
- ScalableLux → `scalablelux`
- Embeddium → `embeddium`
- Radium Reforged → `radium-reforged`

**Compat:**
- Sinytra Connector → `connector`
- ConnectorExtras → `connector-extras`

**Tech tiers:**
- Powah Rearchitected → `powah` (verify — Rearchitected may be separate slug)
- Mekanism → `mekanism`
- Mekanism Generators → `mekanism-generators`
- Mekanism Tools → `mekanism-tools`
- Mekanism Additions → `mekanism-additions`

**Rendering:**
- Iris Shaders → `irisshaders` *(NF variant: `oculus` is the older Forge fork — verify which one CF lists as NF 1.21.1 native)*
- Distant Horizons → `distanthorizons`

**Recipe browser:**
- EMI → `emi`

## Tier 1 — Libraries (15) — most should auto-resolve as deps

- Architectury API → `architectury-api`
- Balm → `balm` *(PIN 21.1.x — see PIN_VERSIONS.md)*
- Bookshelf → `bookshelf`
- Cloth Config API → `cloth-config-forge`
- Collective → `collective`
- CorgiLib → `corgilib`
- Curios API → `curios`
- Kotlin for Forge → `kotlin-for-forge`
- Library Ferret → `library-ferret-forge`
- Patchouli → `patchouli`
- Placebo → `placebo`
- Prism Lib → `prism-lib`
- Puzzles Lib → `puzzles-lib`
- Supplementaries → `supplementaries` *(PIN ≥3.0.40)*
- Moonlight Lib → `selene` *(Supplementaries dep; CF slug historically "selene")*
- TerraBlender → `terrablender-neoforge`

## Tier 2 — Decoration (~31 after drops)

**KEEP:**
- Chipped → `chipped`
- Chisel Reborn → `chisel-reborn`
- Rechiseled → `rechiseled`
- Rechiseled: Chipped → `rechiseled-chipped`
- Rechiseled: Create → `rechiseled-create`
- Macaw's Bridges → `mcw-bridges`
- Macaw's Doors → `mcw-doors`
- Macaw's Fences & Walls → `mcw-fences`
- Macaw's Furniture → `mcw-furniture`
- Macaw's Holidays → `mcw-holidays`
- Macaw's Lights & Lamps → `mcw-lights`
- Macaw's Paths & Pavings → `mcw-paths`
- Macaw's Roofs → `mcw-roofs`
- Macaw's Stairs → `mcw-stairs`
- Macaw's Trapdoors → `mcw-trapdoors`
- Macaw's Windows → `mcw-windows`
- Macaw's Paintings → `mcw-paintings`
- Handcrafted → `handcrafted`
- FramedBlocks → `framedblocks`
- Amendments → `amendments`
- Bibliobiomes → `bibliobiomes`
- Bibliocraft → `bibliocraft`
- Bibliowoods Legacy → `bibliowoods-legacy`
- MrCrayfish's Furniture Refurbished → `mrcrayfishs-furniture-refurbished`
- Factory Blocks → `factoryblocks`
- Simply Light → `simply-light`
- Redstone Pen → `redstone-pen`
- Little Big Redstone → `little-big-redstone`
- Construction Sticks → `constructionsticks` *(verify)*
- Cloud Glass → `cloud-glass`
- Connected Glass → `connected-glass`
- ConnectedTexturesMod → `ctm`
- Fusion → `fusion-connected-textures`
- Glassential Renewed → `glassential-renewed`
- Chroma Carvings → `chroma-carvings`
- Everything is Copper → `everything-is-copper`
- Camol → `camol`
- Rainbows! → `rainbows`
- Dyenamics → `dyenamics`
- Dyenamics: Friends → `dyenamics-friends`
- Showcase Item → `showcase-item`
- OpenBlocks Elevator → `openblocks-elevator`
- Travel Anchors → `travel-anchors` *(De-Os fork)*
- Decorative Blocks → `decorative-blocks`
- Decorative Blocks Refreshed → `decorative-blocks-refreshed`
- Quark → `quark` *(tune modules)*
- Additional Lights → `additional-lights`
- Carpet Staircase → `carpet-staircase`
- Tapestry → `tapestry`
- Waystones → `waystones`

## Tier 3 — Worldgen Add-ons (after Group 3 keeps + adds)

- L_Ender's Cataclysm → `lenders-cataclysm`
- Terralith → `terralith` (likely datapack via Modrinth or via Lithostitched bundle)
- Lithostitched → `lithostitched`
- Formations *(Overworld + Nether)* → `formations` (verify single vs split)
- Repurposed Structures → `repurposed-structures-forge`
- Structory → `structory`
- Structory: Towers → `structory-towers`
- Dungeon Crawl → `dungeon-crawl`
- Explorify → `explorify`
- MES (Moog's End Structures) → `moogs-end-structures`
- MNS (Moog's Nether Structures) → `moogs-nether-structures`
- MSS (Moog's Soaring Structures) → `moogs-soaring-structures`
- MVS (Moog's Voyager Structures) → `moogs-voyager-structures` *(CANONICAL VILLAGES — disable T&T + RS village variants)*
- Mo' Structures → `mo-structures`
- Illager Warship → `illager-warship`
- ChoiceTheorem's Overhauled Village → `choicetheorems-overhauled-village`
- Enderman Overhaul → `enderman-overhaul`
- Creeper Overhaul → `creeper-overhaul`
- The Aether → `aether`
- The Undergarden → `the-undergarden`
- Eternal Starlight → `eternal-starlight`
- The Bumblezone → `the-bumblezone-forge`
- Oh The Biomes We've Gone → `oh-the-biomes-weve-gone`
- Oh The Trees You'll Grow → `oh-the-trees-youll-grow`
- Regions Unexplored → `regions-unexplored`
- YUNG's Better Dungeons → `yungs-better-dungeons`
- YUNG's Better Strongholds → `yungs-better-strongholds`
- YUNG's Better Desert Temples → `yungs-better-desert-temples`
- YUNG's Better Jungle Temples → `yungs-better-jungle-temples`
- YUNG's Better Ocean Monuments → `yungs-better-ocean-monuments`
- YUNG's Better Witch Huts → `yungs-better-witch-huts`
- Towns & Towers → `towns-and-towers`
- Stoneholm Underground Villages → `stoneholm`
- Dungeon Now Loading → `dungeon-now-loading`
- When Dungeons Arise → `when-dungeons-arise`

## Tier 4 — Magic (4 only)

- Apotheosis → `apotheosis` *(modules-only — Attributes + Spawners + Enchanting + Apotheosis-core; you confirmed all 4)*
- Apothic Attributes → `apothic-attributes` *(may be bundled in Apotheosis itself; verify)*
- Apothic Spawners → `apothic-spawners`
- Apothic Enchanting → `apothic-enchanting`
- Gateways to Eternity → `gateways`
- Hostile Neural Networks → `hostile-neural-networks`

## Tier 5 — Colony/Villagers (11)

- Minecolonies *(already in Tier 0)*
- Structurize → `structurize`
- Domum Ornamentum → `domum-ornamentum`
- BlockUI → `blockui`
- MultiPiston → `multipiston`
- Byzantine Styles Pack → `byzantine-styles-pack` *(or as a Minecolonies-style-pack subfolder)*
- Stylecolonies → `stylecolonies`
- Easy Villagers → `easy-villagers`
- No Villager Death Messages → `no-villager-death-messages`

## Tier 6 — Agriculture/Food (~20)

- Farmer's Delight → `farmers-delight` *(PIN 1.2.9)*
- Pam's HarvestCraft 2: Crops → `pams-harvestcraft-2-crops`
- Pam's HC2: Food Core → `pams-harvestcraft-2-food-core`
- Pam's HC2: Food Extended → `pams-harvestcraft-2-food-extended`
- Pam's HC2: Trees → `pams-harvestcraft-2-trees`
- Cooking for Blockheads → `cooking-for-blockheads`
- Farming for Blockheads → `farming-for-blockheads`
- Botany Pots → `botany-pots`
- Botany Trees → `botany-trees`
- Sushi Go Crafting → `sushi-go-crafting`
- Storage Delight → `storage-delight`
- Spice of Life: Carrot Edition → `spice-of-life-carrot-edition`
- Mama's Herbs and Harvest → `mamas-herbs-and-harvest`
- Mama's Merrymaking → `mamas-merrymaking`
- Harvest with ease → `harvest-with-ease`
- Reap Mod → `reap-mod`
- TreeHarvester → `treeharvester`
- Wheat Feeder → `wheat-feeder`
- Nether's Delight → `nethers-delight`
- End's Delight → `ends-delight`
- Brewin' and Chewin' → `brewin-and-chewin`
- Expanded Delight → `expanded-delight`
- Cultural Delights → `cultural-delights`
- Miner's Delight → `miners-delight`
- Croptopia → `croptopia`
- Croptopia: FD Compat → `croptopia-fd-compat` *(verify)*
- Sprout → `sprout`
- Insane Lib → `insane-lib` *(Expanded Delight dep)*

## Tier 7 — Storage (~26)

- Applied Energistics 2 → `applied-energistics-2`
- AE2 Wireless Terminals → `ae2wtlib`
- AE2 JEI → `ae2-jei-integration` *(may not need under EMI; verify)*
- AE2 Network Analyser → `ae2-network-analyser`
- AE2 Things → `ae2things`
- AE2 Additions → `ae2-additions`
- Advanced AE → `advanced-ae`
- Applied Flux → `applied-flux`
- Applied Mekanistics → `applied-mekanistics`
- ExtendedAE → `extendedae`
- Expanded AE → `expandedae`
- Immersive Energistics → `immersive-energistics`
- ME Requester → `me-requester`
- MEGA Cells → `megacells`
- EnderDrives → `enderdrives`
- Extra Disks → `extra-disks`
- ExtraStorage → `extrastorage` *(may need verify — was an AE2 addon historically)*
- Polymorphic Energistics → `polymorphic-energistics`
- Soulplied Energistics → `soulplied-energistics`
- Sophisticated Storage → `sophisticated-storage`
- Sophisticated Storage In Motion → `sophisticated-storage-in-motion`
- Sophisticated Storage Create Integration → `sophisticated-storage-create-integration`
- Sophisticated Backpacks → `sophisticated-backpacks`
- Sophisticated Backpacks Create Integration → `sophisticated-backpacks-create-integration`
- Sophisticated Core → `sophisticated-core`
- Functional Storage → `functional-storage`
- Pocket Storage → `pocket-storage`
- DimStorage → `dimstorage`
- Ender Storage → `ender-storage-1-8`
- Entangled → `entangled`
- Cable Tiers → `cable-tiers`
- Item Collectors → `item-collectors`
- Trash Cans → `trash-cans`
- Tool Belt → `tool-belt`
- Shrink. → `shrink`
- Tempad → `tempad`
- Shulker Drops Two → `shulker-drops-two`

## Tier 8 — Adventure/Combat/Gear (~11 after drops)

- Silent Gear *(already in Tier 0)*
- Silent's Gems → `silents-gems`
- Silent Gear: Metalworks → `silent-gear-metalworks`
- Artifacts → `artifacts`
- Baubley Heart Canisters → `baubley-heart-canisters`
- Cosmetic Armor Reworked → `cosmetic-armor-reworked` *(Mineflayer caveat accepted)*
- Jonn's Trophies → `jonns-trophies`
- Equipment Compare → `equipment-compare`
- Colorful Hearts → `colorful-hearts`
- Overloaded Armor Bar → `overloaded-armor-bar`
- Mob Grinding Utils → `mob-grinding-utils`
- Hardened Armadillos → `hardened-armadillos`
- Gravitational Modulating Additional Unit → `gravitational-modulating-additional-unit`
- Target Dummy → `target-dummy`
- Simply Swords → `simply-swords`

## Tier 9 — QoL (~42 after drops/swaps)

- Xaero's Minimap → `xaeros-minimap`
- Xaero's World Map → `xaeros-world-map`
- Jade → `jade`
- Jade Addons (NF) → `jade-addons-forge`
- Just Enough Archaeology → `just-enough-archaeology`
- Just Enough Breeding → `just-enough-breeding`
- Just Enough Mekanism Multiblocks → `just-enough-mekanism-multiblocks`
- Just Enough Professions → `just-enough-professions`
- Just Zoom → `just-zoom`
- Smithing Template Viewer → `smithing-template-viewer`
- GuideME → `guideme`
- Patchouli *(in libraries)*
- Modonomicon → `modonomicon`
- Waystones *(in Tier 0 — confirmed)*
- Explorer's Compass → `explorers-compass`
- Nature's Compass → `natures-compass`
- Oracle Index → `oracle-index`
- Almanac Lib → `almanac-lib`
- Akashic Tome → `akashic-tome`
- Controlling → `controlling`
- Inventory Essentials → `inventory-essentials`
- Inventory Tweaks - ReFoxed → `inventory-tweaks-refoxed`
- Mouse Tweaks → `mouse-tweaks`
- Crafting Tweaks → `crafting-tweaks`
- Crafting on a stick → `crafting-on-a-stick`
- KeyBind Bundles → `keybind-bundles`
- KeybindsPurger → `keybindspurger`
- Rebind Narrator → `rebind-narrator`
- Dark Mode Everywhere → `dark-mode-everywhere`
- Legendary Tooltips → `legendary-tooltips`
- Enchantment Descriptions → `enchantment-descriptions`
- Searchables → `searchables`
- AppleSkin → `appleskin` *(PIN 3.0.5)*
- Polymorph → `polymorph`
- Clumps → `clumps`
- AttributeFix → `attributefix`
- Better Advancements → `better-advancements`
- Toast Control → `toast-control`
- Simple Backups → `simple-backups`
- Crash Assistant → `crash-assistant`
- Crash Utilities → `crash-utilities`
- FancyMenu → `fancymenu`
- PackMenu → `packmenu`
- Drippy Loading Screen → `drippy-loading-screen`
- Bad Wither No Cookie Reloaded → `bad-wither-no-cookie-reloaded`
- Hey Berry SHUT UP → `hey-berry-shut-up`
- Let Me Despawn → `let-me-despawn`
- Fireproof Boats → `fireproof-boats`
- NetherPortalFix → `netherportalfix`
- Get It Together Drops! → `get-it-together-drops`
- Measurements → `measurements`
- Prism → `prism`
- EMI Loot → `emi-loot`
- EMI Ores → `emi-ores`
- BBOR (Bounding Box Outline Reloaded) → `bounding-box-outline-reloaded`
- SmartBrainLib → `smartbrainlib`
- Chunk Pregenerator → `chunkpregenerator`
- Advancements Reloaded → `advancements-reloaded`
- Advancement Plaques → `advancement-plaques`
- ScreenshotToClipboard → `screenshot-to-clipboard`
- Visual Workbench → `visual-workbench`
- Catalogue → `catalogue`
- Configured → `configured`
- Loot Journal → `loot-journal-neoforge`
- Fragmentum → `fragmentum-neoforge` *(Loot Journal req dep)*

## Tier 10 — Performance/Libs/Shaders (~55)

*(Tier 0 already covers ModernFix, FerriteCore, ScalableLux, Embeddium, Radium Reforged, Sinytra, Iris, DH, etc.)*

- ImmediatelyFast → `immediatelyfast`
- AllTheLeaks → `alltheleaks`
- Nolijium → `nolijium`
- Connectivity → `connectivity`
- ServerCore → `servercore`
- Cobweb → `cobweb`
- Cupboard → `cupboard`
- Memory Settings → `memory-settings`
- Model Gap Fix → `model-gap-fix`
- FlickerFix → `flickerfix`
- Load My F***ing Tags → `load-my-tags`
- FastFurnace → `fastfurnace`
- FastSuite → `fastsuite`
- FastWorkbench → `fastworkbench`
- Monocle → `monocle`
- AlmostUnified → `almostunified`
- Caelus API → `caelus-api`
- Framework → `framework`
- Iceberg → `iceberg`
- Titanium → `titanium`
- PolyLib → `polylib`
- MonoLib → `monolib`
- OctoLib → `octolib`
- EdivadLib → `edivadlib`
- DeimosLib → `deimoslib`
- EpheroLib → `epherolib`
- JamLib → `jamlib`
- Lionfish API → `lionfish-api`
- Atlas API → `atlas-api`
- Scalable Cat's Force → `scalable-cats-force`
- Fzzy Config → `fzzy-config`
- Omega Config → `omega-config`
- Resourceful Config → `resourceful-config`
- Resourceful Lib → `resourceful-lib`
- Cristel Lib → `cristel-lib`
- GeckoLib → `geckolib`
- McJtyLib → `mcjtylib`
- SilentLib → `silent-lib`
- Konkrete → `konkrete`
- owo-lib → `owo-lib`
- playerAnimator → `playeranimator` *(Mineflayer caveat accepted)*
- Not Enough Animations → `not-enough-animations` *(Mineflayer caveat accepted)*
- Cucumber Library → `cucumber-library`
- Common Capabilities → `common-capabilities`
- Cyclops Core → `cyclops-core`
- Open Loader → `open-loader`
- Athena → `athena`
- Jupiter → `jupiter`
- Glodium → `glodium`
- Luminax → `luminax`
- Pylons → `pylons`
- Crystalix → `crystalix`
- Trigon → `trigon`
- Tesseract API → `tesseract-api`
- Sawmill → `sawmill`
- Sound Physics Remastered → `sound-physics-remastered`
- Ambient Sounds → `ambient-sounds`
- Dynamic Lights Reforged → `dynamic-lights-reforged`
- Falling Leaves → `falling-leaves`
- Better Animations Collection Redone → `better-animations-collection-redone`
- Boatload → `boatload`
- Capes → `capes`
- Voice Chat Volume Slider → `voice-chat-volume-slider`
- Particular → `particular`
- Wandering Particles → `wandering-particles`
- Tree Rings → `tree-rings`
- Particle Core → `particle-core`

**Shaders (client-side, optional bundle):**
- BSL Shaders → `bsl-shaders`
- Complementary Reimagined → `complementary-reimagined`
- Complementary Unbound → `complementary-unbound`
- Euphoria Patches → `euphoria-patches`
- MakeUp Ultra Fast → `makeup-ultra-fast-shaders`

## Tier 11 — Scripting/Quests/Server (~28)

- KubeJS *(in Tier 0)*
- KubeJS Tweaks → `kubejs-tweaks`
- Rhino → `rhino`
- Ponder for KubeJS → `ponder-for-kubejs`
- Quests Lang Splitter → `quests-lang-splitter`
- FTB Quests *(in Tier 0)*
- FTB Library → `ftb-library-forge`
- FTB Teams *(in Tier 0)*
- FTB Ranks → `ftb-ranks-forge`
- FTB Chunks *(in Tier 0)*
- FTB Essentials → `ftb-essentials-forge`
- FTB Filter System → `ftb-filter-system`
- FTB Ultimine → `ftb-ultimine-forge`
- FTB XMod Compat *(in Tier 0)*
- All The Tweaks → `all-the-tweaks`
- Better Compatibility Checker → `better-compatibility-checker`
- NeoAuth → `neoauth`
- No Chat Reports → `no-chat-reports`
- CC: Tweaked → `cc-tweaked`
- Advanced Peripherals → `advanced-peripherals`
- More Red → `more-red`

## Tier 12 — Tech (Standalone, ~26)

- Modern Industrialization → `modern-industrialization`
- Industrialization Overdrive → `industrialization-overdrive`
- Extended Industrialization → `extended-industrialization`
- MI Sound Addon → `modern-industrialization-sound-addon`
- MI Tweaks → `mi-tweaks`
- Oritech → `oritech`
- Immersive Engineering → `immersive-engineering` *(PIN 12.4.2-194)*
- Immersive Petroleum → `immersive-petroleum` *(PIN 4.4.1-37)*
- PneumaticCraft: Repressurized → `pneumaticcraft-repressurized`
- Industrial Foregoing → `industrial-foregoing`
- Industrial Foregoing Souls → `industrial-foregoing-souls`
- Industrial Foregoing MIFA → `industrial-foregoing-mifa`
- Railcraft Reborn → `railcraft-reborn`
- Steve's Carts → `steves-carts-reborn` *(verify)*
- Draconic Evolution → `draconic-evolution`
- Brandon's Core → `brandons-core`
- Flux Networks → `flux-networks`
- XNet → `xnet`
- Laser IO → `laserio`
- Laser Bridges & Doors → `laser-bridges-and-doors`
- Modern Dynamics → `modern-dynamics`
- Modular Routers → `modular-routers`
- Mekanistic Routers → `mekanistic-routers`
- Ranged Pumps → `ranged-pumps`
- Generator Galore → `generator-galore`
- Iron Jetpacks → `iron-jetpacks`
- Compact Machines → `compact-machines`
- Hyperbox → `hyperbox`
- Super Factory Manager → `super-factory-manager`
- RFTools Base → `rftools-base`
- RFTools Power → `rftools-power`
- RFTools Builder → `rftools-builder`
- RFTools Storage → `rftools-storage`
- RFTools Utility → `rftools-utility`
- RFTools Control → `rftools-control`
- Actually Additions → `actually-additions`
- Modular Machinery Reborn → `modular-machinery-reborn`
- Modular Machinery Reborn: Mekanism → `modular-machinery-reborn-mekanism`
- Extreme Reactors → `extreme-reactors`
- Ender IO → `ender-io`
- Productive Metalworks → `productive-metalworks`
- Productive Bees → `productive-bees`
- Productive Trees → `productive-trees`
- Modular Bees → `modular-bees`
- Integrated Dynamics → `integrated-dynamics`
- Integrated Tunnels → `integrated-tunnels`
- Integrated Terminals → `integrated-terminals`
- Integrated Crafting → `integrated-crafting`
- Integrated Scripting → `integrated-scripting`
- Immersive Ores → `immersive-ores` *(Vibranium tag clash with AllTheModium — resolve in KubeJS v0)*

## Tier 13 — Create addons (additional, ~9)

- Create: Steam 'n Rails → `create-steam-n-rails` *(community NF 1.21.1 port — verify)*
- Create: Enchantment Industry → `create-enchantment-industry`
- Create: New Age → `create-new-age`
- Create: Big Cannons → `create-big-cannons`
- Create: Copycats+ → `copycats-plus`
- Create: Dreams and Desires → `create-dreams-n-desires`
- Create: Liquid Fuel → `create-liquid-fuel`
- Create: Slice & Dice → `create-slice-and-dice`
- Create: Aquatic Ambitions → `create-aquatic-ambitions`
- Create: Bells & Whistles → `create-bells-and-whistles`
- Create: Dragons Plus → `create-dragons-plus`
- Create: Hypertubes → `create-hypertubes`

## Bot-friendly add (Workstream H sandbox/test dep)

Server-side only — runs alongside the pack but isn't shipped to clients:

- *(External: Mindcraft-CE — not a CF mod, runs on `joe-agent` LXC)*
- *(External: Joe AI services — separate stack per Workstream E port plan)*

## Difficulty (~5)

- Silent's Power Scale → `silents-power-scale` *(swap for discontinued Scaling Health)*
- Apotheosis *(in Tier 4 magic-section — kept all 4 modules)*
- Hordes (or Undead Nights, pick one) → `hordes` *(if Hordes — REQUIRE In Control!)*
- In Control! → `in-control`

## Difficulty / extras (final adds)

- SDMShop *(in Tier 0)*

---

## Estimated final count

Roughly **~314 mods** per `docs/planning/a2-mod-shortlist-v1-locked.md` totals. Some of the entries above will resolve as deps and won't need explicit add (e.g., libraries auto-add when their parent mod is added in CF Studio).

## Workflow tips for CF assembly

1. **Bulk-add via CF Studio search:** type slug, filter to NF 1.21.1, "add latest" or "add specific version" if PIN_VERSIONS.md flags it.
2. **Disable auto-update for pinned mods** — set them to specific file IDs in `manifest.json`.
3. **Resolver errors are normal** during assembly — work through them tier-by-tier.
4. **Save modlist.html** to track what you've added across sessions.
5. **Test with empty world** before populating overrides/.
