# Warm Iverson — Claude-Agented Create Modpack SMP

_Long-term research & planning strategy. Living document. No implementation happens in this channel — each workstream spawns its own execution plan in future sessions._

---

## Context

The real deliverable is a **product**: a reusable toolkit + methodology + agent framework for building large, Create-themed, ATM-scale modpacks with AI assistance. Anyone — solo hobbyist, friend-group owner, commercial pack publisher — should be able to adopt it and ship their own large pack.

The user's own project — a Create + Create: Aeronautics, ~500-mod, self-hosted 10-player SMP with multiple concurrent Claude-managed AI agents — is the **reference implementation and dogfood case**. It proves the product works end-to-end while shaping its requirements.

The user brings deep first-hand experience as a self-hoster, mod author, product owner, stakeholder, tester, and player, plus architecture experience at large enterprises — which is what makes a product of this scope plausibly shippable. The plan should be dense on requirements, integration detail, and reusable patterns — not strategic bullets.

This channel is reserved for planning only. Implementation is out of scope here; the deliverable is a **living research, product-design, and requirements document** that future sessions execute against and extend. Cross-mod integration specs, prompt libraries, skill/tool catalogs, test plans, and product-packaging decisions all live here or are linked from here.

---

## Product Framing

**Name (working title):** _Warm Iverson_ — a framework for building large Minecraft modpacks with AI.

**What it is:** an end-to-end kit for authoring, playtesting, operating, and iterating on ambitious modpacks. Not just a pack template — a full toolchain plus the human-plus-AI process around it.

**Who it's for (personas):**
- **P1 — Solo hobby author.** One person, one pack, a few friends. Needs opinionated defaults and low ceremony.
- **P2 — Friend-group owner (user's own case).** Self-hosted SMP for a small crew. Needs client/server split, basic ops, AI participation as co-players.
- **P3 — Serious/curated publisher.** Targets CurseForge/Modrinth audiences. Needs release discipline, license hygiene, content originality, QA rigor.

**Central thesis (why this product exists):**
ATM-scale, variety-first modpacks are _possible_ (ATM proves it) but today require ATM-level staffing and tacit knowledge to ship. Most teams that try fail on performance, cross-mod integration debt, or content scope. **Warm Iverson's job is to make that class of pack buildable repeatably by small teams** — opinionated methodology + reusable tooling + an AI co-workforce that helps author, test, and operate the pack.

**Core value propositions:**
- **Methodology** — the two-layer interview process (see below), codified as reusable templates so a new modpack author can sit down and drive themselves through it.
- **Reusable KubeJS/CraftTweaker libraries** for cross-mod integration patterns (shared tags, ore unification, Create-bridge recipes).
- **Texture prompt library + iteration harness** for repeatable "3D-looking" custom textures.
- **Questbook authoring patterns** and chapter templates.
- **AI agent framework** — multi-provider, multi-agent, with bot-player and helper/overseer modes, usable in any pack.
- **Sandbox + test-plan harness** — the same rig used to validate this project's agents can validate anyone's.
- **Skills & Tools library** that ships with the product and grows per pack.
- **Client/server pack split tooling** as a standard output.
- **Control plane UI** — a custom web app that is the primary interface to everything above (pack authoring, texture prompts, quests, agent management, server ops, test harness). Design sensibility similar to Hermes-style agent dashboards and OpenClaude/OpenCode-style workspaces, adapted for Minecraft modpack operations.
- **Discord control plane** — bidirectional Discord integration with per-environment channels (dev / sandbox / prod SMP) for admin, chat bridging, world-map posting, and AI-player management from chat.
- **Reference pack** — the user's Create SMP — shipped (or linked) as a worked example.

**Artifacts a consumer of the product receives:**
- Project-starter template (pack skeleton, directory layout, configs).
- KubeJS helper library (versioned).
- Texture prompt library + generation/evaluation scripts.
- Cross-mod integration pattern catalog with copy-pasteable recipes.
- Agent framework (mod + sidecar + CLI) with provider-agnostic configuration.
- Sandbox world templates + example test plans + rubrics.
- Questbook starter chapters keyed to Create / Create: Aeronautics progression.
- Docs site / recipe-book covering the methodology and tools.

**Non-goals (current scope boundary, revisitable):**
- A general-purpose Minecraft launcher — we integrate with existing launchers, we don't replace them.
- A mod hosting service — we rely on CurseForge / Modrinth / direct.
- A vanilla-server product — the product is opinionated toward large, Create-centric packs.

## Product Layer vs Reference Pack Layer

Every workstream below produces two tracks of output, and each session must be clear which track it is advancing:

| Workstream | Product-layer output (generic, reusable) | Reference-pack output (user's SMP, dogfood) |
|---|---|---|
| A Modpack Foundation | Compat-matrix methodology, category budgets, starter manifest schema, launcher integrations | Final loader/version pick, final mod list, this SMP's client/server manifests |
| B Content Layer | KubeJS/CraftTweaker patterns, texture prompt library, questbook templates | This pack's recipes, this pack's textures, this pack's questbook |
| C Hosting & Ops | Reusable systemd/Docker/runbook templates, monitoring presets | User's specific hardware config, backup target, network plan |
| D AI Agent Arch. | Provider-agnostic agent framework, Skills/Tools library, coordination protocol | Named agents for this SMP, per-agent budgets, role palette |
| E Existing AI Mod | Refactored provider-agnostic mod + sidecar as the framework's reference impl | This SMP's specific mod configuration |
| F Dev Workflow | Repo templates, CI templates, contributor model | This project's concrete repo layout |
| G Cross-Mod Integration | Integration pattern catalog, shared tag schema, balance knobs | The specific integrations this pack enables |
| H Sandbox & Tests | Sandbox topology templates, harness, rubric library, example test plans | The actual test plans for this SMP |
| I Product Packaging | Distribution model, docs site, starter CLI, licensing | N/A — the reference pack is a _consumer_ of product packaging, not a producer |
| J Web App & Discord Control Plane | Web app (pack authoring, agent management, ops, tests); Discord bot (per-env channels, chat bridge, console relay, map, agent control); shared auth/ACL model | This SMP's deployed web app instance + Discord server layout |
| K Legal, IP & Licensing | License matrix templates, per-mod legal-status schema, attribution templates, takedown policy | Final per-mod sheet for the reference pack, product name/trademark review |
| L Security & Privacy | Threat model template, secrets/rotation runbook, prompt-injection policy, audit-log schema, consumer security checklist | This SMP's secrets inventory and ACL mapping |
| M Player (non-author) Experience | First-login UX patterns, agent-discovery design, etiquette / code of conduct for agents, accessibility checklist | This SMP's intro flow and agent etiquette |
| N Performance Budgets & Profiling | SLO templates, profiling workflow, benchmark suite, regression-gate rules | This SMP's concrete SLOs and benchmark numbers |
| O World & Pack Lifecycle | Update-runbook template, world-versioning spec, migration tooling, archive strategy | This SMP's update cadence and archive plan |

## Vision (captured from interview — 2026-04-18)

> The section below describes the **reference pack** (user's SMP). Generic product requirements live in **Product Framing** above.


- **Scale & theme**: ≤500 mods, ATM-style breadth, central theme is **Create** + **Create: Aeronautics**. **Multiple tech mod tiers** coexist (e.g., Powah, Mekanism, and others) — not Create-addon-only.
- **Curation philosophy — variety over depth**: prefer many _distinct_ mods each pulling their weight over stacking lots of addons onto a single big tech or magic tree. If two mods solve the same niche, only the better one survives the shortlist unless they offer meaningfully different play.
- **Difficulty layer**: explicit difficulty-modifier mods progressively raise challenge through the campaign (e.g., scaling-health style, regional-difficulty ramps, boss/raid intensifiers). Difficulty ramp is tuned against the questbook beats.
- **Custom content**:
  - Heavy **KubeJS** custom recipes.
  - Heavy custom textures.
  - Large questbook with milestones tied to custom recipes, custom textures, and curated goals.
- **Mod curation emphasis**: performance-vetted mods, QoL mods, decorative blocks.
- **Deployment**: self-hosted **10-player SMP** with friends, generous RAM budget so clients can run **Distant Horizons + shaders** comfortably.
- **Client/server pack split**: separate client and server pack variants; server strips client-only mods.
- **AI participation**:
  - **Multiple AI bot-player instances on one SMP server simultaneously.**
  - Additionally, helper/overseer AI presences on the same server.
  - Both roles coexist (not a toggle) — possibly several bots, some as peer players, some as helpers.
  - **Multi-model flexible**: design so Claude, Gemini, and other providers are swappable.
- **Existing asset**: this repo's Gemini AI Companion mod (Fabric 1.21.1, MCP bridge, build planner, highlights, undo, vision, voice, permissions) is a candidate to evolve into the helper layer and/or be refactored provider-agnostic. Inclusion is "likely if it satisfies the agent-playing-MC-with-the-group requirement."

---

## Planning Methodology — Two-Layer Interview Process

Planning proceeds as two stacked interview layers. No workstream exits planning until both layers have been run for it.

**Layer 1 — Alignment Interview (cross-cutting).**
- Purpose: pull the whole vision out of the user's head, identify every area, set priorities and sequencing, and define the boundary between areas.
- Scope: spans all workstreams simultaneously.
- Output: this plan file's **Context**, **Vision**, **Open Foundational Decisions**, **Workstream charters**, and **Sequencing**.
- Status: in progress in this channel. Additional Layer-1 passes may be needed when scope shifts.

**Layer 2 — Per-Area Deep Interview.**
- Purpose: for a single workstream, extract every requirement, constraint, preference, reference, edge case, and acceptance criterion the user holds.
- Scope: one workstream per dedicated session. Strictly no cross-talk into other workstreams during a Layer 2 session unless a true dependency surfaces.
- Output: a **requirements document** for that workstream (linked from this file), and a follow-up **implementation plan file** in `/root/.claude/plans/`.
- Gate: no implementation starts in a workstream until its Layer 2 interview + requirements doc are signed off.
- Seed agendas for each Layer 2 interview are listed in **Layer 2 Interview Agendas** below.

## Open Foundational Decisions (research must resolve)

These are the decisions that gate everything else. Each is owned by one of the workstreams below.

1. **Loader + MC version** (Workstream A). NeoForge 1.21.1 vs NeoForge/Forge 1.20.1 vs other. Must be compatible with Create, Create: Aeronautics, KubeJS, Distant Horizons, modern shaders, and a viable AI-bot-client stack.
2. **Bot-player technical approach** (Workstream D). Headless modded clients vs server-side fake-player mod vs hybrid. Must survive a ~500-mod pack.
3. **Quest framework** (Workstream B). FTB Quests vs Heracles vs other.
4. **Pack distribution model** (Workstream A). CurseForge pack, Modrinth pack, Prism/MultiMC self-hosted, or custom launcher.
5. **Fate of the Gemini AI mod** (Workstream E). Refactor in place to multi-provider, fork + rename, or greenfield replacement.
6. **Multi-agent coordination model** (Workstream D). Shared world state, turn-taking, collision avoidance, inter-agent chat.
7. **Agent count + cost budget** (Workstream D). Concurrent bot agents; monthly token/hosting ceiling.
8. **Hosting hardware target** (Workstream C). CPU/RAM/storage/network sized for 10 humans + N bot clients + shader-heavy client-side rendering off-box.
9. **Control-plane deployment model** (Workstream J). Self-hosted-per-pack (default for the user's SMP) vs hosted SaaS vs hybrid; where the web app and Discord bot run relative to the Minecraft server.
10. **Auth & ACL model** (Workstream J). Discord OAuth, local accounts, license keys, per-environment roles, who can send commands to which server.
11. **Agent-embodiment mix** (Workstream D). Which combination of true bot-player clients, Minecolonies citizens, Recruits followers, smarter-NPC entities, and disembodied helper agents the reference pack uses, and the per-role assignment.
12. **Curation discipline & per-category mod caps** (Workstream A). Concrete rules for the "variety over depth" heuristic; how overlapping mods are adjudicated; which difficulty-modifier mods join the shortlist.

---

## Research Workstreams

Each workstream is a long-running effort picked up in dedicated future sessions. Each produces decision docs, which in turn spawn implementation plan files.

### Workstream A — Modpack Foundation
**Goals**
- Pick loader + MC version using an ecosystem compatibility matrix (Create, Aeronautics, KubeJS, DH, Oculus/Iris, performance mods, QoL mods, multi-tech mods like Powah/Mekanism/etc., difficulty mods, and the bot-client stack from D).
- Build a categorized mod shortlist using the **variety-over-depth** heuristic: tech tiers (Create + addons, Powah, Mekanism, …), worldgen, adventure/RPG, magic, difficulty modifiers, performance, QoL, decoration, aesthetic, utility. Enforce a "one winner per niche unless meaningfully different" rule.
- Design the client-only vs server-required split rules.
- Choose packaging/distribution toolchain.
- **Central risk**: holding ATM-scale variety to a performance-survivable shortlist. A owns the curation-discipline loop with N (performance) as the enforcement mechanism.

**Deliverables**
- Version/loader decision doc.
- Categorized mod list (living spreadsheet or markdown).
- Client vs server mod manifest.
- Distribution model decision doc.

**Future critical files**: `pack/`, `manifest.json` (or Modrinth `modrinth.index.json`), per-side mod manifests.

### Workstream B — Content Layer (recipes, textures, quests)
**Goals**
- Decide scripting stack (KubeJS only, or KubeJS + CraftTweaker where each excels).
- Design recipe authoring pipeline, file layout, and naming conventions.
- Design custom-texture authoring pipeline (resource pack structure, CTM, CIT, atlas rules, tooling).
- **AI-driven texture generation**: define the prompt library, style guide, and repeatable generation loop for the pack's "3D-looking" custom images. Outputs are not the concern of this plan — the **prompts, constraints, iteration loop, and evaluation criteria** are.
- Pick quest framework; outline questbook chapters tied to Create and Aeronautics progression, with milestone hooks for custom recipes/textures.

**Deliverables**
- Recipe authoring guide + directory layout.
- Texture pipeline doc (tools, naming, atlas constraints, auto-generation where possible).
- **Texture prompt library**: versioned prompts with style anchors, negative prompts, reference imagery rules, palette/dimension constraints, and acceptance criteria. Repeatable by any operator (human or agent).
- **Texture iteration loop**: the exact Think → Generate → Evaluate → Refine cycle and the tools used at each step.
- Questbook outline: chapters, milestones, dependency graph.

**Future critical files**: `kubejs/`, `resourcepacks/`, `prompts/textures/`, `config/ftbquests/` (or Heracles equivalent).

### Workstream C — Hosting & Operations
**Goals**
- Spec hardware for a self-hosted server running ~500 mods + 10 humans + N bot client processes (if headless bots are chosen).
- Decide process topology (single server JVM, multiple JVMs, bot clients in containers vs on separate machines).
- Backup, crash recovery, monitoring, world versioning.
- Networking: port plan, firewall, remote admin access, latency budget.

**Deliverables**
- Hardware bill-of-materials + budget.
- Server startup/systemd/Docker layout.
- Ops runbook (start/stop/backup/restore/upgrade).

### Workstream D — AI Agent Architecture
**Goals**
- Evaluate bot-player technical approaches against the chosen loader/version:
  - Headless modded client (Prism+automation, forked Minecraft client, NPC-driver mod).
  - Server-side fake-player mods (Carpet-style, Create-aware alternatives).
  - Hybrid (server-side stub + remote brain).
- Design a **provider-agnostic abstraction** anchored on MCP, since this repo already ships an MCP bridge. Claude Agent SDK, Gemini, others plug in as MCP clients or via a thin model router.
- Design per-agent memory and shared world memory (what each bot knows, what is common knowledge, how state is persisted across sessions).
- Define safety guardrails: permissions, rate limits, cost ceilings, undo/rollback policy, griefing prevention, human-override flow.
- Plan multi-agent coordination: does each bot have its own context, or is there a "director" layer? How do they avoid stepping on each other (shared locks on regions, task queue, chat protocol)?

**Agent-in-world realism goals (new, cross-cuts G):**
- Persistent in-world identity: stable username / UUID / skin per agent role, shown in player lists and scoreboards; graceful reconnect/handoff if an agent restarts.
- Voice parity: agents can speak (TTS) and listen (STT) on par with humans where policy allows; channel isolation between in-game voice and agent-to-agent audio.
- **Create / Aeronautics reasoning**: mental models for kinetic stress, SU budgets, contraption assembly, fluid networks, train signals, aeronautical lift/thrust. Agents consult a Create-state MCP tool before modifying or constructing contraptions.
- Redstone mental model: recognize and reason about common redstone patterns before editing them.
- Anticheat/grief-protection compatibility: detect which protection mod is present and either operate within its rules (claim tokens, whitelisting) or refuse the action with a clear reason.

**Alternative / hybrid agent embodiments — "NPC-as-body, Claude-as-brain":**
User explicitly wants one or more of:
- **Recruits mod** — recruitable follower NPCs as guards/squadmates driven by Claude.
- **Minecolonies** — colony citizens as long-running role-bound agents (builders, farmers, miners, guards) driven by Claude at a task/plan level instead of per-tick.
- **Smarter-NPC mods generically** — any mod that adds navmesh-capable, scriptable NPCs (e.g., Custom NPCs, Citizens-analogues on Forge/NeoForge) used as the physical body, with Claude providing goals and dialogue.

Design implication: **the bot-player approach is explicitly allowed to be a mix**. A single SMP can run:
- true bot-player clients for full-range peer players,
- Minecolonies citizens for long-horizon role work,
- Recruits followers for scoped combat/escort companions,
- and server-side helper/overseer agents with no body at all.

Workstream D must produce a **capability matrix** comparing each embodiment on: mobility/verbs, mod coupling, TPS cost per agent, realism to humans, anticheat compat, skill-library surface, and suitability per role in the agent palette. The decision doc picks the subset the reference pack uses and documents the rules for combining them.

**Deliverables**
- Bot-player approach decision doc (covering headless, fake-player, recruit/colony, and hybrid paths).
- Provider abstraction interface spec.
- Agent memory schema.
- Safety & cost policy doc.
- Multi-agent coordination protocol.
- Agent-in-world realism spec (identity, voice, Create reasoning, redstone, anticheat compat).
- **Skills & Tools Library** — reusable, versioned capabilities agents (and humans) invoke for repeatable tasks. Organized into two tiers:
  - **Claude Skills** (prompt-packaged playbooks): e.g., `build-starter-base`, `scout-biome`, `stock-kitchen`, `balance-recipe`, `author-quest-chapter`, `generate-texture-set`, `debug-recipe-conflict`.
  - **MCP Tools** (code-backed, deterministic): reuse and extend the existing `minecraft_*` tool surface (buildsite, execute_build_plan, recipe_lookup, etc.) with new tools demanded by the modpack (Create-network inspection, KubeJS recipe validation, quest progress query, fake-player control, resource-pack diffing).
  - Each entry has: name, purpose, inputs, outputs, preconditions, side-effects, cost class, failure modes, and the exact prompt(s) that drive it.

### Workstream E — Existing AI Mod Audit & Evolution
**Goals**
- Map current Gemini AI Companion subsystems: MCP bridge, structured build planner, highlights, undo, vision, voice, permissions, setup wizard, RegistryHints, batch/timed command path.
- Classify each subsystem as: **reuse-as-is**, **refactor-to-provider-agnostic**, or **retire**.
- Plan the fork/rename path if the mod is rebranded off of "Gemini AI Companion."
- Identify coupling points with the loader/version decision (this mod is Fabric 1.21.1 — may force the pack's loader choice, or force a port).

**Deliverables**
- Subsystem audit doc.
- Refactor plan with touchpoints, migration order, and risk notes.
- Port/fork/rename recommendation.

**Current critical files**: `src/` (main mod), `mcp-sidecar/` (Java MCP sidecar), `run-mcp-sidecar-node.js` (Node MCP sidecar), `debug-mcp-client.py`, `build.gradle`, `README.md` (feature catalogue).

### Workstream G — Cross-Mod Integration & Compat
**Goals**
- Catalogue every **integration boundary** between mods that requires custom code/config: shared item tags, unified ore dictionaries, recipe bridges (e.g., Create mechanical crafting vs. Ars Nouveau enchanting vs. Tinkers' smelting), fluid handler compatibility, energy system unification (FE/Forge Energy vs. Create stress/kinetic), inventory/transport bridges, JEI/REI visibility, tooltip harmonization, advancement/quest triggers across mod boundaries.
- Define **per-integration requirement specs**: which mods are coupled, what Minecraft/KubeJS/CraftTweaker mechanism wires them, what tests prove it works, what breaks it.
- Identify recurring **integration patterns** the modpack will rely on repeatedly (e.g., "any ore from any mod must be processable by Create crushing wheels and produce the pack's unified dust tag"). Each pattern becomes a reusable KubeJS helper and/or Claude Skill (Workstream D).
- Balance & progression: map how each mod's progression gates fit into the questbook and into the unified resource economy; remove dead ends and unintended shortcuts.

**Deliverables**
- Integration catalogue — per-pair or per-cluster docs (inputs, outputs, mechanism, validation).
- Shared tag schema (metals, gems, dusts, plates, fluids, machines, weapons, storage) used by KubeJS scripts across all mods.
- Unified energy / fluid / item transport strategy doc.
- Recipe balance pass doc — which mods are rebalanced, by how much, why.
- JEI/REI cleanup doc — hidden items, renamed items, consolidated duplicates.

**Future critical files**: `kubejs/server_scripts/integrations/`, `kubejs/startup_scripts/tags/`, `config/<mod>/` overrides.

### Workstream K — Legal, IP & Licensing
**Goals**
- Map redistribution rules for every mod in the final shortlist (CurseForge/Modrinth API ToS, per-author license, "no rehosting" clauses). Produce a per-mod legal status sheet.
- Decide license posture for product code (likely MIT/Apache-2), for the reference pack's configs/KubeJS/quests (separate, consumer-replaceable), and for AI-generated textures (ownership, provider ToS on commercial use, derivative-work stance).
- Name & trademark audit — "Create" is a trademark; ensure product name, docs, and UI copy do not infringe.
- Attribution format for mods, artists, AI-model providers, and contributors.
- Takedown / DMCA handling plan for user-generated content on any hosted surface.
**Deliverables**
- Per-mod legal status sheet.
- Product license matrix (code / configs / content / textures).
- Attribution and credits template.
- Takedown policy.

### Workstream L — Security & Privacy
**Goals**
- Threat model: adversaries include griefer players, prompt-injecting players (signs/books/chat), compromised MCP sidecars, leaked bridge tokens, malicious mod updates, bad provider responses.
- Secrets management end-to-end: Claude/Gemini API keys, MCP bridge tokens, Discord bot token, RCON passwords, OAuth secrets, webhook signing keys. Rotation policy.
- MCP bridge hardening beyond the current loopback+bearer-token baseline: scoped tokens per client, per-tool rate limits, per-environment allowlists.
- Prompt-injection policy: what counts as untrusted input (player chat, signs, books, item names, world-read data), quarantine rules, refusal patterns. Extends H's chaos tier into a lived policy.
- LLM data-exfil stance: what fields are ever sent to an LLM (chat, inventory, world snippets, player names, coordinates), redaction rules, per-player opt-in/out, retention of logs inside the gateway.
- Privacy-by-default settings shipped to consumers; explicit consent before enabling richer telemetry.
- Audit log: tamper-evident, append-only, covers every agent action + every human admin command + every config change.
**Deliverables**
- Threat model doc.
- Secrets inventory + rotation runbook.
- Prompt-injection + data-handling policy.
- Audit-log schema and retention policy.
- Security checklist for consumers adopting the product.

### Workstream M — Player (non-author) Experience
**Goals**
- First-login flow: what the SMP looks like to a new player on day one (spawn, guidebook, tutorial quest chain, intro to agents).
- In-game agent discovery: how players find out what agents exist, what each does, how to summon / request / dismiss them, how to report bad agent behavior.
- Player-facing help surface: `/help` topics, in-game manual, linkable docs, on-demand agent "explain yourself" affordance.
- Etiquette rules: how agents announce themselves, respect claim/grief-protection boundaries, stay out of private spaces, handle disagreement between two human players.
- Accessibility basics: colorblind-safe highlight palette, readable fonts, optional chat-TTS.
**Deliverables**
- First-login UX spec.
- Agent-discovery + summoning spec.
- In-game help system design.
- Player etiquette / code of conduct for agents.
- Accessibility checklist.

### Workstream N — Performance Budgets & Profiling
**Goals**
- Set explicit **SLOs**: server TPS floor, chunk-tick budget, client FPS floor with and without shaders + DH, memory ceilings per process, network latency budget bot→server.
- Define a **profiling workflow**: Spark profiler runs, jemalloc/GC tuning, client frame-time captures; when each is triggered (nightly, pre-release, on alert).
- Build a **benchmark suite** that runs in the sandbox: canonical test worlds, simulated player/bot workload, reproducible measurements.
- Guardrails: a regression gate that blocks promoting a mod-list change, KubeJS change, or agent-framework change if it breaches SLOs.
- Cross-cuts A (mod choice affects TPS), C (hardware sets ceilings), G (KubeJS/integration complexity costs TPS), D (agents consume CPU via bulk ops and contraption assembly), J (web/Discord bridges must not starve the server thread).
**Deliverables**
- SLO doc with thresholds and rationale.
- Profiling workflow doc.
- Benchmark suite spec.
- Regression-gate rules for CI and sandbox promotion.

### Workstream O — World & Pack Lifecycle
**Goals**
- Safe procedure to update a ~500-mod pack mid-campaign without corrupting saves: dry-run on sandbox, chunk/block-entity compat scan, player-inventory migration, removed-mod cleanup strategy.
- World versioning: tag each save snapshot with pack-version metadata; automate rollback if update fails.
- Migration tooling for removed or renamed blocks/items/entities (KubeJS tag remaps, World-Edit sweeps, inventory rewrites).
- Acceptable downtime and rollback SLAs per environment (dev tolerates more, prod SMP tolerates very little).
- Archive & replay: long-term archive of world + pack-version pairs so a campaign can be opened years later.
- Cross-cuts A (pack releases), C (backup/restore ops), H (sandbox validates each update), J (admins orchestrate updates via web/Discord).
**Deliverables**
- Update-runbook for mid-campaign pack upgrades.
- World-versioning and rollback spec.
- Block/item/entity migration tool inventory.
- Archive/replay strategy.

### Workstream J — Web App & Discord Control Plane
**Goals**
- Design the **web app** as the product's primary UI. First-class surfaces:
  - **Pack authoring**: mod list, client/server split, version targets, launcher export.
  - **Content authoring**: KubeJS recipe editor/linter; texture prompt library with generate/evaluate/diff loop; questbook chapter editor.
  - **Agent management**: spawn/stop/pause bots, assign roles, set per-agent provider + budget, live action stream, cost dashboard, approvals for guarded actions.
  - **Server ops**: console, logs, player list, live or tiled **world map**, performance metrics, restart/backup controls.
  - **Test harness**: run sandbox test plans, view results, drill into rubrics, gate promotions from sandbox → prod.
- Design the **Discord control plane** — chat-first mirror of the web app for day-to-day admin. Supports:
  - **Per-environment channels** (dev / sandbox / prod SMP), or per-env category with sub-channels for `#console`, `#chat-bridge`, `#agents`, `#alerts`, `#map`.
  - **Command surface in channel**: e.g. `/srv cmd <command>` sends directly to the server referenced by the channel; `/agent <name> pause|resume|task ...`; `/map here` posts a map tile.
  - **Chat bridge**: messages in `#chat-bridge` relay to in-game chat and vice versa, with player/bot attribution.
  - **Alerts**: crashes, restarts, backup success/failure, cost spikes, failed test runs, high-risk agent actions awaiting approval.
  - **Agent management from chat**: inspect live agent thoughts/actions, approve/deny guarded actions, tail cost.
- Define the **control-plane architecture**:
  - Web app and Discord bot share one backend ("control-plane gateway") that speaks to the Minecraft server via the agent mod's **MCP bridge** (Workstream E) and to the agent framework (Workstream D).
  - Real-time bus (WebSocket + Redis or NATS) for console, chat, agent events.
  - Event/audit log as the single source of truth for who did what in which environment.
- **Auth / ACL**: Discord OAuth as the default identity for P2 (friend group), with role-mapped ACLs per environment; local accounts / license keys for P1 and P3. Least-privilege by default.
- **Deployment**: self-host-next-to-server by default (docker-compose one-liner). Hosted SaaS is a later option for P1/P3.
- **References to investigate**: Hermes-style agent dashboards and OpenClaude/OpenCode-style workspaces — what to borrow, what to reject.

**Deliverables**
- Web app feature spec, organized per persona (P1 minimal, P2 full, P3 multi-pack).
- Wireframes / IA for the web app (sitemap, major screens).
- Discord bot command + event spec, including slash-command schema and per-channel wiring rules.
- Control-plane architecture doc (services, message buses, data model, event log).
- Auth & ACL model doc.
- Deployment topologies doc (self-host, SaaS, hybrid).
- Observability + audit plan.

**Future critical files**: `web/` (frontend + backend), `discord-bot/`, `control-plane/` or `gateway/`, shared API/MCP schemas under `shared/api/`, deployment assets under `deploy/`.

### Workstream I — Product Packaging, Distribution & Docs
**Goals**
- Decide how the product ships: monorepo vs multi-repo, release cadence, versioning scheme across mod/sidecar/KubeJS-lib/templates/docs.
- Design a **starter experience**: how a new pack author bootstraps (CLI, template repo, script, or manual?), what they get on day one, what decisions the product asks them on day one.
- Plan a **docs site** and a recipe-book of "how to do X": cross-mod integration recipes, texture-prompt recipes, agent-skill recipes, test-plan recipes.
- License and branding: what parts are MIT/Apache-2 reusable by consumers, what parts are example content, how consumers should credit/attribute.
- Telemetry/privacy stance (if any — default off is safe).
- Upgrade path: how consumers pull new product versions without clobbering their pack-specific content.
- Define the contract between the product and the reference pack: where the two repos/trees meet, which consumes which.

**Deliverables**
- Packaging decision doc (mono vs multi-repo, versioning, release flow).
- Starter experience spec (bootstrap path, first-run questionnaire, generated scaffolding).
- Docs site outline (sections, tutorials, reference pages, recipe-book categories).
- License & attribution doc.
- Upgrade/migration strategy doc.

**Future critical files**: `product/` (or separate repo) containing `cli/`, `templates/`, `docs/`, `examples/reference-pack/` pointer.

### Workstream H — Agent Sandbox & Test Plans
**Goals**
- Stand up a dedicated **sandbox Minecraft instance(s)** — mirror of the live SMP pack, isolated from human players, safe to nuke and restore — that agents exercise against before any change touches production.
- Author **well-written test plans** for every agent capability: preconditions, setup script, test steps (human-readable + machine-executable), expected in-world state, expected MCP tool results, expected failure modes, acceptance criteria, cleanup.
- Build a **repeatable test harness**: seed world, spawn agent(s), run test plan, snapshot result, diff against expected, tear down. Same harness used for regression before deploying prompt/skill/tool changes.
- Categorize tests by risk tier:
  - **Tier 1 (smoke)** — cheap, run on every change: agent connects, chats, runs one validated build, undoes it.
  - **Tier 2 (capability)** — one test plan per Skill / MCP Tool: build a cabin, balance a recipe, author a quest stub, scout a biome, diagnose a recipe conflict.
  - **Tier 3 (scenario)** — multi-agent scripted scenarios: two bots collaborate on a Create contraption; helper agent resolves a griefing incident; bot fills a quest objective while human plays.
  - **Tier 4 (chaos / adversarial)** — prompt-injection attempts via sign/chat/book, malformed MCP calls, concurrent-edit races, partial crashes.
- Define **evaluation rubrics** for non-deterministic outputs: what counts as a "good enough" cabin, texture, questbook chapter, or diagnostic report, scored repeatably.
- Define **test-authoring conventions**: naming, directory layout, fixtures, expected-state format (NBT diff, screenshot hash, structured-log match, MCP response schema).

**Deliverables**
- Sandbox topology doc (how many sandbox worlds, how they reset, how they diverge from prod).
- Test harness spec — process, tooling, CI integration.
- Test plan catalogue index — one entry per Skill/Tool/Scenario; each entry links to a standalone test plan file.
- Evaluation rubric library.
- Adversarial/chaos test catalogue.

**Future critical files**: `sandbox/`, `tests/agent/`, `tests/fixtures/worlds/`, `tests/plans/`, `tests/rubrics/`.

### Workstream F — Development Workflow & Repo Layout
**Goals**
- Decide repo strategy: monorepo vs multi-repo (modpack config, AI mod, sidecar, resource pack, KubeJS scripts, quests, ops scripts).
- CI/testing approach: pack integrity checks, recipe/quest linting, agent smoke tests, shader/DH regression tests.
- Large binary strategy (textures, world saves, release jars) — Git LFS vs releases vs external storage.
- Contribution workflow for solo + occasional collaborators.

**Deliverables**
- Repo layout doc.
- CI plan.
- Contribution guide (CONTRIBUTING-level).

---

## Sequencing (dependency order)

1. **A-1 (version/loader)** and **D-1 (bot-player approach)** must land first — they gate nearly everything else. They should be researched together because the bot-client stack narrows the loader options.
2. **E (existing mod audit)** runs in parallel with A-1/D-1; its outputs feed **D-2 (provider abstraction)** and may influence the loader decision.
3. **A-2 (mod shortlist)** unblocks **B (content layer)** and **G (cross-mod integration)** — integration work cannot start until the shortlist is stable.
4. **G (integration catalogue)** and **B (content layer)** run together because recipes and tags are the concrete expression of integration decisions.
5. **D (Skills & Tools library)** accretes continuously — as patterns emerge in B and G, they are promoted into reusable skills/tools.
6. **H (sandbox & test plans)** begins as soon as **A-1** and **D-1** are set; every Skill/Tool promoted in step 5 ships with a test plan in H before it lands in prod. H gates all agent-facing releases.
7. **C (hosting)** can run in parallel once A-1 is decided. The sandbox instance(s) from H share the same hardware planning inputs.
8. **F (workflow)** can be drafted early and finalized once A, D, E, H are settled.
9. **I (product packaging)** runs last in each cycle — it aggregates outputs from A–H into shippable artifacts, starter templates, and docs. But its **starter-experience design** begins early (it shapes what every other workstream must produce as a reusable artifact vs. a one-off).
10. **J (web app + Discord)** begins its **design** immediately after D-1 and E are scoped, because its architecture dictates what the agent framework (D) and MCP bridge (E) must expose (events, control APIs, auth). Its **build** follows D's and E's first stable surfaces. J and H are tightly coupled — the test harness runs through J, and J enforces the sandbox→prod promotion gate H defines.
11. **K (legal)** begins alongside A-2 because the per-mod legal status is an input to the final shortlist. Product-wide licensing can be drafted early and ratified once the shortlist stabilizes.
12. **L (security)** begins alongside D and J; the threat model is required before J's auth/ACL lands and before D exposes any new MCP tools.
13. **M (player UX)** starts once D-1 and the agent palette are sketched; it cannot be designed without knowing which agent embodiments exist.
14. **N (performance)** runs continuously; SLOs are drafted once A-1 is chosen and tightened once the mod shortlist reaches a first cut. Every subsequent workstream must respect the regression gate.
15. **O (lifecycle)** drafts its update-runbook once C is settled, but mid-campaign update drills start as soon as the sandbox (H) exists.

---

## How Future Sessions Should Use This Document

1. Confirm whether the session is a **Layer 1** refinement or a **Layer 2** deep-dive. Layer 2 sessions pick exactly one workstream.
2. For a Layer 2 session, open the relevant entry in **Layer 2 Interview Agendas** and run the interview. Append user answers inline or into a linked requirements doc.
3. After the interview, run the supporting research (web research on ecosystem compatibility, codebase audits, experiments, etc.).
4. Record findings and decisions under that workstream's section in this file, or spawn a companion decision/requirements doc and link it.
5. Spawn a new plan file in `/root/.claude/plans/` for any implementation work that the requirements unlock.
6. Log any vision updates in the **Living Vision Log** so later sessions can trace how scope evolved.

---

## Layer 2 Interview Agendas (seed questions per workstream)

Each agenda below is the **starting list** for that workstream's Layer 2 deep-dive session. Expect 2–4x as many questions to emerge during the interview itself. Answers get written to the workstream's section above or to a linked requirements doc.

### A — Modpack Foundation
- What evidence would satisfy you that a loader/version choice is correct (compat matrix depth, specific mod presence, performance benchmarks)?
- Preferred launcher/distribution (CurseForge, Modrinth, Prism, ATLauncher, custom)?
- Hard "must-have" mods and hard "never" mods?
- Per-category mod budget (tech, magic, adventure, deco, QoL, perf) and how aggressively to trim?
- Release cadence for pack updates vs. SMP uptime constraints?
- Licensing stance on redistributing configs, KubeJS scripts, textures?

### B — Content Layer (recipes, textures, quests)
- KubeJS preferred over CraftTweaker, or a split? Why?
- Texture style anchors — reference images, palette, lighting direction, "3D-looking" definition, banned aesthetics?
- Texture generation tooling preference (local SD, hosted model, Claude image API, other) and cost ceiling?
- Questbook framework preference (FTB Quests, Heracles) and why?
- Questbook backbone — what are the big chapter beats, endgame goal, optional branches?
- Localization requirements (English only, others)?
- How closely should custom recipes bend vanilla/Create balance vs. preserve it?

### C — Hosting & Operations
- Physical host location, OS, current specs, upgrade budget?
- ISP bandwidth, static IP or dynamic, VPN/tunnel preference?
- Backup target (local NAS, cloud, both), retention window, restore-time objective?
- Remote admin model (SSH, web panel, in-game ops, Discord bot)?
- Uptime expectation and acceptable maintenance window?
- Player geo distribution for latency planning?

### D — AI Agent Architecture
- How many concurrent bot agents max, steady state vs. peak?
- Monthly cost ceiling across providers; per-agent budget?
- Role palette — list the named roles (e.g., "Builder-Bot," "Scout-Bot," "Quartermaster," "Overseer") and each role's remit?
- Allowed-command surface per role; hard-denied commands?
- Sensitive zones (player homes, spawn, staged builds) — how opted in/out?
- Memory horizon — how far back does an agent remember, and what is forgotten on purpose?
- Logging/audit requirements — what must be recorded, retained how long, who can read?
- Human override model — how does a human stop/redirect an agent mid-task?

### E — Existing AI Mod Evolution
- Keep the Gemini brand, rename, or fork into a new project?
- Which subsystems are must-preserve (MCP bridge, build planner, highlights, undo, voice, vision, permissions) and which are optional?
- Is porting to NeoForge/Forge acceptable if A-1 lands there, or is the current Fabric 1.21.1 codebase a floor?
- Source license posture post-fork (stay MIT, relicense, dual)?
- Keep Gemini as a runtime-selectable provider, or strip?

### F — Development Workflow & Repo Layout
- Mono-repo or multi-repo; who touches which?
- Collaborators beyond the user; access model?
- CI budget and host (GitHub Actions free tier, self-hosted runner)?
- Secret management (API keys, bridge tokens, server RCON passwords)?
- Release/tag strategy for pack versions, mod versions, sidecar versions?

### G — Cross-Mod Integration & Compat
- Priority mod clusters to integrate first (Create + addons, tech tier, magic tier, adventure tier)?
- Balance philosophy — tight/minimal-duplication or generous/kitchen-sink?
- Unified tag schema preferences — follow Common tags, invent custom, or layer both?
- Banned items/mechanics (creative flight, teleports, infinite power, void miners)?
- Progression pacing — hours to first Create setup, to Aeronautics, to endgame?
- How aggressively should JEI/REI hide duplicates vs. surface them?

### K — Legal, IP & Licensing
- For every mod on the shortlist: do we have explicit permission to redistribute in a pack? CurseForge/Modrinth pack rules sufficient, or per-author reach-out needed?
- Product code license preference (MIT, Apache-2, BSL, proprietary-then-open)?
- Stance on AI-generated textures: who owns them, are we OK with provider-ToS constraints on commercial use, do we ship provenance metadata?
- Product naming: avoid "Create" in the name to sidestep trademark? Working title "Warm Iverson" OK?
- Attribution surface — where credits appear (pack loading screen, docs site, in-game `/credits`)?
- Takedown process if a mod author objects post-launch?

### L — Security & Privacy
- Who is in scope as an adversary (griefer player, prompt-injecting player, leaked provider key, compromised MCP sidecar)?
- How are secrets stored (env files, secret manager, OS keyring) and rotated?
- What player/world data is allowed to reach an LLM, redacted how, and for whose agents?
- Opt-in vs opt-out for players whose chat/voice/actions might be sampled by agents?
- Audit-log retention window, access control, tamper protection?
- Consumer default posture: privacy-tight out of the box?

### M — Player (non-author) Experience
- What should the very first minute of a new player's session feel like?
- Where do players first learn that AI agents exist on this server?
- How does a player summon, task, pause, or dismiss an agent without the web app or Discord?
- Rules of engagement — can agents enter private bases, can players "hire" one for a task, can agents refuse?
- Accessibility priorities (colorblind, motion, text size, TTS preference)?
- How are abusive or annoying agent behaviors reported by players?

### N — Performance Budgets & Profiling
- Minimum acceptable server TPS with full player + bot load; hard floor before we alert?
- Client FPS target with shaders + DH at player-preferred settings?
- Memory budget per process (server JVM, each bot client, gateway, bot sidecar)?
- Who owns the profiling runs and how often?
- Which benchmarks matter most (worldgen, bulk fills, Create contraption tick cost, DH streaming)?
- CI regression gate threshold — how big a regression blocks a merge?

### O — World & Pack Lifecycle
- Expected update cadence for the pack (weekly, biweekly, ad hoc)?
- Tolerance for downtime during upgrades on prod SMP vs dev vs sandbox?
- Must-preserve world state across updates (builds, inventories, advancements, quest progress, Create contraptions)?
- Rollback window — how fast must we roll back if an update breaks the world?
- Archive horizon — do we want to be able to re-open a two-year-old campaign?
- Migration tooling preference (KubeJS remaps, WorldEdit batches, a dedicated migration agent skill)?

### J — Web App & Discord Control Plane
- What specific Hermes / OpenClaude / OpenCode features are you drawing inspiration from? Any hard don'ts?
- Is the web app the primary interface for content authoring (recipes, textures, quests), or does it just observe/orchestrate while authoring stays in files?
- World map: live (tile server, e.g. BlueMap/squaremap) or snapshot posts on demand? Both?
- Discord: one guild per environment, or one guild with per-env channels/categories? Do P1/P3 need non-Discord chat options (Matrix, Slack)?
- Who is allowed to send console commands via Discord, and is there an approval flow for destructive ones?
- Chat-bridge identity: should in-game messages sourced from Discord show the Discord user's name, a prefix, or an in-game proxy player?
- Auth — Discord OAuth as the sole identity, or layer with local accounts / API tokens for headless automation?
- Default deployment — docker-compose next to the Minecraft server, k8s, bare binaries? Hosted SaaS ever on the table?
- Which agent-framework events (from D) and MCP surfaces (from E) must J have from day one, and which can come later?
- Observability scope — logs only, metrics, traces? Retention?

### A-additional (variety / difficulty)
- How do we decide between two tech mods that overlap (e.g., Mekanism vs. Powah vs. Ad Astra for space)? Objective criteria?
- Which difficulty-modifier mods are in scope (scaling-health, regional difficulty, raid/boss intensifiers, hunger/thirst, temperature)?
- How tightly does difficulty ramp align with questbook chapters (Workstream B)?
- Which "lots of overlapping addons for one mod" traps are we most at risk of and willing to reject?
- What's the quantitative cap on mods per category (tech, magic, deco, QoL, perf, worldgen, adventure, difficulty)?

### I — Product Packaging, Distribution & Docs
- Mono-repo product + reference pack in one tree, or split into `warm-iverson/` (product) and `reference-pack/` (user's SMP)?
- Which pack-building step should the starter experience own — mod-list choice, quest authoring, integration wiring, agent setup, all of the above?
- Should the starter be a CLI, a template repo (`git clone`), a cookiecutter-style scaffolder, or a web wizard?
- What is the minimum viable v0.1 of the product you'd be proud to put in front of another pack author?
- Licensing — MIT for product code, separate treatment for example content and AI-generated textures?
- Docs platform preference (Docusaurus, mdBook, MkDocs, plain markdown in-repo)?
- Who is the first external user you'd want to onboard, and what do they need from day one?

### H — Agent Sandbox & Test Plans
- How many sandbox instances concurrently; do they share hardware with the SMP host?
- World-reset cadence (every test, nightly, on demand)?
- Acceptable flake rate per tier; retry policy?
- Evaluation-model / LLM-as-judge budget, or strictly deterministic checks?
- Adversarial scope — prompt injection via signs/books/chat, malformed MCP payloads, concurrent-edit races — how paranoid?
- Who signs off on promoting a Skill/Tool from sandbox to prod?

## Living Vision Log

Append-only record of vision updates from the user.

- **2026-04-18 — Session 1, curation + thesis update:**
  - Reference pack spans **multiple tech tiers** (Powah, Mekanism, etc.) alongside Create/Aeronautics, with a **variety-over-depth** curation philosophy ("one winner per niche unless meaningfully different").
  - **Difficulty-modifier mods** are a first-class content pillar; the difficulty ramp is tuned against questbook beats.
  - **Central product thesis locked in**: ATM proves ATM-scale-with-variety is achievable; Warm Iverson's job is to make it **repeatable by small teams without ATM-level staffing**, leveraging methodology + tooling + AI co-workforce. User's enterprise-architecture background is the backing assumption that this scope is shippable.
  - New **Open Foundational Decision #12**: curation discipline + per-category mod caps.
- **2026-04-18 — Session 1, pre-interview gap fill:**
  - Added Workstreams **K (Legal, IP & Licensing)**, **L (Security & Privacy)**, **M (Player non-author Experience)**, **N (Performance Budgets & Profiling)**, **O (World & Pack Lifecycle)**.
  - Expanded Workstream D with **agent-in-world realism** (identity/skin, voice parity, Create/Aeronautics reasoning, redstone models, anticheat compat) and an **alternative/hybrid embodiment path** using Recruits mod, Minecolonies, and/or smarter-NPC mods driven by Claude. Hybrid combinations across true bot-players, colony citizens, follower recruits, and disembodied helpers are explicitly allowed.
  - Added **Open Foundational Decision #11**: agent-embodiment mix.
  - Not folded in this pass (may revisit): anticheat/grief-protection as its own workstream (folded into D/G realism goals instead); product business model + success metrics.
- **2026-04-18 — Session 1, control-plane addition:**
  - Product now includes a custom **web app** as the primary interface (pack authoring, agent management, ops, test harness, world map), inspired by Hermes-style agent dashboards and OpenClaude/OpenCode-style workspaces.
  - Companion **Discord control plane**: per-environment channels (dev / sandbox / prod SMP), direct server console via channel-scoped commands, bidirectional chat bridge, world-map posts, and live AI-player management from chat.
  - New **Workstream J — Web App & Discord Control Plane** added. Two new Open Foundational Decisions: deployment model (self-host vs SaaS vs hybrid) and auth/ACL model (Discord OAuth, local, license). J's architecture dictates what D and E must expose.
- **2026-04-18 — Session 1, product reframe:**
  - Scope is no longer "plan the user's modpack." It is "build a **product** — methodology + toolkit + agent framework + sandbox — that any ambitious modpack author can use to build their own large pack." The user's Create + Aeronautics 10-player SMP becomes the **reference implementation / dogfood case**.
  - Three personas in scope: solo hobby author, friend-group owner (user), curated publisher.
  - New **Workstream I — Product Packaging, Distribution & Docs** added. Every other workstream now produces two tracks: a **product-layer** generic artifact and a **reference-pack** concrete instance. Matrix captured in _Product Layer vs Reference Pack Layer_.
- **2026-04-18 — Session 1, methodology update:**
  - Planning itself proceeds as a **two-layer interview process**: Layer 1 aligns all areas cross-cutting (this session and future refinement passes); Layer 2 runs a massive per-area interview for each workstream before any implementation begins. Layer 2 seed agendas are captured above and grow during each deep-dive.
- **2026-04-18 — Session 1, updates after initial capture:**
  - User brings first-hand experience as self-hoster, mod author, product owner, stakeholder, tester, and player. Plan should go deep on requirements, not stay strategic-only.
  - Cross-mod integrations (recipes, shared tags, compat) are a first-class workstream (Workstream G).
  - AI-driven "3D-looking" custom texture generation is in scope **as prompts, style guides, iteration loops, and evaluation criteria** — the outputs themselves are not the plan's concern (Workstream B).
  - Reusable **Skills/Tools library** is in scope for "repeatable needs" (Workstream D deliverable).
  - A dedicated **agent sandbox + well-written test plans** workstream is required so agents are exercised safely before reaching prod SMP (Workstream H).
- **2026-04-18 — Session 1 initial capture:**
  - Create + Create: Aeronautics–centered, ≤500 mods, ATM-breadth feel.
  - Heavy KubeJS recipes and custom textures.
  - Performance + QoL mods + decorative blocks emphasis.
  - Large questbook tied to custom content.
  - Self-hosted 10-player SMP; client RAM generous for DH + shaders; client/server pack split.
  - Multiple AI bot-player instances + helper/overseer agents on one SMP server, not switchable roles.
  - Multi-model flexible AI provider layer (Claude, Gemini, others).
  - Existing Gemini AI mod in this repo is a likely component if it satisfies the "agent playing MC with the group" requirement.
  - Loader/version, bot-player approach, hardware spec, agent count, and distribution model all flagged as TBD pending research.

---

## Not In Scope For This Plan File

- Line-level implementation steps (those belong in per-workstream follow-up plan files).
- Final mod list (tracked as a Workstream A deliverable, linked from here).
- Final quest text (tracked as a Workstream B deliverable, linked from here).
- Generated texture binaries (outputs live outside; only prompts and evaluation rubrics live here).

Everything else — requirements, integration specs, prompt libraries, skill/tool catalogs, test plans, rubrics — is in scope and expected to accumulate in this file or in companion docs it links.
