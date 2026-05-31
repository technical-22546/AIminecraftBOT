# Pack assembly checklist — from "decisions locked" to "play-testable today"

## Phase 1 — Manifest assembly (~half-day)

- [ ] Create CurseForge modpack project (name: "Create Remastered" or working name)
- [ ] Add NeoForge 1.21.1 as modLoader
- [ ] Add ~314 mods from `docs/planning/a2-mod-shortlist-v1-locked.md`
  - [ ] Use CF mod-search; for each tier, batch-add via "modpack mod browse" filtered to NF 1.21.1
  - [ ] Apply mandatory version pins from `notes/PIN_VERSIONS.md`
  - [ ] Skip dropped mods (FTB JEI Extras, JER, JourneyMap, Refined Storage, RS integrations, Pickup Notifier, Tinkers' family, Thermal series, Bigger Reactors, Ad Astra, Immersive Geology, Mystical Agriculture trio, IceAndFire CE, full Ars suite, etc.)
- [ ] Verify resolver: no missing deps, no version conflicts flagged by CF
- [ ] Export manifest to `pack/manifest.json`
- [ ] Export modlist.html to `pack/modlist.html`

## Phase 2 — Server deployment (~half-day)

- [ ] Create LXC on Proxmox (12-16 GB JVM heap → use 24 GB → 32 GB container; 4-6 vCPU; 80 GB disk)
- [ ] Install Pterodactyl Wings on LXC
- [ ] Create new server in Pterodactyl panel using "NeoForge" egg
- [ ] Upload pack ZIP (mod jars + overrides) to server
- [ ] Configure startup command: `java @startup-flags.txt -jar neoforge-1.21.1-server.jar nogui` (point to `pack/server/startup-flags.txt`)
- [ ] Apply `pack/server/server.properties` (seed locked at 5252266024153750674)
- [ ] Set whitelist.json with your initial trusted player IDs
- [ ] Start server; verify no crash, no missing deps

## Phase 3 — Single-player smoke test (~1 hour)

- [ ] Export pack as Prism instance
- [ ] Open Prism, install instance, launch client
- [ ] Connect to your Proxmox server
- [ ] Walk into the world; confirm DH renders, Tectonic terrain visible
- [ ] Open FTB Quests; verify Chapter 1 (Survival start) opens
- [ ] Place a Create kinetic block; verify rotation works
- [ ] Open SDMShop; verify admin-shop opens
- [ ] Place an FTB Chunks claim; verify claim hook fires
- [ ] Spawn Vibranium ore via creative; verify Immersive Ores' Vibranium and AllTheModium Vibranium are visually distinct (will fix via KubeJS later if same texture/name)

## Phase 4 — Initial friend join (~1 hour)

- [ ] Add 1-2 friend Microsoft UUIDs to whitelist.json + ops.json
- [ ] Have friend(s) install pack via Prism
- [ ] Friend(s) connect, walk world together, basic gameplay
- [ ] Catalogue issues: crashes, missing recipes, perf complaints, mod-conflict behaviors

## Phase 5 — Iterate (v0 phase begins)

- [ ] Apply Tier 2 config tunes per `CONFIG_TUNES.md`
- [ ] Tune worldgen spacings if structure density feels off
- [ ] Tune AlmostUnified rules
- [ ] Resolve Immersive Ores Vibranium clash via KubeJS (G workstream)
- [ ] Author SDMShop initial price list (~50 anchor items)
- [ ] When ready: kick off Workstream E (existing mod port) and Workstream G (KubeJS scripts)

## Estimated time to first play-test

**1-2 days of focused work** assuming the Compat Pass 1 conclusions hold. Most risk is in Phase 1 manifest assembly — a single mod-pair conflict can take hours to debug.

## After play-test passes

- Workstream A complete (final final shortlist confirmed by play)
- Workstream C primary deployment complete
- Workstream E unblocks (the existing mod's port can start in parallel with B/G content work)
- Workstream J unblocks (web app + Discord bot scaffold for control plane)
- Workstream D still parked pending the 1-peer-bot constraint resolution
