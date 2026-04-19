# A-2 Compat Verification Pass 1 (2026-04-18)

_Consolidated output from 5 parallel compat-verification agents against A-2 strawman. **Mixed WebSearch access across agents** — some verifications are pattern-based from training knowledge rather than live CF/Modrinth fetches. Every entry marked as requiring live re-check on build day. Use as a pre-check scaffold, not final authority._

---

## Sweep 2 — Decoration + Aesthetic (returned first)

_Agent self-reported: no WebSearch access. Pattern-based. Live re-verify mandatory._

### Tier 2 Decoration verdicts

| Mod | NF 1.21.1 version | Conflict notes | Bot-friendly | Verdict |
|---|---|---|---|---|
| Chipped | 4.0.x (native NF) | None | y | **SHIP** |
| Macaw's Bridges | 3.1.x via Connector | Needs Connector; align Macaw's minor | y | SHIP |
| Macaw's Doors | 1.2.x via Connector | Double Doors overlaps — disable DD auto-pair | caveat (non-vanilla hinges) | MAYBE |
| Macaw's Windows | 2.3.x via Connector | None | y | SHIP |
| Macaw's Fences | 1.1.x via Connector | Quark fence extension conflict — disable | y | SHIP |
| Macaw's Furniture | 3.3.x via Connector | Custom sit-blocks confuse Mineflayer | caveat | MAYBE |
| Macaw's Lights & Lamps | 1.1.x via Connector | Perfect Gilded-Age fit | y | **SHIP** |
| Macaw's Roofs | 2.3.x via Connector | None | y | SHIP |
| Macaw's Paintings | 1.0.5 via Connector | None | y | SHIP |
| Macaw's Paths/Pavings | 1.1.x via Connector | None | y | SHIP |
| Handcrafted | 4.0.x NF | Victorian fit | y | **SHIP** |
| Supplementaries | 3.0.40+ NF | **Pin to ≥3.0.40** — older builds break Mineflayer (#1195) | y only ≥3.0.40 | SHIP (pin) |
| Quark | 4.0.x NF | Overlap Chipped/Supplementaries/Amendments — tune modules | y | SHIP (tune) |
| Additional Lights | 1.21-NF | None | y | SHIP |
| Decorative Blocks | 5.0.x NF | None | y | SHIP |
| Decorative Blocks Refreshed | NF port | Matches DB major | y | SHIP |
| Frame Up | Fabric-only | Collides with Framed Blocks | caveat | **SKIP** (redundant) |
| Framed Blocks | 1.21.1-NF (AztecTheLost) | None | y | **SHIP** |
| Little Contraptions | Not on 1.21.1 as of early 2026 | — | — | **SKIP** |
| Amendments | 1.21-NF | None | y | SHIP |
| Variants & Ventures | Fabric-only | Connector-only, minor datapack issues | caveat | MAYBE |
| Double Doors | NF 1.21.1 | Conflicts Macaw's Doors — whitelist | y | MAYBE |
| Carpet Staircase | Connector-viable | None | y | SHIP |
| Better With Art | Likely 1.20 only | — | — | **SKIP** |
| Better Wooden Ladders | NF 1.21.1 | **Custom climb speed desyncs Mineflayer Y** | caveat | MAYBE |
| Builder's Delight | Fabric, Connector-viable | None | y | SHIP |
| Sit Everywhere | Not on 1.21.1 | Would conflict Quark chairs | — | **SKIP** |
| Tapestry | Connector | Works; Victorian fit | y | SHIP |
| Rope Bridge | NF 1.21.1 | **Bot pathfinding can't traverse dynamic rope** | n | **SKIP** (or non-bot zones only) |
| Glassential | NF 1.21.1 | None | y | SHIP |
| Stained Glass+ | Connector | Glassential overlap — pick one | y | MAYBE |
| Painted World | Not on 1.21.1 | — | — | **SKIP** |
| Waystones | 21.1.x NF (BlayTheNinth) | None; well-supported by Mineflayer plugins | y | **SHIP** |
| Bookshelves (custom rendering) | Ambiguous ID | KubeJS render double-fire risk | caveat | MAYBE (clarify ID) |
| Mana and Artifice Decor | Heavy magic dep | **Off-theme** | — | **SKIP** (no magic) |
| Create: Deco Cannons | Name unclear; Create Big Cannons addon is 1.20.1 only | — | — | **SKIP** |
| Amendments: Additional Furniture | NF 1.21.1 | None | y | SHIP |
| Velvet Curtains + Carpet Trim | Not a published mod | Build in-house via KubeJS + resource pack | y | SHIP (custom) |

### Tier 9 Aesthetic verdicts

| Mod | NF 1.21.1 version | Conflict notes | Bot-friendly | Verdict |
|---|---|---|---|---|
| Ambient Sounds 6 | 6.1.x NF | None | y (client) | SHIP |
| Sound Physics Remastered | 1.21.1 NF (henkelmax) | CPU heavy | y | SHIP |
| Presence Footsteps | Connector-viable | None | y | MAYBE |
| Physics Mod (Free) | 1.21.1 Free build | **Severe perf hit w/ Create contraptions** — disable ragdoll+snow | y | MAYBE (warn) |
| Particle Core | 1.21.1 NF | None | y | SHIP |
| Particular | Connector | None | y | SHIP |
| Better Clouds | Connector-only | Shader overlap | y | MAYBE |
| Cloth Falling Leaves | Connector | Overlap Falling Leaves — pick one | y | MAYBE |
| Falling Leaves | NF 1.21.1 (native) | Prefer over Cloth variant | y | **SHIP** |
| Wandering Particles | Connector | None | y | SHIP |
| Tree Rings | NF 1.21.1 | None | y | SHIP |
| Dynamic Lights | **Prefer Dynamic Lights Reforged (NF native) or Sodium Dynamic Lights NF port** | None | y | SHIP (NF-native fork) |
| Better Animations Collection Redone | NF 1.21.1 | Conflicts NEA arm-pose — NEA priority | caveat | MAYBE |
| Boatload | NF 1.21.1 | None | y | SHIP |
| Fancy Leaves / Better Foliage | Better Foliage unstable; Puzzle's Fancy Leaves works | GPU cost | y | MAYBE |
| Stay True | Resource pack | **Fights our custom textures** | y | **SKIP** |
| Capes | NF 1.21.1 | None | y | SHIP |
| First-Person Animations | Bundled with NEA | Duplicate | y | **SKIP** (redundant) |
| Not Enough Animations | NF 1.21.1 | None | y | **SHIP** |
| Voice Chat Volume Slider | SVC addon, NF 1.21.1 | Requires SVC | y | SHIP |
| Fancy Crops | ID unclear | — | — | **SKIP** (clarify ID) |

### Key cross-cutting findings

1. **Supplementaries must be ≥3.0.40** to avoid Mineflayer-breaking packets (issue #1195).
2. **Stay True** hard SKIP — overrides our custom Industrial/Gilded-Age texture pack.
3. **Dynamic Lights** — switch to NF-native fork, not Lambdynlights-via-Connector.
4. **Physics Mod Free** — disable ragdoll/snow or skip; severe Create-contraption perf.
5. **Mineflayer bot-unfriendly blocks**: Rope Bridge (skip), Sit Everywhere (skip), Better Wooden Ladders (MAYBE climb-desync), Macaw's Furniture sit-blocks (MAYBE), Macaw's Doors hinges (MAYBE).
6. **Macaw's suite** — pin all to same minor version; mismatches cause block-registry desync.
7. **Quark** — disable overlapping modules (Chipped, Supplementaries, Amendments duplicates).
8. **Ambiguous entries to clarify**: Create: Deco Cannons, Velvet Curtains + Carpet Trim, Fancy Crops, Bookshelves-custom-rendering, Little Contraptions, Painted World, Better With Art — some are misnamed / custom content / unreleased.

---

## Sweep 1 — Infra + Create addons + Compat + Tinkers (pending)

_Agent still running._

## Sweep 3 — QoL + Utility (pending)

_Agent still running._

## Sweep 4 — Mobs + Food + Adventure (returned)

_Agent self-reported: no WebSearch, training cutoff Jan 2026. Live re-verify mandatory for Feb–Apr 2026 releases._

### Tier 4 Mobs

| Mod | Verdict | Notes |
|---|---|---|
| Alex's Mobs (+ Citadel dep) | SHIP | NF 1.21.1 native |
| Mowzie's Mobs | SHIP | Boss structures may collide Structory — add spacing blacklist |
| Born in Chaos | MAYBE | Forge 1.21.1, NF via Connector; test boss-phase packets w/ Mineflayer |
| Dynamic Lights Reforged (NF native) | SHIP | Prefer over Lambdynlights-via-Connector |
| Friends & Foes | SHIP | |
| Untamed Wilds | **SKIP** | Abandoned, no 1.21.1 |
| Rats | SHIP | Citadel dep |
| Insane Lib | SHIP | Library for Expanded Delight |
| Goblins & Dungeons | MAYBE/SKIP | 1.21.1 NF status unconfirmed |
| Creatures and Beasts | SHIP | |
| Enhanced Celestials | SHIP | Works w/ SereneSeasons |
| **Epic Knights** | **SKIP** | **Custom combat packets break Mineflayer PvP extension** |
| Twilight Forest | SHIP | No ATM dim collision; gate portal recipe in FTB Quests |
| Naturalist | SHIP | Connector |

### Tier 5 Food

| Mod | Verdict | Notes |
|---|---|---|
| Farmer's Delight (vectorwing 1.2.5+) | SHIP | |
| Create: Enchantment Industry Cookery (bundled in CEI) | SHIP | |
| Nether's Delight | SHIP | |
| End's Delight | SHIP | |
| Brewin' and Chewin' | SHIP | Via Connector |
| Expanded Delight | SHIP | Requires Insane Lib |
| Cultural Delights | SHIP | |
| Miner's Delight | SHIP | |
| Croptopia (+ Terralith compat built-in) | SHIP | |
| Croptopia FD compat | SHIP | |
| **SereneSeasons** | **MAYBE** | Perf cost ~5–10% TPS; **conflicts Agricarnation** on crop tick hooks. Disable crop tempering to recover TPS. |
| **Agricarnation** | **MAYBE** | **PICK ONE** with SereneSeasons |
| Sprout | SHIP | Connector |
| Butcher Knives | MAYBE | Fabric-only, Connector-viable |
| Spice of Life: Carrot Edition | SHIP | |
| **Pam's HarvestCraft 3** | **SKIP** | Fragmented ports; redundant w/ Croptopia + Delights |
| Create: Enchantment Industry | SHIP | |

### Tier 7 Adventure

| Mod | Verdict | Notes |
|---|---|---|
| YUNG's Better Dungeons | SHIP | |
| YUNG's Better Strongholds | SHIP | |
| YUNG's Better Desert Temples | SHIP | |
| YUNG's Better Jungle Temples | SHIP | |
| YUNG's Better Ocean Monuments | SHIP | |
| YUNG's Better Witch Huts | SHIP | |
| **YUNG's Better End Island** | **SKIP** | **Conflicts Nullscape (locked)** |
| YUNG's Menu Tweaks | SHIP | |
| **Towns & Towers** | MAYBE | Village spacing overlap MVS — raise separation ≥32 chunks or disable T&T villages |
| **Repurposed Structures** | MAYBE | Village overlap MVS + T&T — disable RS villages |
| Structory + Towers + Cities | SHIP | Tune spacing ≥20 chunks; no Incendium/Nullscape overlap |
| Stoneholm Underground Villages | SHIP | |
| Dungeon Now Loading | SHIP | Connector |
| **When Dungeons Arise** | SHIP (tune) | Raise separation ≥40 chunks; density w/ YUNG's + T&T |
| **Medieval Villages Overhaul** | SHIP (canonical) | Canonical villages; disable T&T + RS village variants |
| Explorer's Compass | SHIP | |
| Nature's Compass | SHIP | |
| The Graveyard | MAYBE | **Pick one** with Graveyard Overhaul |
| Dungeon Crawl | SHIP | Raise under-stronghold depth |
| Awesome Dungeon (biome sets) | MAYBE | Density overlap WDA — tune |
| Philip's Ruins | MAYBE | 1.21.1 port uncertain |
| AI Improvements | SHIP | Server-side AI tweaks only; Mineflayer-safe |
| The Undergarden | SHIP | Dim; no ATM overlap |
| Deeper & Darker | SHIP | Otherside dim; no ATM overlap |
| Graveyard Overhaul | MAYBE | **Pick one** with The Graveyard |
| Waystones | SHIP | BalmLib dep |

### Cross-cutting

- **Epic Knights SKIP** per bot-unfriendly policy (custom combat packets).
- **Structure density tuning**: YUNG's + T&T + WDA + Structory Cities together oversaturates worldgen — lift separations.
- **Village canonicalization**: MVS as primary; disable T&T + RS village variants.
- **Crop-tick exclusivity**: SereneSeasons XOR Agricarnation.
- **TF safe w/ ATM** — no dim ID collision; gate portal in quests for progression pacing.

---

## Sweep 1 — Infra + Create addons + Compat + Tinkers (returned, WebSearch-verified)

### Tier 1 Libraries — all SHIP with version pins

| Library | NF 1.21.1 version | Notes |
|---|---|---|
| Architectury API | 13.0.8+neoforge | |
| **Balm (NeoForge)** | **21.1.x** | **PIN 21.1.x — 21.11.x is 1.21.11 only** |
| Bookshelf | 21.1.80 | |
| Cloth Config | 15.0.140+neoforge | |
| Collective | 1.21.1-7.91 | |
| CorgiLib | 1.21.1-5.0.0.9 | |
| Curios API | 9.5.1+1.21.1 | **Use Curios; do NOT ship Trinkets natively** — Connector bridges Trinkets-based Fabric mods |
| Kotlin for Forge | 6.2.0 | KubeJS dep |
| Library Ferret | 4.0.0 | |
| Patchouli | 1.21.1-93-NEOFORGE | |
| Placebo | 1.21.1-9.9.1 | |
| Prism Lib | 1.21.1-neoforge-1.0.11 | |
| Puzzles Lib | 21.1.38 | |
| Supplementaries + Moonlight | Supp 3.5.34 / Moonlight 2.29.32 | Current builds bot-safe |
| TerraBlender | 1.21.1-4.1.0.8 | Dep of BOP/regional biome mods |

### Tier 10 Tinkers' — **DROP ENTIRE TIER**

**No official Tinkers' Construct NF 1.21.1 port as of April 2026.** SlimeKnights roadmap: Mantle → Metalborn → TiC. Only experimental community ports (LopyLuna/Tinkers21Port, Rainy1127/unofficial) — neither production-ready. **Silent Gear (already locked) covers the tool-tiering niche.** Drop TiC + Rapier + Tool Leveling + Plustic + Construct's Armory + Pane in the Glass + ExtraTiC entirely.

### Tier 12 Compat

| Mod | NF 1.21.1 version | Verdict |
|---|---|---|
| **Farmer's Delight** | **1.2.9** | **PIN 1.2.9 — 1.2.11 breaks Create: Integrated Farming (#1266)** |
| Create ↔ FD crossover | Built-in at FD 1.2.9+ | SHIP |
| Alex's Delight (AM+FD compat) | 1.6 (Feb 2026) | MAYBE — depends on Alex's Mobs port stability |
| Every Compat (Macaw's) | 1.21-2.11.37-neoforge | SHIP |
| Create + TiC bridge | N/A | SKIP (no TiC) |

### Tier 13 Create addons — major drops

| Addon | Verdict | Reason |
|---|---|---|
| Create: Steam 'n Rails | **MAYBE** | Community NF 1.21.1 port only (Developer-lfierro743); test against Create 6.0.9 |
| Create: Enchantment Industry 2.3.0 | SHIP | For Create 6.0.9+ |
| **Create: Interactive** | **SKIP** | 1.20.1 Fabric/Forge only; VS2 dep; VS2 not on NF 1.21.1 stable; **VS2 ship physics also breaks Mineflayer** |
| **Create: Metallurgy** | **SKIP** | 1.20.1 only (last was 0.0.7) |
| Create: New Age 1.1.7c | SHIP | |
| **Create: Deco Cannons** | renamed/clarified | Likely Create: Big Cannons 5.11.0 — SHIP (bot-safe projectiles but bots can't see cannonballs) |
| Create: Copycats+ 3.0.4 | SHIP | CBC 5.11 already patched |
| Create: Dreams and Desires | MAYBE | Fabric build via Connector |
| **Create: Utilitarian** | **SKIP** | Not found on NF 1.21.1 |
| Create: Liquid Fuel 2.1.1 | SHIP | |
| **Create: Copperworks** | **SKIP** | Not found on NF 1.21.1 |
| Create: Slice & Dice 4.2.4 | SHIP | FD optional dep |
| **Create: Symmetry** | **SKIP (doesn't exist)** | Symmetry is a built-in Create feature, not an addon |

---

## Sweep 3 — QoL + Utility (returned, WebSearch-verified)

### Tier 3 QoL verdicts

| Mod | Version | Verdict |
|---|---|---|
| Xaero's Minimap | 25.3.5 | SHIP |
| Xaero's World Map | 1.39.8 | SHIP (pair-lock with Minimap) |
| **Jade** | 15.10.3+neoforge | SHIP — exclude WTHIT (double-renders) |
| Jade Addons (NF) | 6.1.0+neoforge | SHIP |
| **JER (Just Enough Resources)** | — | **SKIP** — JEI-only; we use EMI |
| EMI | 1.1.21–1.1.22 | SHIP |
| EMI Loot | 0.7.4+1.21+neoforge | SHIP |
| **EMI Trades** | — | **SKIP** — no NF 1.21.1 build |
| EMI Ores | 1.2+1.21.1+neoforge | SHIP |
| AppleSkin | **3.0.5** | PIN 3.0.5 — 3.0.8 is 1.21.11 |
| Mouse Tweaks | 2.26.1 | SHIP |
| Inventory Essentials | 21.1.1+neoforge-1.21.1 | SHIP (needs Balm) |
| IPN | 2.1.9 | SHIP |
| No Chat Reports | 2.10.x | SHIP |
| No Resource Pack Warnings | 1.21-1.0.0 | SHIP |
| Pick Up Notifier | 21.1.x (Fuzss) | SHIP |
| BBOR | 1.21.1 | SHIP |
| BWNCR | 21.1.x (Fuzss) | SHIP |
| SmartBrainLib | 1.15.x | SHIP |
| Chunk Pregenerator | 4.4.5.1 | SHIP (run once pre-claim) |
| Advancements Reloaded | 0.6.1+neoforge-1.21.1 | SHIP |
| Advancement Plaques | 1.6.x | SHIP |
| Controlling | 18.0.x | SHIP |
| **Configured (NOT Mod Menu)** | 2.x | SHIP — Mod Menu is Fabric-only |
| ScreenshotToClipboard | 1.0.x | SHIP |
| Crash Utilities | sparse | MAYBE |
| Visual Workbench | v21.1.0 | SHIP |
| Drippy Loading Screen | 3.0.12 / 3.1.0 | SHIP (needs FancyMenu + Konkrete) |
| Loading Screen Tweaks | — | MAYBE (pick one vs Drippy) |
| Catalogue | MrCrayfish NF 1.21.1 | SHIP |
| FancyMenu | 3.3.x | SHIP |

### Tier 10 Utility verdicts

| Mod | Version | Verdict |
|---|---|---|
| Carry On | 2.10.x | SHIP (blacklist Create BEs in config) |
| Chisels & Bits | 21.1.11 | SHIP |
| Construction Wand | ricksouth NF 1.21.1 | SHIP |
| Building Gadgets | BG3 NF 1.21.1 | SHIP (redundant w/ Construction Wand but safe together) |
| **Building Wands** | 3.0.2-beta | **MAYBE — redundant with BG+ConstructionWand; drop** |
| Fast Workbench | 21.1.x (Fuzss) | SHIP (compat with Visual Workbench) |
| Tool Belt | 3.0.x (gigaherz) | SHIP (Curios opt) |
| Traveler's Backpack | 10.1.21 | SHIP |
| Create: Steam 'n Rails | community port | MAYBE |
| **Ender's Bags** | — | **SKIP** — no clean 1.21.1 NF build |
| **Jet Packs** | — | **SKIP** — redundant with Mekanism jetpack |
| Simply Swords | 1.58+ | SHIP |
| **Trinkets → Curios** | — | Use Curios; Connector bridges Trinkets-based Fabric accessory mods |
| **Flint & Steel+** | — | **SKIP** — no verified build |
| **Hopper Ducts** | — | **SKIP** — no native 1.21.1; use Create pipes |
| Chisel Reborn | 1.8.1 | SHIP (pick Reborn over Chisel Modern) |
| **Decorative Barrels** | — | **SKIP** — no 1.21.1 build |

---

## Major shortlist changes so far (Sweeps 1, 2, 3, 4)

### DROP (no NF 1.21.1 port or redundant)

**Tier 2 Decoration:** Frame Up, Little Contraptions, Better With Art, Sit Everywhere, Painted World, Mana and Artifice Decor, Create: Deco Cannons (as named)
**Tier 4 Mobs:** Untamed Wilds, **Epic Knights**
**Tier 5 Food:** Pam's HarvestCraft 3
**Tier 7 Adventure:** YUNG's Better End Island (Nullscape conflict)
**Tier 9 Aesthetic:** Stay True (resource pack conflict), First-Person Animations (redundant with NEA)
**Tier 10 Utility:** Tinkers' full tier (no NF 1.21.1), Ender's Bags, Jet Packs, Flint & Steel+, Hopper Ducts, Decorative Barrels, Building Wands (redundant)
**Tier 3 QoL:** JER (JEI-dep; we use EMI), EMI Trades, Mod Menu (Fabric-only; use Configured)
**Tier 13 Create addons:** Create: Interactive (VS2 dep + Mineflayer-breaking), Create: Metallurgy (1.20.1 only), Create: Utilitarian (not on 1.21.1), Create: Copperworks (not on 1.21.1), Create: Symmetry (doesn't exist as addon)

### PIN versions (mandatory)

- **Balm: 21.1.x** (not 21.11.x)
- **Farmer's Delight: 1.2.9** (1.2.11 breaks Create: Integrated Farming)
- **Supplementaries: current / ≥3.0.40** (older builds break Mineflayer)
- **AppleSkin: 3.0.5** (not 3.0.8)
- **Macaw's suite: same minor** across all Macaw's mods

### PICK ONE

- SereneSeasons XOR Agricarnation (crop-tick hook collision)
- The Graveyard XOR Graveyard Overhaul
- Falling Leaves (NF-native) over Cloth Falling Leaves
- Chisel Reborn over Chisel Modern
- MVS as canonical villages (disable T&T + RS village variants)

### TUNE SPACING in worldgen configs

- When Dungeons Arise: separation ≥40 chunks
- Towns & Towers villages: separation ≥32 chunks
- Structory Cities: separation ≥20 chunks

### SWAP recommendations

- **Tinkers' → Silent Gear** (already locked; fills the tool-tiering niche)
- **Lambdynlights-via-Connector → Dynamic Lights Reforged (NF native)** or Sodium Dynamic Lights NF port
- **Mod Menu → Configured**
- **Trinkets (Fabric) → Curios (NF native)** — Connector bridges Trinkets-based accessory mods

---

## Sweep 5 — Tech + Storage + Difficulty (returned, WebSearch-verified)

### Tier 6 Storage

| Mod | NF 1.21.1 version | Verdict |
|---|---|---|
| Sophisticated Storage | 1.21.1-1.5.30.1535 | SHIP |
| Sophisticated Storage Reloaded | — | SKIP (redundant) |
| Sophisticated Backpacks | 1.21.1-3.25.37.1646 | SHIP |
| Sophisticated Core | bundled | SHIP |
| **Iron Chests** | Only 1.21.11 in 2026 | MAYBE (legacy 16.2.x) |
| **Shulker Loot Drops → Shulker Drops Two** | 1.21.0-3.3 | SHIP (renamed) |
| Chipped Shulkers | covered by Chipped | MAYBE |
| **Easy Shulker** | — | SKIP |
| Ender Storage | 1.21.1-2.13.0.191 | SHIP |
| Travel Anchors (De-Os fork) | unofficial | MAYBE |
| Trashcans | No 1.21.1 | MAYBE |
| RFTools Storage | 1.21-6.0.4 | SHIP |
| Modular Routers | 1.21.1-13.2.4 | SHIP |

### Tier 11 Difficulty

| Mod | NF 1.21.1 version | Verdict |
|---|---|---|
| **Scaling Health → Silent's Power Scale** | 0.3.1 (Apr 2026) | **SWAP** — SH discontinued |
| **Apotheosis modules-only** | 1.21.1-8.5.2 | SHIP — keep Apothic Attributes + Spawners + Apotheosis core; **drop Apothic Enchanting** |
| **Regional Difficulty+** | — | SKIP (not a mod) |
| Hordes vs Undead Nights | Hordes 1.21.1-1.6.2b / UN 1.1.0 | MAYBE (pick one) |
| **In Control!** | 1.21-10.2.6 | SHIP — **REQUIRED if Hordes ships** |

### Tier 14 Standalone Tech — major drops

| Mod | NF 1.21.1 version | Verdict |
|---|---|---|
| Immersive Engineering | 12.4.2-194 | SHIP |
| Immersive Petroleum | 4.4.1-37 | SHIP (IE ≥12.4 dep) |
| **Immersive Geology** | **No 1.21.1 port** | **SKIP** |
| Industrial Foregoing | (present) | SHIP |
| **Thermal Foundation + Expansion + Dynamics + Innovation + Cultivation + Integration** | **NONE on NF 1.21.1** as of Apr 2026 (TeamCoFH has not ported) | **SKIP ENTIRE SERIES** (6 mods lost) |
| Applied Energistics 2 | 19.2.17 | SHIP (primary over RS) |
| AE2 Additions | 1.21.1-6.0.2 | SHIP |
| AE2 Things | 1.21.1 NF port | SHIP |
| **Refined Storage** | 2.0.2 / 3.0.0-beta.3 | **SKIP** (AE2 chosen) |
| Flux Networks | 1.21.1 | SHIP |
| XNet | 1.21.1 (McJty) | SHIP |
| Laser IO | 1.21.1-1.9.11 | SHIP (may overlap Mek cables) |
| **Pipez** | Published 1.21.3, no 1.21.1 | MAYBE |
| RFTools series (Base/Power/Builder/Storage/Utility/Control) | 1.21-line | SHIP all 6 |
| Actually Additions | No clean 1.21.1 NF release | MAYBE |
| Modular Machinery Reborn | 3.0.12 + MMR-Mekanism 3.0.0 | SHIP |
| **Bigger Reactors** | No 1.21.1 port (stalled) | **SWAP to Extreme Reactors** |
| Ender IO | 1.21.1-8.2.6-beta | SHIP (beta — QA gate) |
| CC: Tweaked | 1.117.1 | SHIP |
| Advanced Peripherals | 0.7.61b | SHIP |
| **Advanced Rocketry** | AR3 fork only | MAYBE |
| **Ad Astra** | **No 1.21.1 NF** (upstream issues open) | **SKIP** — no AR fallback available |

---

## FINAL CONSOLIDATED SHORTLIST CHANGES (all 5 sweeps)

### SKIP — no NF 1.21.1 port / redundant / theme-conflict

- **Decoration:** Frame Up, Little Contraptions, Better With Art, Sit Everywhere, Painted World, Mana and Artifice Decor, Create: Deco Cannons (as named)
- **Mobs:** Untamed Wilds, **Epic Knights**
- **Food:** Pam's HarvestCraft 3
- **Storage:** Sophisticated Storage Reloaded, Easy Shulker
- **Adventure:** YUNG's Better End Island (Nullscape conflict)
- **Aesthetic:** Stay True, First-Person Animations (redundant)
- **Utility:** entire Tinkers' tier, Ender's Bags, Jet Packs, Flint & Steel+, Hopper Ducts, Decorative Barrels, Building Wands
- **QoL:** JER, EMI Trades, Mod Menu, Regional Difficulty+
- **Create addons:** Create: Interactive, Metallurgy, Utilitarian, Copperworks, Symmetry
- **Tech:** **Entire Thermal series (6 mods)**, Immersive Geology, Refined Storage, Bigger Reactors, Ad Astra

### SWAP (NF-native successor)

- Scaling Health → **Silent's Power Scale 0.3.1**
- Bigger Reactors → **Extreme Reactors**
- Lambdynlights → **Dynamic Lights Reforged (NF native)**
- Mod Menu → **Configured**
- Trinkets → **Curios**
- Chisel Modern → **Chisel Reborn**
- Shulker Loot Drops → **Shulker Drops Two**
- Tinkers' → **Silent Gear** (already locked)
- Ad Astra → no fallback (AR3 MAYBE)

### PIN exact versions

- **Balm: 21.1.x** (not 21.11.x)
- **Farmer's Delight: 1.2.9** (1.2.11 breaks Create: Integrated Farming)
- **Supplementaries: ≥3.0.40** (pre-3.0.30 breaks Mineflayer)
- **AppleSkin: 3.0.5** (not 3.0.8)
- **IE: 12.4.2-194 + IP: 4.4.1-37**
- **Apotheosis: modules-only — drop Apothic Enchanting** for no-magic
- **Macaw's suite: same minor across all Macaw's**
- **Xaero pair, EMI family, RFTools series: version-line aligned**

### PICK ONE

- SereneSeasons XOR Agricarnation
- The Graveyard XOR Graveyard Overhaul
- Hordes XOR Undead Nights
- Falling Leaves (NF-native) over Cloth Falling Leaves
- Chisel Reborn over Chisel Modern
- AE2 over Refined Storage
- MVS as canonical villages (disable T&T + RS variants)

### TUNE spacing

- WDA: separation ≥40 chunks
- T&T villages: ≥32 chunks
- Structory Cities: ≥20 chunks

### CRITICAL IF-THEN

- Ship Hordes ⇒ **REQUIRE In Control!** (bot swarm survival)
- Ship AE2 ⇒ **drop Refined Storage**
- Ship Twilight Forest ⇒ **gate portal recipe in FTB Quests**
- Ship Carry On ⇒ **blacklist Create BEs in config**
- Ship IE ⇒ **pin ≥12.4** (IP dep)

### Revised ship count estimate

- Original strawman: ~281 mods
- Definite SKIPs: ~40 mods removed
- SWAPs: ~8 (net zero on count)
- MAYBEs (QA-gated): ~15
- **Revised estimate: ~240 mods SHIP at Release, ~15 under QA watch, 40 removed**
