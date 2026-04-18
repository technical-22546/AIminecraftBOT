# Warm Iverson: Loader + MC Version Decision (2026-04-18)

_Full research report. Summary folded into `../warm-iverson.md` Workstream A._

## 1. Compatibility matrix

Legend: A = Available and actively maintained in 2026, P = Partial (works via fork / stale port / compat layer / known issues), M = Missing.

| Mod | NeoForge 1.21.1 | NeoForge 1.20.1 | Forge 1.20.1 | Fabric 1.21.x |
|---|---|---|---|---|
| Create | A (6.x, Jan 2026) [1] | A | A | M – Fabric port stuck at 1.20.1 [1] |
| Create: Aeronautics | A (1.0.2, 2026-04-17) [2] | P (older branch) | P | M [2] |
| KubeJS | A (2101.7.2, Nov 2025) [3] | A | A (2001.6.5) [3] | P – Fabric build lags behind NeoForge [3] |
| Distant Horizons | A (2.2.1-a) [4][8] | A | A | A |
| Oculus / Iris | P – Iris 1.8.1+ on NeoForge works, Oculus fork trails [5][8] | A (Oculus 1.7 + DH 2.1 only) [5] | A (Oculus 1.8.0) [5] | A (Iris native on Fabric) [5] |
| Powah! (Rearchitected) | A (6.2.8, Jan 2026) [6] | A | A | M (Rearchitected is NF-first) |
| Mekanism | A (10.7.19, Apr 2026) [6] | A | A | M |
| FTB Quests | A (2101.1.2) [7] | A | A | P (Fabric builds exist but lag) |
| Heracles | A | A | A | A (multi-loader) [7] |
| Villager Recruits | P – author ships 1.20.1/1.19.x Forge only; no 1.21.1 NF build visible as of Apr 2026 [9] | A (1.13.5, Apr 2026) [9] | A [9] | M |
| MineColonies | A (1.1.1285, Mar 2026; snapshot 1.1.1300 Apr 12 2026) [10] | A | A | M |

The single blocker across versions is **Recruits on 1.21.1**: talhanation is still shipping 1.20.1 Forge as the live branch in April 2026 [9].

## 2. Ecosystem / performance mods

- **Fabric 1.21.x**: Sodium, Lithium, Starlight, FerriteCore, Krypton — the canonical high-performance stack. Cleanest rendering + tick-logic combo available.
- **NeoForge 1.21.1**: Embeddium (Sodium port), Radium Reforged (Lithium port), Canary, FerriteCore, ModernFix. Embeddium supports NeoForge 1.20.1+ through 1.21.x [11]. Iris 1.8.1-neoforge now ships natively alongside Embeddium, replacing the old Oculus-only path [5][8].
- **Forge 1.20.1**: Rubidium/Embeddium + Oculus 1.8 + Radium/Canary. Most mature Forge performance stack ever shipped, but it is 1.20.1-locked.
- **NeoForge 1.20.1**: Same options as Forge 1.20.1 in practice; little reason to pick it over Forge at that MC version.

## 3. Bot-client viability

- **Mineflayer** is a protocol-level Node.js client — it talks raw Minecraft protocol and is **loader-agnostic** on the server side. It supports MC 1.8–1.21.11 [12]. Any server (Fabric, NeoForge, Forge, Paper) that doesn't add custom packet gating will accept Mineflayer bots.
- **Mindcraft** wraps Mineflayer + LLMs; it officially supports up to 1.21.11 with 1.21.6 recommended [12]. Also loader-agnostic.
- **Voyager** historically required specific Fabric mods (chest-API, mineflayer-collectblock etc.) tuned to 1.19/1.20 — running it on NeoForge 1.21.1 is untested terrain and will need re-porting of its helper mods [12].
- **NPC-body hybrids**: On NeoForge 1.21.1 you can use NeoBots / PVP Bots-style in-world dummies [12]; on Fabric 1.21.x, carpet-bot / fabric-carpet gives you in-world bodies that Mineflayer can steer.
- **Headless clients**: HeadlessMC supports Vanilla/Fabric/Forge/NeoForge launches [12]; for a purely headless spectator/helper client, Fabric has the richest ecosystem. NeoForge has no drop-in equivalent of fabric-headless-client, but for a 10-player SMP you shouldn't need it — Mineflayer over the protocol is enough.

## 4. Recommendation

**Primary: NeoForge 1.21.1.** Confidence: **High (~80%)**.

Rationale: every load-bearing content mod (Create, Aeronautics, KubeJS, Powah, Mekanism, FTB Quests, MineColonies, DH + Iris) has a shipping 1.21.1 NeoForge build dated within the last 4 months [1][2][3][6][7][8][10]. Fabric 1.21 is disqualified because Create is frozen on Fabric 1.20.1 [1] — that alone kills the theme. Forge 1.20.1 is a safe harbour but locks you out of 1.21's worldgen, data components, and the full NeoForge 2026 mod release cadence, which would be a regrettable floor for a release 2–4 months out that you'll want to support for a year.

**Biggest risk of the primary pick**: **Villager Recruits has no 1.21.1 NeoForge port as of April 2026** [9]. The author is actively shipping updates (April 10 2026) but on the 1.20.1 branch. You'd need to (a) wait for a port, (b) sponsor/fork one, or (c) substitute (Tough As Nails armies / Guard Villagers / a KubeJS-scripted replacement). Secondary risk: DH + Iris on NeoForge 1.21.1 still has known breakage cycles whenever DH ships [4][5][8] — budget for shader-pack version pinning.

**Runner-up: Forge 1.20.1.** Confidence: **Medium (~60%)**.

Choose this only if Recruits is non-negotiable and the 2 – 4-month window can't absorb a substitution. Every mod on your list has a battle-tested 1.20.1 Forge build, Oculus 1.8 + DH 2.x is the most-documented shader stack in existence, and KubeJS 2001.6.5 is mature [3][5]. Cost: you ship a modpack on a version the wider ecosystem is leaving behind, and the Fabric 1.21.1 AI mod you already have will need a downport.

## 5. Open questions

1. **Recruits**: can you live without it (or commission/port it yourself) if the 1.21.1 NF build doesn't land in the next 60 days?
2. **Your existing AI mod on Fabric 1.21.1** — is the plan to rewrite it on NeoForge, or run bots purely over the Mineflayer protocol (which removes the loader-matching requirement)?
3. **Shaders**: do the 10 SMP players require shaders + DH simultaneously, or is DH-only acceptable? That decides how aggressively you track Iris-NeoForge breakage.
4. **Quest system**: FTB Quests or Heracles? Both viable on NF 1.21.1 [7]; Heracles is more "modern" and multi-loader, FTB has richer reward/team features.
5. **Server software**: Vanilla NeoForge server, or NeoForge + Sinytra-on-server hybrid to re-enable Fabric mods like Lithium server-side?
6. **Create: Aeronautics maturity**: 1.0.2 shipped a day ago (2026-04-17) [2] — do you want to ride the bleeding edge, or wait 30 days for a stability patch before committing the pack?

---

## Sources

- [1] [Create mod Development Status (wiki)](https://wiki.createmod.net/users/development-status); [Create on CurseForge](https://www.curseforge.com/minecraft/mc-mods/create)
- [2] [Create Aeronautics 1.0.2 for 1.21.1 (Modrinth)](https://modrinth.com/mod/create-aeronautics/version/HY8u0JqC); [GitHub repo](https://github.com/Modders-of-Create/Create-Aeronautics)
- [3] [KubeJS NeoForge 2101.7.2 (CurseForge)](https://www.curseforge.com/minecraft/mc-mods/kubejs/files/7278501); [KubeJS Forge 2001.6.5](https://www.curseforge.com/minecraft/mc-mods/kubejs/files/5853326)
- [4] [DH 2.2.0-a for 1.21.1 neo/fabric (Modrinth)](https://modrinth.com/mod/distanthorizons/version/2.2.0-a-1.21.1); [DH shader-compatibility gist](https://gist.github.com/Steveplays28/52db568f297ded527da56dbe6deeec0e)
- [5] [Oculus 1.8.0 (CurseForge)](https://www.curseforge.com/minecraft/mc-mods/oculus/files/6020952); [DH 2.2 + Oculus compat thread](https://www.answeroverflow.com/m/1312768256062521425)
- [6] [Powah! 6.2.8 NeoForge 1.21.1](https://modrinth.com/mod/powah/version/6.2.8-neoforge); [Mekanism 1.21.1-10.7.19](https://www.curseforge.com/minecraft/mc-mods/mekanism)
- [7] [FTB Quests 2101.1.2 NeoForge 1.21.1](https://www.curseforge.com/minecraft/mc-mods/ftb-quests-forge/files/6083251); [Heracles (GitHub)](https://github.com/terrarium-earth/Heracles)
- [8] [Distant Horizons & Iris Shaders NeoForge 1.21.1 modpack](https://modrinth.com/modpack/distant-horizons-iris-shaders/version/0.1.1+1.21.1.neoforge)
- [9] [Villager Recruits on CurseForge (files)](https://www.curseforge.com/minecraft/mc-mods/recruits/files/all); [on Modrinth](https://modrinth.com/mod/villager-recruits)
- [10] [MineColonies files (CurseForge)](https://www.curseforge.com/minecraft/mc-mods/minecolonies/files/all)
- [11] [Embeddium (Modrinth)](https://modrinth.com/mod/embeddium); [NeoForge FPS/TPS mod guide](https://minestrator.com/en/blog/article/neoforge-performance-mods-minecraft-fps-tps)
- [12] [Mindcraft (GitHub)](https://github.com/mindcraft-bots/mindcraft); [Mineflayer (GitHub)](https://github.com/PrismarineJS/mineflayer); [HeadlessMC](https://github.com/headlesshq/headlessmc); [Voyager](https://github.com/MineDojo/Voyager)
