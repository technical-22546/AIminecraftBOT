# NeoForge 1.21.1 + Sinytra + Mineflayer Compatibility Report (April 2026)

_Layer-2 re-research (R2) run 2026-04-18. Summary folded into `../warm-iverson.md` Workstreams A + D._

## 1. Compat Verdict

**Conditionally viable** for a 10-player SMP with 3+ Mineflayer bots, but not a "drop-in" stack. NeoForge 1.21.1 is the Sinytra team's primary supported target and still receives fixes ([Sinytra FAQ](https://connector.sinytra.org/faq)). Mineflayer works against NF 1.21.1 *vanilla-protocol* surface, but requires workarounds in the 1.21 configuration phase. Treat the stack as "production-viable with a hardening checklist," not "production-ready out of the box."

## 2. Known Issues (severity / workaround)

| # | Issue | Sev | Workaround |
|---|---|---|---|
| 1 | Mineflayer sends physics packets during 1.21 **configuration phase** → server kicks bot with "internal error." | High | Start bot with `physicsEnabled: false`, re-enable on `spawn` event ([issue #3776](https://github.com/PrismarineJS/mineflayer/issues/3776)). |
| 2 | `Failed to decode packet clientbound/minecraft:custom_payload` when mods send unknown plugin channels (JEI, Create, Supplementaries all observed). | High | Catch & ignore unknown channels in `client.on('error')`; register no-op handlers via node-minecraft-protocol's custom channel API ([mineflayer #3663](https://github.com/PrismarineJS/mineflayer/issues/3663), [Supplementaries #1195](https://github.com/MehVahdJukaar/Supplementaries/issues/1195)). |
| 3 | **Lithium (Fabric) via Sinytra is NOT supported.** Sinytra explicitly excludes Sodium/Lithium; native Forge ports are required. | Critical | Replace with **Radium Reforged 0.13.x+1.21.1** (direct Lithium port for NeoForge) ([CurseForge](https://www.curseforge.com/minecraft/mc-mods/radium-reforged)). Also consider **Canary** + **ScalableLux** ([Minestrator perf guide](https://minestrator.com/en/blog/article/neoforge-performance-mods-minecraft-fps-tps)). |
| 4 | NF `FrozenRegistrySyncComplete` disconnect leaves registries in broken state — relevant when bots reconnect in a loop. | Med | Don't hot-reconnect faster than 5 s; let the client fully dispose ([NeoForge #2950](https://github.com/neoforged/NeoForge/issues/2950)). |
| 5 | Vanilla server connection throttle (`settings.connection-throttle` ~4000 ms) drops rapid bot reconnects. | Med | Stagger bot logins ≥1500 ms apart; set `connection-throttle=0` in `server.properties` for trusted local bots ([mineflayer #1456](https://github.com/PrismarineJS/mineflayer/issues/1456)). |
| 6 | Sinytra beta.14 has reports of mixin crashes from specific Fabric mods (Mini Tardis, Lavender + FFAPI). | Low | Pin tested mod versions; watch [Sinytra issues](https://github.com/Sinytra/Connector/issues). |
| 7 | NeoForge networking rework (Common Network Protocol V2) changes login handshake — generic MCProtocolLib clients OK, but Velocity in front of NF sometimes throws "Incompatible client." | Med | Connect Mineflayer direct to NF port, skip Velocity ([Velocity #1511](https://github.com/PaperMC/Velocity/issues/1511)). |
| 8 | Create Aeronautics visual glitches with Iris shaders — no bot impact. | Low | Cosmetic only. |

## 3. Recommended Sinytra Version

**`connector-2.0.0-beta.14+1.21.1-full`** (Feb 2026, latest as of research) from [Modrinth](https://modrinth.com/mod/connector/version/2.0.0-beta.14+1.21.1). Ships Forgified Fabric API bundled. Ops tax in the field: ~15–25 s extra startup, ~200–400 MB extra heap, occasional first-launch mixin patch retries; no reports of runtime TPS tax.

Config tweaks: leave defaults, but **do not** install Lithium, Sodium, or Indium via Sinytra; use NeoForge-native replacements.

## 4. Bot-Unfriendly Mods Blacklist (for the A-2 shortlist)

Avoid or config-gate these on a Mineflayer SMP:

- **Any client-list anticheat** — `Pixel's AntiCheat`, `EasyAntiCheat (Relay)`, `WatchDog Anti Cheat`, `ADM Anticheat`, `Mod Whitelist Forge/NeoForge`, `CheckYourMods`: all kick clients that don't announce a matching mod list ([CurseForge AC search](https://www.curseforge.com/minecraft/search?class=mc-mods&search=anticheat)). Mineflayer advertises vanilla brand — instant kick.
- **Grim-style movement AC** — not NF-native yet, but any port will break pathfinder ([mineflayer #3791](https://github.com/PrismarineJS/mineflayer/issues/3791)).
- **EpicFight** — heavy custom packets; MNPE crashes on NF 1.21.1 ([EpicFight #2153](https://github.com/Epic-Fight/epicfight/issues/2153)).
- **Supplementaries** (older builds) — custom-payload decode crash under Mineflayer ([#1195](https://github.com/MehVahdJukaar/Supplementaries/issues/1195)).
- **MidnightControls** — NF 1.21.1 network protocol error on join ([#326](https://github.com/TeamMidnightDust/MidnightControls/issues/326)).
- **Lithium (Fabric via Sinytra)** — replace with Radium Reforged.
- **JEI cheat-permission payload** — benign but spams decode errors; filter the channel ([JEI #3614](https://github.com/mezz/JustEnoughItems/issues/3614)).

Create, Create: Aeronautics, KubeJS, FTB Quests, Minecolonies, Guard Villagers, Distant Horizons, Iris, Embeddium, FTB Backups 2, and Spark have **no known Mineflayer-breaking packets** — safe to ship, with Minecolonies + Create requiring the integration datapack on server ([Create #4545](https://github.com/Creators-of-Create/Create/issues/4545)).

## 5. NeoForge Bot Detection

NF 1.21.1 has **no built-in bot detection or blocking**. Vanilla `whitelist.json` and `ops.json` apply normally; add bot UUIDs there. The only gating is via the optional anticheat mods listed above.

## 6. Concurrency

No Mineflayer-side hard cap; 3–10 bots on one host is routine. Bottlenecks: (a) vanilla connection throttle, (b) Node RSS ~80–150 MB per bot with chunk loading, (c) server view-distance × bot-count chunk load. Recommend dedicated chunk-loader area and `view-distance=8`, `simulation-distance=6` for bot region.

## 7. Open Questions

- Does Radium Reforged 0.13.x interact cleanly with Create: Aeronautics' physics contraptions under heavy load? (Not tested in public threads.)
- Will Sinytra 2.0 exit beta before NF 1.21.x reaches EOL, or will it ship as perma-beta?
- Does Distant Horizons server-side LOD streaming emit payloads that Mineflayer must explicitly ignore? Untested.
- Any KubeJS-defined custom packets in the target pack? Script-defined channels won't be in Mineflayer's protodef.

## Sources
- [Sinytra Connector FAQ](https://connector.sinytra.org/faq)
- [Sinytra Compatibility DB](https://connector.sinytra.org/compatibility)
- [Sinytra 2.0.0-beta.14 on Modrinth](https://modrinth.com/mod/connector/version/2.0.0-beta.14+1.21.1)
- [Sinytra GitHub issues](https://github.com/Sinytra/Connector/issues)
- [Radium Reforged — CurseForge](https://www.curseforge.com/minecraft/mc-mods/radium-reforged)
- [NeoForge perf mods guide — Minestrator](https://minestrator.com/en/blog/article/neoforge-performance-mods-minecraft-fps-tps)
- [Mineflayer config-phase physics kick — #3776](https://github.com/PrismarineJS/mineflayer/issues/3776)
- [Mineflayer custom_payload decode — #3663](https://github.com/PrismarineJS/mineflayer/issues/3663)
- [Mineflayer multi-bot connection throttle — #1456](https://github.com/PrismarineJS/mineflayer/issues/1456)
- [NeoForge Networking Rework](https://neoforged.net/news/20.4networking-rework/)
- [NeoForge registry sync disconnect bug #2950](https://github.com/neoforged/NeoForge/issues/2950)
- [Velocity + NeoForge incompatibility #1511](https://github.com/PaperMC/Velocity/issues/1511)
- [EpicFight NF 1.21.1 MNPE #2153](https://github.com/Epic-Fight/epicfight/issues/2153)
- [Supplementaries NF 1.21.1 protocol error #1195](https://github.com/MehVahdJukaar/Supplementaries/issues/1195)
- [MidnightControls NF protocol bug #326](https://github.com/TeamMidnightDust/MidnightControls/issues/326)
- [Create + Minecolonies server crash #4545](https://github.com/Creators-of-Create/Create/issues/4545)
- [Mineflayer vs Grim anticheat #3791](https://github.com/PrismarineJS/mineflayer/issues/3791)
- [Mod Whitelist Forge/NeoForge](https://www.curseforge.com/minecraft/mc-mods/mod-whitelist-forgeneoforge)
- [CheckYourMods](https://www.curseforge.com/minecraft/mc-mods/checkyourmods)
- [Distant Horizons 1.21.1 neo/fabric](https://modrinth.com/mod/distanthorizons/version/2.2.0-a-1.21.1)
