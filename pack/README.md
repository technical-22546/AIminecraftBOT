# Create Remastered — Pack assembly

Per Workstream F Phase 1 (combined monorepo), the modpack manifest + server config lives in this directory.

## Layout

```
pack/
├── README.md                  this file
├── manifest.json              CurseForge modpack manifest (to be assembled)
├── modlist.html               human-readable mod list (CF-generated)
├── overrides/                 config + resource pack overrides shipped with pack
│   ├── config/                per-mod configs (apply Tier 2 tunes here)
│   ├── kubejs/                KubeJS scripts (G workstream output lands here)
│   ├── resourcepacks/         custom Industrial/Gilded-Age textures (B workstream)
│   └── defaultconfigs/        default configs for fresh installs
├── server/
│   ├── server.properties      initial server config
│   ├── startup-flags.txt      Aikar's flags for 24GB heap + G1GC
│   ├── whitelist.json         whitelist placeholder
│   └── ops.json               ops list placeholder
└── notes/
    ├── CONFIG_TUNES.md        Tier 2 config-tune queue
    ├── PIN_VERSIONS.md        mandatory version pins from Compat Pass 1
    └── ASSEMBLY_CHECKLIST.md  step-by-step pack assembly

```

## Locked decisions (2026-04-18)

- **Manifest format:** CurseForge `manifest.json`
- **Loader:** NeoForge 1.21.1 (specific version per `manifest.json/minecraft/modLoaders`)
- **Server platform:** Pterodactyl on Proxmox LXC; vanilla NeoForge egg
- **JVM heap:** 24 GB G1GC + Aikar's flags
- **Server world seed:** `5252266024153750674` (Sbeev's pack seed)
- **view-distance:** dynamic 7–20
- **simulation-distance:** 5
- **max-players:** 8
- **Whitelist:** enabled; populated during initial setup
- **Mod count:** ~314 per `docs/planning/a2-mod-shortlist-v1-locked.md`

## Outstanding before first boot

- Assemble `manifest.json` from v1 shortlist (CurseForge project IDs + file IDs)
- Decide which mods need custom configs in `overrides/config/`
- Apply mandatory version pins (see `notes/PIN_VERSIONS.md`)
- Verify NF 1.21.1 + Sinytra Connector + all locked deps resolve cleanly

## Assembly path (CurseForge route)

1. Create a CurseForge modpack project ("Create Remastered" or working title)
2. Add ~314 mods from v1 list one-by-one (or via CF's mod-search + select-all-for-version)
3. Apply pinned versions where Compat Pass 1 dictates (Balm 21.1.x, FD 1.2.9, Supplementaries ≥3.0.40, AppleSkin 3.0.5, IE 12.4.2, etc.)
4. Export pack manifest via CF web → save to `pack/manifest.json`
5. Copy any pack-specific overrides to `pack/overrides/`
6. Import to Prism for client launch; Pterodactyl egg for server
