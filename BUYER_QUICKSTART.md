# Quantizy Buyer Quickstart

Quantizy helps local-PC users check whether a model can run with the memory they actually have free, avoid RAM thrash, and use validated Quantizy artifacts when a paid release includes them. It does not make every model fit every computer.

## What You Need

- A Mac with Apple Silicon.
- A local model folder, or a validated Quantizy artifact from your purchase email or artifact catalog.
- Your Quantizy license key.
- Enough free disk space for the model or artifact you want to test.

Quantizy does not download huge base models for you. Start with a model you already have locally, or with a validated artifact that was explicitly included with the release.

Your purchase email is the product handoff: it contains the Quantizy DMG link,
the expected SHA-256 checksum, your license key, this quickstart link, and the
support contact for order recovery.

## 1. Download and Verify

Download the Quantizy build from your purchase email.

On macOS, verify the DMG checksum:

```bash
shasum -a 256 Quantizy-macos-arm64.dmg
```

The printed value should match the `DMG SHA-256` value in your email.

## 2. Install and Activate

1. Open the DMG.
2. Drag Quantizy to Applications.
3. Launch Quantizy.
4. Paste the license key from your purchase email when prompted.
5. Keep the email. The app verifies the key offline, and the order details help with support.

## 3. Run a Safe Fit Check in the App

Before testing a large model, close extra heavy apps if possible. If you keep IDEs open, Quantizy can still account for that as long as `Use current RAM` is enabled.

1. Open Quantizy.
2. Read the `Start Here` panel. It mirrors the purchase email: save license, choose model, keep live RAM enabled, run the fit matrix, then export only after a passing recipe.
3. Choose the local model folder or Quantizy artifact you already have.
4. Enable `Use current RAM`.
5. Leave a RAM reserve for macOS and open apps.
6. Run `Fit Matrix` for a full local-model recipe, or `Fit Check` for a quick parameter-only estimate.

If the fit check passes, Quantizy will show the suggested settings and run command. If it fails, reduce context length, use a smaller quant/artifact, close heavy apps, or pick a smaller model.
Your first successful session is simple: the app accepts your license, the fit check runs against your current free RAM, and you leave with either a passing recipe or a clear fallback for the model you selected.
Advanced KV features such as tiered cold-tail paging, protected old-token pages, sparse cold-tail attention, byte-aware adaptive landmark hydration, fine-token cold-tail hydration, latent KV, KV head sharing, cross-layer shared KV, and asymmetric KIVI-style key/value sidecars are used only when the selected recipe and runner support them. Treat them as verified runtime recipes, not manual knobs you need to guess on the first run.
For validated resident/offload MoE artifacts, `Use current RAM` also enables Quantizy's RAM-pressure behavior: cold offloaded experts can start from compact fallback records and promote only after the prompt repeatedly needs them.
If RAM is tight, Quantizy may also load a compact cold-expert fallback instead
of evicting a hotter cached expert. That is deliberate: it protects the current
working set on machines that already have IDEs, browsers, or other heavy apps
open.
When your prompt shifts to a different group of experts, repeated routes can
now protect the new hot working set from stale cache history without letting
one-off cold misses churn RAM.

## 4. Optional Command-Line Fit Check

Use this when support asks for a reproducible report, or when you prefer the terminal:

```bash
moe-squeeze streaming-mlx-fit-matrix /path/to/local-model \
  --bits 2,3,4 \
  --use-current-ram \
  --reserve-gib 4 \
  --auto-context \
  --context-options 4096,8192,16384,32768,65536 \
  --auto-kv-bits \
  --auto-kv-retention \
  --auto-kv-sink \
  --kv-sink-options 0,4,8,16,32,64,128 \
  --auto-kv-residual \
  --kv-residual-options 128,256,512,1024 \
  --auto-kv-protection \
  --kv-protected-options 0,16,32,64,128,256 \
  --auto-kv-paging \
  --kv-tail-block-options 8,16,32 \
  --kv-tail-quant-axis token \
  --kv-tail-residual-sign-bits 0 \
  --auto-kv-metadata \
  --kv-landmark-block-options 0,512 \
  --auto-prefill-chunk \
  --prefill-chunk-options 512,1024,2048,4096,8192 \
  --kv-window-options 1024,4096,8192 \
  --kv-tail-bit-options 4,2 \
  --json
```

Use `recommended_recipe` when the result says `ok: true`. If it says `ok: false`, use `fallback_recipe`, lower `context_tokens`, close memory-heavy apps, or pick a smaller model. The report also includes `runtime_pressure_ladder`: if RAM gets tighter after the first check, use the matching pressure entry's `export_recipe` instead of guessing a smaller context by hand. When a recipe uses sparse or tiered KV, carry the recommended `kv_sink_tokens`; the fit check may select this automatically with `kv_sink_source: "auto_sink"`. When a recipe uses tiered KV, Quantizy counts protected cold tokens, query-sketch metadata, coarse landmark metadata, cold-tail page size, and dequantized KV page-cache MiB against the same current-RAM budget.
Leave `--kv-tail-quant-axis token` unless the recipe or support says your runner has separate key/value sidecar support. When it does, use `--kv-tail-quant-axis kivi`: Quantizy records key-cache tail packing as `channel` and value-cache tail packing as `token`, so keys get the lower-error channel layout while values keep cheap partial old-token loads.
Leave `--kv-tail-residual-sign-bits 0` unless the recipe or support says your tiered-KV runner applies residual-sign sidecars. When enabled with `1`, Quantizy stores one packed residual sign bit per cold-tail value plus fp16 per-group residual scales, and counts those bytes in the fit report as `kv_tail_residual_*`.
When `--auto-prefill-chunk` selects `prefill_chunk_tokens`, keep that value with the recipe for runners that support chunked prompt prefill. It protects against transient long-prompt memory spikes; it does not reduce the model or KV cache size.

If support tells you that your runner has latent-KV support, add this to the fit matrix command:

```bash
  --auto-kv-latent \
  --kv-latent-rank-options 0,32,64,128,256 \
  --kv-latent-min-retained-ratio 0.25
```

Only use a latent-KV recipe when the report says `ok: true` and your runner supports the reported `kv_latent_rank`. Quantizy marks these recipes with `kv_cache_requires_runtime_support`. If a smaller rank fits RAM but falls below the retained-KV floor, the report leaves it visible with `kv_latent_quality_ok: false` and does not recommend it.

If support tells you that your runner has GQA-style KV head-sharing support, add this to the fit matrix command:

```bash
  --auto-kv-head-sharing \
  --kv-head-group-options 0,2,4,8,16 \
  --kv-head-min-retained-ratio 0.125
```

Only use a KV head-sharing recipe when the report says `ok: true` and your runner supports the reported `kv_head_group_size`. Quantizy marks these recipes with `kv_cache_requires_runtime_support`. If a larger group fits RAM but falls below the retained-KV floor, the report leaves it visible with `kv_head_quality_ok: false` and does not recommend it. Stock MLX model runners do not automatically gain this behavior just because the fit report can budget it.

If support tells you that your runner has cross-layer KV support, add this to the fit matrix command:

```bash
  --auto-kv-cross-layer \
  --kv-cross-layer-group-options 0,2,4,8 \
  --kv-cross-layer-rank-options 0,32,64,128,256 \
  --kv-cross-layer-min-retained-ratio 0.125
```

Only use a cross-layer KV recipe when the report says `ok: true` and your runner supports the reported `kv_cross_layer_group_size` and `kv_cross_layer_rank`. Quantizy marks these recipes with `kv_cache_requires_runtime_support`. If a smaller grouped rank fits RAM but falls below the retained-KV floor, the report leaves it visible with `kv_cross_layer_quality_ok: false` and does not recommend it.

## 5. Export From a Passing Recipe

Only export after a fit check passes. Carry the recommended KV settings into the export. If a recommended KV field is blank or `null`, omit that flag. Add `--kv-tail-offload` only when `recommended_recipe.kv_tail_offload` is `true`:

```bash
moe-squeeze streaming-mlx-export \
  --hf /path/to/local-model \
  --out /path/to/quantizy-mlx-model \
  --bits <recommended bits> \
  --group-size 64 \
  --use-current-ram \
  --reserve-gib 4 \
  --context-tokens <recommended context_tokens> \
  --kv-bits <recommended kv_bits> \
  --kv-residual-tokens <recommended kv_residual_tokens> \
  --kv-window-tokens <recommended kv_window_tokens> \
  --kv-sink-tokens <recommended kv_sink_tokens> \
  --kv-tail-bits <recommended kv_tail_bits> \
  --kv-tail-block-tokens <recommended kv_tail_block_tokens> \
  --kv-tail-quant-axis <recommended kv_tail_quant_axis> \
  --kv-tail-residual-sign-bits <recommended kv_tail_residual_sign_bits> \
  --kv-tail-cache-mib <recommended kv_tail_cache_mib> \
  --kv-protected-tail-tokens <recommended kv_protected_tail_tokens> \
  --kv-protection-block-tokens <recommended kv_protection_block_tokens> \
  --kv-attention-score-mode <recommended kv_attention_score_mode> \
  --kv-query-sketch-dim <recommended kv_query_sketch_dim> \
  --kv-query-sketch-block-tokens <recommended kv_query_sketch_block_tokens> \
  --kv-query-sketch-dtype <recommended kv_query_sketch_dtype> \
  --kv-landmark-block-tokens <recommended kv_landmark_block_tokens> \
  --kv-landmark-dtype <recommended kv_landmark_dtype> \
  --kv-latent-rank <recommended kv_latent_rank> \
  --kv-latent-rope-dim <recommended kv_latent_rope_dim> \
  --kv-latent-extra-dim <recommended kv_latent_extra_dim> \
  --kv-latent-min-retained-ratio <recommended kv_latent_min_retained_ratio> \
  --kv-head-group-size <recommended kv_head_group_size> \
  --kv-head-min-retained-ratio <recommended kv_head_min_retained_ratio> \
  --kv-cross-layer-group-size <recommended kv_cross_layer_group_size> \
  --kv-cross-layer-rank <recommended kv_cross_layer_rank> \
  --kv-cross-layer-min-retained-ratio <recommended kv_cross_layer_min_retained_ratio> \
  --json
```

The output is a local MLX model directory. Keep the fit report with it; it tells support which memory recipe you used.

## 6. Use a Quantizy Artifact

If your purchase includes a validated Quantizy artifact:

1. Download the artifact from the link in your email or the artifact catalog.
2. Verify its checksum when one is provided.
3. Open it in Quantizy.
4. Confirm that the validation report is marked as passed.
5. Keep **Auto resident hit-rate** enabled when inspecting a resident/offload artifact. Quantizy will choose the highest saved-routing hit target that fits the RAM you currently have free, then show the selected target and candidate list.
6. Check **Offload fit** before using a resident/offload artifact:
   - **strong** means the saved routing trace had enough local reuse for bounded expert sidecar caching.
   - **usable** means it may work, but leave extra RAM headroom and expect model-specific behavior.
   - **poor** means this artifact should not be treated as a good offload candidate on the chosen cache size.
7. Check **Prefetch** when it is shown. **strong** means saved routes make offloaded misses predictable enough for a runner to warm likely experts. Keep **Auto expert prefetch slots** enabled unless support tells you otherwise; it searches the useful slot count under the current memory budget. **poor** means sidecar reads are likely to stall unless more experts are resident.
8. Use the generated run command with your local runner.

Command-line artifact check:

```bash
moe-squeeze artifact-info /path/to/quantizy-artifact \
  --use-current-ram \
  --reserve-gib 4 \
  --auto-resident-hit-rate \
  --resident-hit-rate-options 0.95,0.9,0.85,0.8,0.7,0.6 \
  --auto-expert-prefetch-slots \
  --expert-prefetch-slots 4 \
  --json
```

Do not treat draft or unvalidated artifacts as paid quality claims.

## 7. If Something Goes Wrong

- **License rejected**: paste the exact key from the purchase email and check for extra spaces.
- **DMG checksum mismatch**: stop and contact support; do not install that file.
- **Fit check fails**: this is not a broken purchase. It means the selected model/context does not fit with the memory currently free. Try `fallback_recipe`, use a lower `runtime_pressure_ladder` recipe, lower context length, close heavy apps, or use a smaller model/artifact.
- **App opens but feels slow**: rerun the fit check with a larger RAM reserve.
- **No purchase email**: contact support with your checkout email. The license worker records a recoverable buyer handoff before email is sent.

## 8. What V1 Is For

Quantizy V1 is best for:

- Checking whether a local model fits on your real machine today.
- Avoiding crashes from optimistic RAM estimates.
- Packaging and opening validated local artifacts.
- Running validated resident/offload MoE artifacts with current-RAM pressure fallbacks when other apps are still open.
- Protecting changing MoE prompt working sets with burst-aware sidecar cache scoring.
- Using advanced KV recipes only when the app or support workflow marks the selected runner as compatible.
- Using residual-sign corrected cold-tail KV sidecars only when the selected tiered-KV runner can apply them.
- Trying guarded latent-KV, KV head-sharing, or cross-layer-KV runtime recipes only when the selected runner can actually execute them.
- Keeping license activation simple and offline-friendly.

Quantizy V1 is not a guarantee that a huge frontier model will run on a small machine, and it does not claim to beat every existing GGUF or dynamic quantization method.
