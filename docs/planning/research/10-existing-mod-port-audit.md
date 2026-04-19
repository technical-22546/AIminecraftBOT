# Gemini AI Companion Subsystem Audit: Port / Retire / Adapt Analysis

_Layer-2 re-research (R4) run 2026-04-18 — codebase audit. Summary folded into `../warm-iverson.md` Workstream E._

Based on exploration of the AIminecraftBOT codebase (~14K lines across 4 Java files + Node MCP sidecar + Python debug client), subsystem-by-subsystem audit for the **external Joe AI services**:

---

## 1. MCP Bridge (McpBridgeServer.java, 567 lines + run-mcp-sidecar-node.js, 1751 lines)

**What it does:** Exposes Minecraft world state, build planning, and actions over loopback HTTP+JSON to external agents (Claude Desktop, Codex, generic MCP clients). Includes 22 tools (inventory, buildsite, execute_build_plan, highlight, undo, etc.) and bearer-token auth.

**Port to external:** ✅ **PORT** — This is the primary lever. The MCP bridge's JSON schemas, tool surface, and HTTP endpoints should be reused *as-is* in the external Mineflayer+RCON build. The Node sidecar is already transport-agnostic and can be adapted to speak RCON instead of Java HTTP server. Extract: all tool schemas (minecraft_help, minecraft_buildsite, minecraft_execute_build_plan inputs/outputs), the prompt templates embedded in run-mcp-sidecar-node.js (buildAgentWorkflowGuide, buildBuildPlanGuide), and the endpoint contract (/v1/tools/*, /v1/actions/*).

**Target:** Control-plane gateway or dedicated Mineflayer agent runtime. Reimplement McpBridgeServer logic over RCON; keep Node sidecar MCP wrapper.

---

## 2. Structured Build Planner (VoxelBuildPlanner.java, 2491 lines)

**What it does:** Validates and compiles build_plan JSON (v2 schema with cuboids, hollow_cuboid, steps, rotation, anchors, terrain clearing) into safe Minecraft commands. Handles auto-repair (pillar support, Y-grounding, terrain flattening), rotation, volume clamping (32K blocks max), and preview/execute split.

**Port to external:** ✅ **PORT** — The build-plan data model and planner logic are pure algorithms, not Fabric-tied. Core value: the v2 schema (coordMode, origin/offset/anchor, steps[], autoFix, snapToGround, clearVegetation) and validation rules (bounds checking, floating-block detection, support pillar placement, rotation application).

**Target:** Agent runtime + inference node. Extract: (1) build-plan JSON schema as TypeScript/Pydantic models; (2) planner logic as standalone Node/Python package; (3) undo snapshot format (BlockUndoSnapshot records). The Java planner is ~2.5K lines but is stateless; porting to Python or Node is feasible.

---

## 3. Highlights (in GeminiCompanion.java, ~100 lines of highlight code)

**What it does:** Renders 3D world overlays via Fabric client-side particles/boxes to point at blocks. Data: Highlight record (x,y,z, label, colorHex, expiryMs). Sent via HighlightsPayloadS2C packet.

**Retire / Adapt:** 🛑 **RETIRE in-mod; ADAPT externally** — Highlights are client-side Fabric-only and cannot move. For external Mineflayer, replace with: (1) Litematica schematics (saved .litematic files for "ghost" structures), (2) BlueMap web-map markers, or (3) chat messages with coordinates. Recommend Litematica for build preview UX parity.

---

## 4. Undo (in GeminiCompanion.java, ~150 lines, UndoBatch + LAST_UNDO_BATCHES map)

**What it does:** Stores per-player UndoBatch (undo commands, BlockUndoSnapshot list). On `/chat undo`, replays snapshots to restore block state. Undo is **logical** over MCP (executeMcpUndo method), not in-world-only.

**Port to external:** ✅ **PORT** — Undo is MCP-aware and stateless. Extract: (1) BlockUndoSnapshot format (x,y,z, blockId, blockState, nbt?); (2) undo execution logic (replay snapshots in reverse order); (3) per-player batch map pattern. Can map to external task queue (Redis or in-memory) keyed by player UUID.

**Target:** Agent runtime. Implement snapshot capture during build execution, store in transient map or Redis, replay on undo command.

---

## 5. Vision (Client screenshot + server capture dispatch in GeminiCompanion.java & GeminiCompanionClient.java, ~200 lines)

**What it does:** Client captures PNG screenshot (VisionPayloadC2S), server receives and forwards to Gemini API for multimodal understanding. Returns image in base64.

**Adapt / Port:** ✅ **ADAPT** — Vision is client-side Fabric but the API pattern is portable. External: use **prismarine-viewer** (Mineflayer plugin) to render voxel scene to PNG, send to inference endpoint (Claude vision, local ViT, DGX-hosted model). The vision flow (capture → LLM inference → text response) is transferable.

**Target:** Inference node + agent runtime. Port: (1) screenshot capture logic to prismarine-viewer; (2) vision inference to Claude API or local model; (3) response pipeline. The mod's vision code is Fabric-specific but the UX (think, act, observe) is not.

---

## 6. Voice (STT/TTS in GeminiCompanion.java, ~100 lines audio codec + callGeminiTranscribe)

**What it does:** Push-to-talk (key V) → client captures WAV/Ogg → AudioPayloadC2S → server calls Gemini STT API (callGeminiTranscribe, ~50 lines) → transcript → prompt. TTS is **not implemented** (roadmap item).

**Port to external:** ✅ **PORT** — Voice pipeline is API-shaped and transferable. STT: replace Gemini with Whisper (local via Piper on DGX, or OpenAI API). TTS: integrate Piper (DGX-local) or ElevenLabs for Discord voice.

**Target:** Discord bot (for TTS) + inference node (for Whisper). Extract: (1) audio format negotiation (mimeType, sample rate); (2) Gemini STT call shape (can adapt to Whisper API); (3) transcript sanitization (sanitizeTranscript method). For Discord, add voice-channel output after inference.

---

## 7. Permissions (OPS / WHITELIST / ALL modes in GeminiCompanion.java, ~100 lines)

**What it does:** PermissionMode enum + AI_WHITELIST set (UUID-based) + canAccessAI() guard. Persisted in player settings JSON.

**Port to external:** ✅ **PORT** — Permissions are data-driven. Extract: (1) permission mode enum (OPS, WHITELIST, ALL); (2) whitelist storage (SQLite/JSON); (3) Discord OAuth role mapping (new: auth.js + Discord guild roles as ACL).

**Target:** Control-plane gateway + Discord bot. For external Joe AI: replace Minecraft op checks with Discord role checks (e.g., `hasRole("ai-user")`). Store whitelist in PostgreSQL or Discord API.

---

## 8. Setup Wizard (in GeminiCompanion.java, ~150 lines runSetupWizard, showMcpBridgeSetup)

**What it does:** Interactive in-world config for API key, permission mode, MCP sidecar setup. Generates ready-to-paste MCP config blocks for Codex/Claude-Desktop/etc.

**Adapt:** ✅ **ADAPT** — In-world wizard is Fabric-bound. External equivalent: web-app first-run (Next.js + Shadcn UI). Flow: (1) intro screen, (2) Discord OAuth login, (3) API key paste + store in .env.local or 1Password, (4) MCP sidecar auto-download, (5) test connection. Reuse the MCP setup generation logic (buildMcpSetupBundle in mod) as a server endpoint.

**Target:** Web app + control-plane gateway. Port: config generation templates (currently built on-the-fly in runChatSkill) to web backend.

---

## 9. RegistryHints (in GeminiCompanion.java, ~50 lines buildRegistryHints)

**What it does:** Scans item/entity/block registries for tokens matching the prompt. Returns hints like "You mentioned 'oak' — did you mean: minecraft:oak_log, minecraft:oak_planks?" Inserted into context before Gemini call.

**Port to external:** ✅ **PORT** — Pure algorithm. Extract: (1) registry match logic (findRegistryMatches method); (2) hint formatting. For external: parse modded registries from serverProperties or a registry dump (vanilla items hardcoded, modded items from mod JAR metadata).

**Target:** Agent runtime. Implement: static Minecraft registry snapshots (vanilla 1.21.1) + modded registry loader from modpack metadata.

---

## 10. Batch Command Path (PendingMcpCommandBatch in GeminiCompanion.java, ~200 lines)

**What it does:** Timed command execution via delayTicks/delayMs (using Minecraft server tick scheduler). Stores state in PENDING_MCP_COMMAND_BATCHES map, polled via minecraft_batch_status tool.

**Port to external:** ✅ **PORT** — Task queue pattern is transferable. Extract: (1) command scheduling (delayTicks to milliseconds conversion); (2) batch state machine (PENDING → RUNNING → DONE); (3) status polling contract (batchId, pending, executed, failed counts).

**Target:** Control-plane gateway or Redis Streams. Replace Minecraft tick scheduler with: (1) Redis Streams for durable queues, or (2) in-memory scheduler (node-cron for Node.js). Batch status endpoint already exists and is MCP-safe.

---

## 11. Structured Logging & Crash/Recovery (scattered across GeminiCompanion.java, ~50 lines)

**What it does:** Server-side logging via SLF4J (McpBridgeServer.LOGGER), error handling in command execution (try-catch with user feedback), retry loops (up to MAX_COMMAND_RETRIES).

**Port to external:** ✅ **PORT (partial)** — Error handling and retry logic are reusable. Extract: (1) retry loop pattern (up to 10 retries, exponential backoff placeholder); (2) error categorization (COMMAND_ERROR, API_ERROR, etc.); (3) context snapshot for debugging (last prompt, last response, executed commands).

**Target:** Agent runtime + control-plane logs. Implement: structured logging (Winston/Pino for Node, structlog for Python) with request IDs. Crash recovery: store checkpoints (last successful command, last error) in Redis or database.

---

## 12. Client/Server Wiring & Gradle Build

**What it does:** Fabric mod entry point (ModInitializer), Mixin injection, Gradle build (build.gradle, 90 lines). Event registration for ServerLifecycleEvents, ServerTickEvents, ServerPlayerEvents.

**Retire / Adapt:** 🛑 **RETIRE** — Fabric-specific. No direct external equivalent. For Mineflayer: use Mineflayer plugin hooks instead. For control-plane: use HTTP/gRPC event streams.

---

## 13. Settings Management (scattered, ~100 lines)

**What it does:** Per-player + global settings stored in JSON files (settingsDir(), savePlayerSettings, saveGlobalSettings). Includes debug mode, particles, retry limits, model preference, sidebar toggle.

**Port to external:** ✅ **PORT** — Settings persistence is portable. Extract: settings schema (JSON), storage pattern. For external: use SQLite, PostgreSQL, or .env files; keep same key names (debug, particles, retries, model).

**Target:** Web app + control-plane gateway. Implement: settings endpoint (/api/user/settings), Discord user account linking.

---

## Top 5 Port Priorities (Highest Leverage)

1. **Build Planner (VoxelBuildPlanner.java)** — Core value; unlocks reliable structured builds externally. ~2.5K lines → ~1K lines in Python/Node.
2. **MCP Bridge Surface** (schemas + tool routing) — Largest reuse surface. 22 tools already defined; adapt from HTTP to RCON or gRPC.
3. **Batch/Timed Command Executor** — Powers delayed sequences and build phasing. Minimal code; big UX impact.
4. **Undo Snapshots + State Reversal** — Safety-critical for agents; restore logic is clean and transferable.
5. **Vision Capture + Inference Routing** — Multimodal agent capability; port to prismarine-viewer + Claude API.

---

## Subsystems to Retire

- **In-world Highlights** → Replace with Litematica schematics + BlueMap markers.
- **Fabric Client-Server Wiring** → Use Mineflayer plugins.
- **Setup Wizard (in-game)** → Move to web-app first-run.

---

**Total codebase:** 14K lines Java + 1.75K lines Node MCP sidecar. **Portable core:** ~5–6K lines (build planner, undo, batch scheduler, vision routing, permissions, registry hints). **Fabric-bound:** ~8–9K lines (client rendering, packet codecs, Mixin, Gradle).
