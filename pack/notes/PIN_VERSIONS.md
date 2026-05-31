# Mandatory version pins — apply during manifest assembly

Per Compat Pass 1 verification. **Do not let CurseForge auto-resolve to "latest" for these mods.**

| Mod | Version pin | Reason |
|---|---|---|
| **Balm (NeoForge)** | **21.1.x** | 21.11.x is 1.21.11-only |
| **Farmer's Delight** | **1.2.9** | 1.2.11 breaks Create: Integrated Farming (vectorwing #1266) |
| **Supplementaries** | **≥3.0.40** (current 3.5.34) | pre-3.0.30 sends malformed flute/globe packets that break Mineflayer (#1195) |
| **AppleSkin** | **3.0.5** | 3.0.8 is 1.21.11 |
| **Immersive Engineering** | **12.4.2-194** | IP 4.4.1-37 requires IE ≥12.4 |
| **Immersive Petroleum** | **4.4.1-37** | matches IE |
| **Sinytra Connector** | **2.0.0-beta.14+1.21.1-full** | latest stable beta (Feb 2026) |
| **Macaw's suite (all)** | **same minor across all Macaw's mods** | mismatches cause block-registry desync |
| **Xaero pair (Minimap + WorldMap)** | **same version-line** | waypoint sync only works when both match |
| **EMI family (EMI + EMI Loot + EMI Ores)** | **same minor (1.1.21+)** | recipe-screen consistency |
| **RFTools series (Base/Power/Builder/Storage/Utility/Control)** | **same 1.21-line** | inter-mod registry deps |
| **Sophisticated Storage + Backpacks + Core** | matching set | shared core dep |

## Version conflicts to actively avoid

- **DO NOT install Lithium via Sinytra** — use Radium Reforged (NF-native Lithium port) instead
- **DO NOT install both AE2 and Refined Storage** — AE2 chosen; RS dropped to avoid dual-network confusion
- **DO NOT install both SereneSeasons and Agricarnation** — they collide on crop-tick hooks (we dropped both)
- **DO NOT install JEI alongside EMI** — pick EMI; drop FTB JEI Extras, JER

## Mod-pair compat caveats

- **Twilight Forest portal recipe** — gate via FTB Quests for progression pacing (no ATM dim collision)
- **AE2 + Integrated Dynamics** — disable spatial transport of ID networks (Cyclops #1410 crash)
- **Carry On** — blacklist Create BEs in config (avoid contraption breakage)
- **Quark** — disable modules overlapping Chipped, Supplementaries, Amendments
- **AlmostUnified** — config carefully; do NOT unify ATM materials (Allthemodium/Vibranium/Unobtainium must stay distinct from any other Vibranium-named ore like Immersive Ores')
- **Immersive Ores Vibranium tag conflict with AllTheModium** — resolve via KubeJS tag remap in v0
