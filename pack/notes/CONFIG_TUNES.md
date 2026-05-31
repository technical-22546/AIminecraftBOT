# Config tunes — Tier 2 queue

Apply after first boot succeeds; iterate during v0 play.

## Worldgen spacing

- `config/yungs-better-dungeons-*.toml` — leave default
- `config/towns_and_towers.toml` — village separation ≥32 chunks; **disable T&T village variants** (MVS = canonical villages)
- `config/structory.toml` — Structory Cities separation ≥20 chunks
- `config/when-dungeons-arise-*.toml` — separation ≥40 chunks
- `config/repurposed_structures.toml` — **disable RS village variants**
- `config/twilight_forest.toml` — gate portal-recipe access via FTB Quests Chapter 5 unlock; do NOT block worldgen of TF dim

## Mod-specific tunes

- `config/distantHorizons.toml` — disable LOD generation for The Beyond dim (ATM); leave Nether + Other + End as-is
- `config/quark-common.toml` — disable modules that overlap Chipped (block variants), Supplementaries (signposts/safes/clocks/globes/lanterns), Amendments (extensions)
- `config/almostunified.json` — preserve ATM materials (Allthemodium/Vibranium/Unobtainium) as distinct tags; **do NOT** add `c:ingots/vibranium` unification because Immersive Ores ships its own Vibranium
- `config/apotheosis/` — all 4 modules enabled (Attributes, Spawners, Enchanting, Apotheosis-core)
- `config/serene-seasons.toml` — N/A (mod dropped)
- `config/sound-physics-remastered.toml` — leave default; tune if perf drops in v0
- `config/physics-mod-free.toml` — disable ragdoll + snow layers (Create contraption perf concern per Sweep 2)

## KubeJS scripts (G workstream output — defer until G kicks off)

- `kubejs/server_scripts/chunk_claim_purchase.js` — FTB Chunks claim event → Numismatics coin charge (D.7 economic-zone model)
- `kubejs/server_scripts/joe_journal.js` — Joe AI per-day journal entry serialization (D.6 diegetic memory)
- `kubejs/server_scripts/atm_create_recipes.js` — ATM Vibranium/Piglich → Create mill recipes (F2)
- `kubejs/server_scripts/immersive_ores_vibranium_remap.js` — Immersive Ores Vibranium → distinct tag (resolves AllTheModium clash)
- `kubejs/server_scripts/quests/` — FTB Quests custom tasks per XMod Compat

## Carry On

- `config/carryon.toml` — blacklist Create BEs (kinetic_shaft, encased_shaft, fan, all Create BlockEntity registry entries) to prevent contraption breakage on pickup

## Pterodactyl egg

- Memory: 24 GB
- Startup command: `java @startup-flags.txt -jar neoforge-1.21.1-server.jar nogui`
- Auto-restart on crash: enabled (per C self-healing layer)
- Backups: 4hr × 6 rolling per day per D.16
