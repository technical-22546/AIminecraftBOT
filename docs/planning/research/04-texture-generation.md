# AI Texture Pipeline for "Warm Iverson" Minecraft Modpack (April 2026)

_Full research report. Summary folded into `../warm-iverson.md` Workstream B._

## Field Survey

### Stable Diffusion Family (local, $0/mo)
On a DGX Spark (128 GB unified, GB10), both **SDXL** and **Flux.1** are comfortable. NVIDIA benchmarks show Flux.1 12B at FP4 producing a 1K image in ~2.6 s, and BF16 SDXL at ~7 images/min — both dramatically overkill for 16x16 generation, which means we can run CN+LoRA stacks cheaply ([NVIDIA DGX Spark perf blog](https://developer.nvidia.com/blog/how-nvidia-dgx-sparks-performance-enables-intensive-ai-tasks/), [LMSYS review](https://www.lmsys.org/blog/2025-10-13-nvidia-dgx-spark/)). SDXL still wins on **LoRA/ControlNet ecosystem breadth** in 2026; Flux.1 and Flux 2 win on base prompt fidelity ([Apatero Flux2-vs-SDXL](https://apatero.com/blog/flux-2-vs-stable-diffusion-xl-comparison-2026), [pxz.ai](https://pxz.ai/blog/flux-vs-sdxl)). No canonical "minecraft-block-texture-v3" LoRA surfaced on Civitai as of April 2026; closest relevant LoRAs are **Pixel Art XL** (SDXL LoRA, 8 steps, CFG 1.5, strength 1.2) and general pixel-art Flux LoRAs ([Pixel Art XL](https://www.promptlayer.com/models/pixel-art-xl/)). Expect to train a small custom LoRA on 30–80 reference tiles.

### Pixel-art-specialized generators
- **PixelLab.ai** — Hosted API, free trial = 40 fast gens; tilesets and animated sprites supported ([PixelLab API](https://www.pixellab.ai/pixellab-api)). Quality for indie sprites is strong; less proven for Minecraft's specific 16x16 rasterization.
- **Retro Diffusion** — Purpose-built authentic pixel-art diffusion, commercial API via Runware ([Retro Diffusion](https://runware.ai/blog/retro-diffusion-creating-authentic-pixel-art-with-ai-at-scale)).

### Commercial image APIs (fallback)
As of April 2026, per-image pricing ([IntuitionLabs](https://intuitionlabs.ai/articles/ai-image-generation-pricing-google-openai), [BuildMVPFast](https://www.buildmvpfast.com/api-costs/ai-image)):
- **Imagen 4 Fast** $0.02, **Standard** $0.04, **Ultra** $0.06
- **Gemini 3 Pro Image** ~$0.035 (best in-image text, edit fidelity)
- **GPT Image 1.5** premium, **Mini** $0.005 (cheapest)
- **Ideogram 3.0** $0.03, **Flux 2 Pro** mid-range

Batch APIs cut costs ~50%. **Anthropic does not ship an image-generation API** in April 2026 — use Claude only for prompt-writing/evaluation. Commercial ToS generally permit distributed use with some restrictions; verify per provider before shipping in modpack.

### Minecraft-specific AI tools
**Craftbench**, **Bloxal**, **Magic Palette / Magic Block**, **Media.io Minecraft Texture Generator**, **Pixel GPT (SpigotMC)**, **McTexturePack-AI**-style bulk tools all exist, mostly SaaS with per-credit pricing and variable quality ([Craftbench](https://www.craftbench.ai/), [Bloxal](https://bloxal.com/), [Magic Block](https://block.magicpalette.io/)). They're good for idea generation and one-offs; weak for repeatable pipelines and CTM/CIT coherence.

### Classical + AI augmentation
- **Real-ESRGAN** pixel model for pre-upscale/denoise cycles ([Planet Minecraft ESRGAN test](https://www.planetminecraft.com/forums/communities/texturing/i-upscaled-1-17-16x16-vanilla-minecraft-with-esrgan-and-the-results-are-pretty-cool-627666/))
- **ComfyUI-PixelArt-Detector** — palette loader, KMeans/best-offset downscale to 16x16 ([repo](https://github.com/dimtoneff/ComfyUI-PixelArt-Detector))
- **PBRify_Remix** and **ComfyUI-TextureAlchemy** — albedo -> normal/roughness/AO for chiseled/parallax look ([PBRify](https://github.com/Kim2091/PBRify_Remix), [TextureAlchemy](https://github.com/amtarr/ComfyUI-TextureAlchemy))
- **DSINE/BAE** normal-map preprocessors for fake parallax depth cues on flat textures.
- **ComfyUI-seamless-tiling** node for tileable outputs ([repo](https://github.com/spinagon/ComfyUI-seamless-tiling)).

## 1. Recommended Primary Pipeline (local, $0)

**Base:** SDXL-base + **Pixel Art XL LoRA** (strength 0.9) + custom "warm-iverson-block" LoRA (train on 50–80 curated tiles) + **ControlNet-Depth** for chiseled geometry cues (feed a simple Blender/Minecraft depth render or LooseControl-style coarse depth) + **seamless-tiling** node.

**Tooling:** ComfyUI on DGX Spark, workflow modeled after [LooseControl-with-Minecraft](https://openart.ai/workflows/nomadoor/loosecontrol-with-minecraft/5OMvauBIIKJ5Ww7liJ0X) and the Depth-ControlNet reference ([OpenArt](https://openart.ai/workflows/openart/controlnet-depth/F53UyK70KGj2KrL8whYa)).

**Post:** Generate at 512x512 -> Real-ESRGAN pixel model pass -> PixelArt-Detector best-offset downscale -> KMeans quantize to a 32–64 color locked palette -> optional PBRify for normal/rough/AO side-outputs.

**Throughput:** Hundreds/hour trivially on DGX Spark.

## 2. Fallback Pipeline (quality-critical hero blocks / item icons)

**Gemini 3 Pro Image** for best edit/variation consistency, OR **Ideogram 3.0** when readable symbols are required on panels/signs. Pipe output through the same PixelArt-Detector + palette-lock post-process so hero assets visually match the local-generated bulk. Budget: $5–20 covers a release's worth of hero icons.

## 3. Prompt Library Skeleton

All prompts append a shared suffix: `, 16x16 pixel art texture, seamless tileable, top-down orthographic, limited palette, crisp pixel edges, warm iverson style`. Shared negative: `blurry, anti-aliased, photographic, isometric perspective, text, watermark, signature, 3d render, smooth gradient, noise`.

| Category | Sample prompt |
|---|---|
| Stone variants | `weathered granite block, cool grey with mica flecks, chiseled bevel edges` |
| Metal blocks | `polished industrial steel plate, rivets on corners, brushed metal sheen` |
| Wood planks | `oak plank, vertical grain, warm amber tone, knot in center` |
| Ores | `deepslate embedded with glowing cobalt crystals, sharp facets` |
| Tech / machine | `sci-fi machine panel, blue LED indicator, vent grille, dark gunmetal` |
| Decorative | `ornate carved sandstone, filigree relief, desert-tomb motif` |
| Item icons | `hand-held iron pickaxe, 3/4 view, drop shadow, transparent background` |

## 4. Iteration Loop (Think -> Generate -> Evaluate -> Refine -> Assign)

**Think:** Agent reads a YAML spec (block id, category, adjacency constraints, CTM/CIT role) -> fills prompt template from library.

**Generate:** ComfyUI batch of N=8 seeds per spec.

**Evaluate** — automated scoring:
1. Palette conformity (% pixels matching locked palette)
2. Tileability (self-seam pixel-diff after 90° shift)
3. Contrast/mean-luminance band matching neighbor blocks
4. Edge crispness (Laplacian variance post-downscale)
5. Semantic check — small VLM (Claude or local Llama-3.2-Vision) asks: "Does this look like <prompt>? Rate 1–5."
6. Deduplication hash against the existing pack.

Threshold gate: score >= 0.75 auto-accept; 0.5–0.75 queued for human review; <0.5 auto-refine.

**Refine:** img2img with CFG 3–5, 0.35 denoise, reusing seed + adjusted prompt weights.

**Assign:** Write PNG + `.mcmeta` + CTM/CIT JSON to the pack tree; commit to git with the spec hash in commit message for reproducibility.

## 5. Open Questions

1. Target Minecraft edition — **Java with Optifine/Continuity (CTM)** vs **Bedrock RTX PBR**? Changes the normal-map pipeline.
2. Baseline resolution — **16x** (vanilla feel) or **32x** (more "3D" room)?
3. Is a hand-drawn seed set available for custom LoRA training, or do we bootstrap from a license-compatible public pack?
4. Distribution channel (CurseForge/Modrinth) and need for a human-authored attribution / AI-disclosure file?
5. Is parallax/POM (depth texture) in scope, or only stylistic "fake 3D" via shading?
6. Budget ceiling for fallback API spend per release (suggest $25)?
7. Should the agent auto-commit to git, or produce a PR for human review?

## Sources
- [NVIDIA DGX Spark performance blog](https://developer.nvidia.com/blog/how-nvidia-dgx-sparks-performance-enables-intensive-ai-tasks/)
- [LMSYS DGX Spark review](https://www.lmsys.org/blog/2025-10-13-nvidia-dgx-spark/)
- [Flux 2 vs SDXL comparison (Apatero)](https://apatero.com/blog/flux-2-vs-stable-diffusion-xl-comparison-2026)
- [Flux vs SDXL 2026 (pxz.ai)](https://pxz.ai/blog/flux-vs-sdxl)
- [Pixel Art XL LoRA](https://www.promptlayer.com/models/pixel-art-xl/)
- [PixelLab API](https://www.pixellab.ai/pixellab-api)
- [Retro Diffusion on Runware](https://runware.ai/blog/retro-diffusion-creating-authentic-pixel-art-with-ai-at-scale)
- [AI Image API pricing April 2026 (BuildMVPFast)](https://www.buildmvpfast.com/api-costs/ai-image)
- [AI Image Pricing 2026 (IntuitionLabs)](https://intuitionlabs.ai/articles/ai-image-generation-pricing-google-openai)
- [Craftbench](https://www.craftbench.ai/)
- [Bloxal](https://bloxal.com/)
- [Magic Block](https://block.magicpalette.io/)
- [ComfyUI PixelArt-Detector](https://github.com/dimtoneff/ComfyUI-PixelArt-Detector)
- [PBRify_Remix](https://github.com/Kim2091/PBRify_Remix)
- [ComfyUI-TextureAlchemy](https://github.com/amtarr/ComfyUI-TextureAlchemy)
- [ComfyUI-seamless-tiling](https://github.com/spinagon/ComfyUI-seamless-tiling)
- [LooseControl with Minecraft workflow](https://openart.ai/workflows/nomadoor/loosecontrol-with-minecraft/5OMvauBIIKJ5Ww7liJ0X)
- [Depth ControlNet reference workflow](https://openart.ai/workflows/openart/controlnet-depth/F53UyK70KGj2KrL8whYa)
- [ESRGAN on vanilla Minecraft test](https://www.planetminecraft.com/forums/communities/texturing/i-upscaled-1-17-16x16-vanilla-minecraft-with-esrgan-and-the-results-are-pretty-cool-627666/)
