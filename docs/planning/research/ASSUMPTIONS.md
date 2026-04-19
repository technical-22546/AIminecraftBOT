# Assumptions Used in the 2026-04-18 Layer-2 Research Sweep

Every research report under this folder was shaped by assumptions. Some were
facts the user stated, some were decisions logged earlier in planning, some
were added by the parent agent when briefing each subagent, and some were
inferred by the subagents themselves during web research. This file enumerates
them so any later reader can spot which conclusions would change if an
assumption is wrong.

Dated 2026-04-18. Update this file if any assumption is overturned by a
later session.

---

## A. User-stated facts (anchor assumptions)

These were asserted by the user during Layer-1 interview and treated as ground truth.

1. **Pack scale & theme**: ≤500 mods, ATM-style breadth, **Create + Create: Aeronautics** central theme, plus multi-tier tech (Powah, Mekanism, others) and difficulty-modifier mods.
2. **Curation philosophy**: variety over depth — "one winner per niche unless meaningfully different."
3. **Deployment**: **self-hosted 10-player SMP** with friends; client RAM generous enough for Distant Horizons + shaders; client/server pack split.
4. **Existing asset**: the current repo ships a **Gemini AI Companion mod on Fabric 1.21.1** (MCP bridge, build planner, highlights, undo, vision, voice, permissions).
5. **Multi-agent intent**: multiple AI bot-player instances coexist with helper/overseer presences on one SMP simultaneously; **not** a switchable role.
6. **Multi-model flexible**: provider layer must stay swappable across Claude, Gemini, others.
7. **Inference hardware**: **NVIDIA DGX Spark** (128 GB unified GB10) dedicated to Warm Iverson agents. Local inference preferred; cloud APIs as fallback.
8. **Ops hardware**: **multiple Proxmox hosts** (user's existing homelab). **LXC containers** for agent runtimes. **Pterodactyl** for the SMP. DGX Spark is a separate node.
9. **Team**: **solo — the user only**.
10. **Timeline**: Release target **2–4 months** from 2026-04-18; no hard date.
11. **Friends**: all local (same city); **half are family in the same physical home as the server**. Latency is effectively LAN. No remote-region concerns.
12. **Voice platform**: group has used **both Discord and Simple Voice Chat**; Release agent voice must handle both.
13. **Pack narrative**: none. **Quests are purely mechanical gameplay milestones** — tier gates, not storyline.
14. **Player-count headroom**: none. Design strictly for 10, not 20+.
15. **Monetization**: unmonetized through v0 / Release / Day+1. Ad-supported web app is a "~day+100" possibility only.
16. **Commercial-safety filter**: **disallowed** as a design/research gate. Features beat commercial-readiness. ARR licenses, commercial-use restrictions on provider APIs, and mod redistribution terms are not blockers at this scope.
17. **Heavy custom content**: heavy KubeJS recipes, heavy custom textures, large questbook with milestones tied to custom content.
18. **Success gate**: author gut call, anchored on three non-negotiables — (1) recipes all work, (2) looks good, (3) runs decent for play.
19. **No monetization-driven scope.** See #16.

## B. Layer-1 derivations (decisions that became assumptions for research)

Decisions made during Layer-1 alignment that the research agents then inherited.

1. **Two-layer interview methodology** — research is advisory, the user ratifies per workstream.
2. **v0 / Release / Day+1 ladder**:
   - v0 = testing/beta, chat-only acceptable
   - Release = friends go-live, TTS+STT voice required
   - Day+1 = two-world concurrent instances with friends, plus launcher, plus custom-datapack terrain
3. **Workstream A–O structure** was fixed; the six sweep topics map to A, B×2, C, D, J.
4. **Repo strategy Phase 1** = combined monorepo; split deferred until architecture approved.
5. **Shared events / cross-service contracts** use the existing MCP bridge + Gemini-mod event surface, not a new shared-contract layer.
6. **Agent embodiment is hybrid-allowed**: peer bot-players + helpers + Minecolonies + Recruits + smarter-NPC mods can coexist.
7. **Sandbox + test-plan harness (Workstream H)** exists as a concept — but agents were **not asked** to research its internals; that's a later Layer-2 topic.
8. **Legal/licensing scope** narrowed to personal-use + friend-group compliance — commercial redistribution is not in scope.

## C. Context baked into each agent's prompt by the parent

What I told each subagent before it started searching. If any of these was wrong, the recommendation is suspect.

### Shared across all six prompts
- Today's date is 2026-04-18.
- Project name = "Warm Iverson."
- Solo dev, Release target 2–4 months.
- ~500 mod pack, Create-themed, self-hosted 10-player SMP.
- Self-hosted inference on DGX Spark; hosted Claude/Gemini as fallback.

### 01 — Loader / MC version
- Candidates specified: **NeoForge 1.21.x, NeoForge/Forge 1.20.1**, existing Fabric 1.21.1.
- Mandatory compat list specified: Create, Create: Aeronautics, KubeJS, DH, Oculus/Iris, Powah, Mekanism, FTB Quests, Heracles, Recruits, Minecolonies, difficulty-modifier mods.
- Bot-client viability asked about explicitly.

### 02 — AI-agent landscape
- Must cover: Mindcraft, Voyager, Altera, Mineflayer + plugins, MineDojo, Baritone, Claude/MCP-specific MC agents, NPC-body mods (Recruits, Minecolonies, Guard Villagers, CustomNPCs).
- Target: peer bot + helper + NPC-body hybrid.
- Referenced existing Fabric 1.21.1 Gemini AI Companion codebase.

### 03 — Quest framework
- Compare FTB Quests, Heracles, BetterQuesting, Questify, any newer entrants.
- Loader "likely NeoForge 1.21.x or 1.20.1 (TBD by another sweep)."
- Purely mechanical quests — no narrative.
- Eventual web-UI integration for authoring.

### 04 — Texture generation
- Hardware: DGX Spark 128 GB.
- Preference: $0/mo API spend, local preferred.
- Output style: "3D-looking" textures; 16×16 or 32×32 baseline; CTM/CIT compatibility.
- Pipeline must be **repeatable** — prompt library + generate/evaluate/refine/assign loop.

### 05 — Self-healing ops
- Specific remediation categories enumerated in prompt: crash-loop, TPS-drop, config drift, disk guardrails, agent-runtime failures, diagnostic-agent escalation.
- Infra constraint specified: Proxmox + Pterodactyl + LXC + DGX Spark separate.

### 06 — Control plane
- References named: Hermes-style agent dashboards, OpenClaude / OpenCode workspaces.
- Feature list enumerated: pack creator, content authoring surfaces, agent management, server ops, test harness, Discord per-env channels, chat bridge, world map, approvals, alerts.
- Deployment target: Docker-compose on Proxmox LXC.
- Hinted auth preference: Discord OAuth default.

## D. Subjective judgment calls each agent made

Not factual; ranking/selection decisions inside the research reports.

1. **Loader agent** judged the Villager Recruits 1.21.1 NF gap as a **tolerable risk** (worth sponsoring/forking a port or substituting), not a disqualifier for NeoForge 1.21.1. A different reviewer could weight this heavier.
2. **Loader agent** assigned ~80% confidence to NeoForge 1.21.1 — impressionistic, not statistical.
3. **AI-agent landscape** declared Voyager "stale" based on last-commit date; could be reopened if a fork resurrects it.
4. **AI-agent landscape** estimated "~4× verb work" for design-and-build-contraption vs operate-existing-contraption — rough order of magnitude, not measured.
5. **Quest agent** dismissed Heracles as "narrative-oriented" — a characterization based on its tree/branching model, not a hard architectural block.
6. **Texture agent** recommended training a custom LoRA on 50–80 reference tiles — assumed we have or will produce those tiles; user has not confirmed the seed set exists.
7. **Texture agent** picked SDXL over Flux 2 primarily on LoRA ecosystem breadth — a reasonable but subjective ecosystem-maturity call.
8. **Self-healing agent** defaulted to Redis Streams over NATS JetStream with "overkill until you scale past one box" — judgment call for solo dev scope.
9. **Self-healing agent** drew the auto-vs-human-approval line (auto-quarantine yes, auto-revert-git no) — a suggested default, not an asserted fact.
10. **Control-plane agent** picked Next.js over SvelteKit/Remix on solo-dev friction, not raw capability.
11. **Control-plane agent** picked Auth.js over Better Auth on Discord-OAuth maturity — Better Auth listed as a credible alternative.
12. **Control-plane agent** specified "LangGraph/Python" as primary agent-framework; agent-framework language is still an open question for the user to pick.

## E. Silent inferences the agents made without being told

These are assumptions the research accepted implicitly and that the user has not yet confirmed.

1. **Minecraft Java Edition** — no agent asked. Bedrock is never referenced. (Almost certainly right, but not stated.)
2. **DGX Spark bandwidth suffices for 3+ concurrent bots** at 5–15 tok/s with vLLM/TGI batching — asserted by the AI-agent agent, not benchmarked.
3. **The Gemini AI Companion mod stays the helper role after rebrand** — research assumes it's reskinned, not greenfielded.
4. **Friends tolerate LLM-driven peer players joining their SMP** — not explicitly verified; social/consent dynamics assumed.
5. **Mineflayer bots can join a NeoForge 1.21.1 server without custom packet-gating hitting them** — typically true, but specific mods in the final shortlist could break this.
6. **Discord voice bridge can feed live TTS/STT streams from the DGX Spark agent runtime to a Discord voice channel in near-real-time** — treated as straightforward by control-plane agent; actual latency budget unverified.
7. **The existing `mcp-sidecar/` is extensible for Mindcraft/Baritone bridges** — assumed without audit.
8. **Pterodactyl API + BlueMap API can both be proxied under our gateway** — standard pattern, but specific CSP / iframe policy not verified.
9. **LangGraph + LangSmith are acceptable operational dependencies for a local-first solo deploy** — no blocker identified, but not confirmed.
10. **Docker Compose on a single Proxmox LXC is enough for the control-plane stack** — no load estimation done.
11. **Single-guild Discord with per-env channels is the chosen topology** — recommended by control-plane agent; user has not ratified.
12. **Release is English-only**. No i18n researched.

## F. Methodology assumptions

1. Six topics was the **right scope** for the first sweep. A follow-up sweep is likely needed for Workstreams E (existing-mod audit), G (cross-mod integration), H (sandbox/test plans), and the open sub-questions each topic surfaced.
2. The per-topic reports are **advisory**; per-workstream Layer-2 deep-dives still need to ratify picks with the user.
3. **All subagents use WebSearch/WebFetch** with 2025–2026-preferred sources. If any cited source is later found stale, the conclusion built on it must be re-checked.
4. **Length budgets** (500–1200 words per report) forced omissions — many niche mods/tools were only name-checked or skipped entirely.
5. **No direct product hands-on**: no subagent installed, ran, or benchmarked any of the tools it surveyed. Every capability claim is derived from documentation + release notes.
6. **Cross-topic dependencies** were acknowledged but not re-verified. For example: the loader decision affects which quest/mod/shader builds apply; quest agent assumed "loader TBD but 1.21.x/1.20.1 on NF" rather than waiting for loader agent.

## G. Deliberate exclusions (not researched this sweep)

Flagged here so no one assumes these are settled.

1. **Specific mod shortlist** beyond loader-compatibility anchors — Workstream A deliverable, not yet started.
2. **Pack distribution model** (CurseForge vs Modrinth vs Prism vs custom) — open.
3. **DGX Spark model selection** — Qwen2.5-72B, Llama-3.3-70B, or something else was suggested but not evaluated.
4. **Agent memory architecture** (vector DB vs session-only, which DB).
5. **Voice TTS/STT provider choice** (ElevenLabs vs OpenAI Realtime vs local Piper/Whisper) — deferred to the release gate.
6. **Sandbox test-harness internals** (Workstream H).
7. **Cross-mod integration specifics** (Workstream G) — the tag-unification, Create-bridge-recipe patterns are a later deep-dive.
8. **World-lifecycle migration tooling** (Workstream O).
9. **Performance budgets / SLOs** (Workstream N) — no TPS/FPS/memory targets proposed.
10. **Repo split specifics** — Phase 2 deferred.
11. **Localization / i18n**.
12. **Accessibility specifics beyond a checklist mention** (Workstream M).
13. **Business-model deep-future scenarios** — ad-supported web app is out of scope.

---

## How to use this file

- Before ratifying any Workstream pick, re-read the assumption list relevant to it.
- If an assumption in A is wrong, the research recommendation may flip; log the correction in the plan's Living Vision Log and re-run the relevant sweep.
- If an assumption in D/E bothers you, open a targeted follow-up research sub-sweep instead of a full re-run.
