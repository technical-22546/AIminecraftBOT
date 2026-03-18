# Release v1.2.0 - The Voxel Architect Update

This release turns Gemini AI Companion into a true structured Minecraft builder instead of a raw command generator.

### Main additions
- **Structured voxel architect**: The AI can now return a `build_plan` with cuboids, single-block placements, palettes, rotations, and phased `steps[]`.
- **Terrain-aware planning**: New `buildsite` scanning gives the model real local terrain context before it builds.
- **Planner validation and repair**: The build pipeline validates block IDs, block states, volume, coordinates, rotations, and support, then repairs common near-miss issues automatically.
- **Phased construction**: Larger builds can decompose into foundation, walls, roof, details, and follow-up passes.
- **Safer special block handling**: Better expansion and normalization for doors, beds, stairs, slabs, fences, panes, and walls.
- **Support-aware execution**: Floating plans can be repaired with support pillars, and obviously unsupported builds are rejected with useful retry feedback.
- **Undo for structured builds**: `/chat undo` can now restore terrain and block-entity state after planner-driven builds.

### Gemini runtime improvements
- **Gemini 3.1 Pro Preview** is now the Pro target.
- **Native structured output schema** is applied to Gemini requests for more reliable JSON responses.
- **Better loop control** reduces repeated tool spam and pointless ASK/TOOL bouncing on simple build requests.
- **API fallback handling** now retries cleaner on transient Gemini failures.

### Why this release matters
You can now ask for things like:
- `build me a little oak house here`
- `make a compact furnace shed next to me`
- `build a watchtower with a stone base and wood roof`

and the mod will plan the structure, validate it, repair what it can, execute it safely, and preserve an undo path.
