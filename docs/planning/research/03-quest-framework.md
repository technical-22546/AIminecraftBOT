# Warm Iverson - Questbook Framework Research (April 2026)

_Full research report. Summary folded into `../warm-iverson.md` Workstream B._

## 1. Comparison Matrix

| Feature | **FTB Quests** | **Heracles / Odyssey Quests** | **BetterQuesting** | **Questify** |
|---|---|---|---|---|
| Loader / MC | NeoForge+Fabric 1.20.1, 1.21.1, 1.21.11 (2111.1.5, Apr 2026) | NeoForge+Fabric+Forge 1.20.1, 1.21.x | Forge 1.20.1 only (4.0.71 Mar 2025); no 1.21 | Fabric+NeoForge 1.21.x (0.6-SNAPSHOT) |
| Authoring UX | In-game editor (cheats on), SNBT files on disk | JSON files in `config/`, minimal in-game editor | In-game editor, JSON export | In-game editor + drag-drop theme editor + in-game KubeJS editor |
| Task types | Item, kill, visit dimension/biome, advancement, stat, XP, fluid, energy, checkmark, custom (via XMod/KubeJS) | Item, kill, biome, advancement, structure, composite, scripted | Retrieve, craft, hunt, location, NBT, scripted | Full FTBQ parity + JSON advancement conditions + scripted |
| Reward types | Items, XP, commands, choice, random pool, custom (KubeJS) | Items, XP, commands, custom | Items, commands, choice | Items, commands, unlocks, KubeJS callbacks |
| Dependency graph | AND/OR/XOR, hide/lock, chapter locks, dependency groups | Tree + parallel branches, prerequisite lists | Prerequisites, logic quest type | FTBQ parity + JSON-advancement gating |
| KubeJS integration | Via **FTB XMod Compat** (required >=1902.5.0); custom tasks w/ `event.setCheckTimer`/`check` | Native event hooks; less mature surface | Community-written bridges | First-class; built-in KubeJS editor |
| Portability | SNBT is widely tooled; Odysseus converts FTBQ -> Heracles | Native JSON, converter from FTBQ/HQM | JSON (ARR license since 4.0) | Auto-imports FTBQ packs |
| Performance | Known issue: 500-quest load >30 min; crafting hook hot; needs **FTB Quests Optimizer** + **Quests Freeze Fix** | Lighter; fewer reports of pathological lag | Aging, but small packs fine | Dynamic search, better scaling claims |
| Community | Huge (~192M dl); Discord; docs at docs.feed-the-beast.com | Active (Terrarium Earth); smaller | Slow; no 1.21 port visible April 2026 | Small but active; snapshot builds |
| License | LGPL (friendly) | MIT-ish | **ARR post-4.0** (risk) | Open, see Modrinth |

## 2. Recommendation

**Primary: FTB Quests + FTB XMod Compat + FTB Quests Optimizer + Quests Freeze Fix.**
For a ~500-mod Create pack with tight dependency webs, FTBQ is the only mature option with a proven tier-gate model, team progression for your 10-player SMP, and battle-tested KubeJS surface for custom tasks (observation, NBT, multi-block Create contraptions). Performance pitfalls are real but fully mitigated by the two known optimizer mods. Loader-wise it is the safest bet on both 1.20.1 and 1.21.x.

**Runner-up: Questify.** Its FTBQ auto-import, JSON-advancement conditions, and in-game KubeJS editor map directly onto your web-UI roadmap. Use it as a hot backup and, if Questify's web-editor ambitions materialize, migrate mid-project. Avoid BetterQuesting (no 1.21, ARR license); avoid Heracles as primary - its branching/tree model is narrative-oriented, which contradicts your "purely mechanical" requirement.

## 3. Authoring Pipeline (Solo Dev)

1. **Source of truth:** `quests/` SNBT files committed to git. Never treat the in-game editor as authoritative - export every session.
2. **Templating layer:** Write a Node/Python generator that reads a YAML tier spec (`tier: 2, chapter: create-brass, deps: [...], tasks: [{type: item, id: "create:brass_ingot", count: 8}]`) and emits SNBT. This lets you regenerate 300 quests from a 1-page spreadsheet.
3. **KubeJS custom tasks:** One `kubejs/server_scripts/quests/custom_tasks.js` per quest category, registering observation/NBT/Create-specific checks via `FTBQuestsEvents.custom_task`.
4. **Recipe<->quest binding:** Generate KubeJS recipe IDs and FTBQ item-task IDs from the same YAML - guarantees no drift.
5. **Validation CI:** Lint script that walks the SNBT tree and fails on dangling dependencies or orphan chapters before commit.
6. **Web-UI roadmap:** Short term, render the YAML with a static SvelteKit/Next dashboard; medium term, wrap it in a quest-graph editor (Cytoscape.js) that writes YAML; long term, evaluate Questify's web editor or fork Odysseus's converter.

## 4. Risks

- **Quest-load lag on server start** with 500+ quests is the single biggest thing that will bite a 10-player SMP. Bake optimizer mods in from day one and load-test monthly.
- **FTB XMod Compat breakage**: version-coupled with FTBQ and KubeJS; a single mismatched bump can nuke every custom task. Pin exact versions.
- **Loader decision (1.20.1 vs 1.21.x)** cascades here - BetterQuesting dies on 1.21; Heracles/FTBQ/Questify all survive. Delay any BQ consideration until loader is decided.
- **AI-assisted quest authoring is not production-ready** (Skywork/Kodari generate books/datapacks, not FTBQ SNBT). Don't budget for it; treat as experimental only.
- **ARR license creep** if you mix in BetterQuesting 4.x addons - avoid.

## 5. Open Questions

1. Loader+MC version lock-in date? (Gates BQ out and affects optimizer compat.)
2. Target quest count (200? 500? 1000?) - changes performance mitigation budget.
3. Is team-shared progress required (FTBQ native) or per-player only?
4. Web-UI timeline: MVP by Release, or post-Release v1.1?
5. Budget for a custom Cytoscape editor vs waiting on Questify's web tool?
6. Will quests reference proprietary/ARR assets that limit pack redistribution?

## Sources

- [FTB Quests (NeoForge) - CurseForge](https://www.curseforge.com/minecraft/mc-mods/ftb-quests-forge)
- [FTB-Quests CHANGELOG - GitHub](https://github.com/FTBTeam/FTB-Quests/blob/main/CHANGELOG.md)
- [FTB Quests docs](https://docs.feed-the-beast.com/mod-docs/mods/suite/Quests/)
- [FTB XMod Compat - GitHub](https://github.com/FTBTeam/FTB-XMod-Compat)
- [FTB XMod Compat - KubeJS wiki](https://kubejs.com/wiki/addons/ftb-xmod-compat)
- [FTB Quests Integration - latvian.dev](https://mods.latvian.dev/books/kubejs/page/ftb-quests-integration)
- [FTB Quests 500-quest load perf issue](https://github.com/FTBTeam/FTB-Mods-Issues/issues/1440)
- [FTB Quests crafting-lag bug](https://github.com/FTBTeam/FTB-Mods-Issues/issues/1063)
- [FTB Quests Optimizer - CurseForge](https://www.curseforge.com/minecraft/mc-mods/ftb-quests-optimizer)
- [Quests Freeze Fix - CurseForge](https://www.curseforge.com/minecraft/mc-mods/ftb-quests-freeze-fix)
- [Heracles / Odyssey Quests - GitHub](https://github.com/terrarium-earth/Heracles)
- [Odyssey Quests - Modrinth](https://modrinth.com/mod/odyssey-quests)
- [BetterQuesting - CurseForge](https://www.curseforge.com/minecraft/mc-mods/better-questing)
- [Questify - Modrinth](https://modrinth.com/mod/questify)
- [Questify 0.6-SNAPSHOT notes](https://modrinth.com/mod/questify/version/L76Ujo6L)
- [12 Popular Minecraft Quest Mods - CurseForge blog](https://blog.curseforge.com/best-minecraft-quests-mods/)
- [Kodari AI datapack generator 2026](https://kodari.ai/blogs/minecraft-datapack-generator-ai)
