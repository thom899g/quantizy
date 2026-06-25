<p align="center">
  <img src="assets/quantizy-hero.png" alt="Quantizy compresses huge local AI models into a smaller Mac memory footprint" width="100%">
</p>

# Quantizy

**Fit a bigger brain on your Mac.**

Quantizy is a native Apple Silicon app for local-model fit checks, memory
guardrails, and validated MoE artifact handoff. It is built for normal users who
want to understand what can actually run on their own Mac before they waste time
and disk space downloading huge model weights.

## Buy The Paid Beta

**Early paid beta: $19.**

Sponsor `thom899g` for **$19 or more** on GitHub Sponsors, then open a license
request from the same GitHub account. Your beta access includes the signed Mac
app, buyer quickstart, checksum verification, and manual offline license
delivery.

- Pay / sponsor: https://github.com/sponsors/thom899g
- Download: https://github.com/thom899g/quantizy/releases/tag/v0.1.9
- Request license: https://github.com/thom899g/quantizy/issues/new/choose
- Planned V1 personal license after beta: **$49**

## Paid Beta Status

**Manual paid beta access is live through GitHub Sponsors.** The signed and
notarized `0.1.9` build is available from GitHub Releases, and paid activation
is handled by manual license delivery while automatic Stripe fulfillment is
being brought online.

Current gate:

- GitHub release DMG:
  `https://github.com/thom899g/quantizy/releases/download/v0.1.9/Quantizy-macos-arm64.dmg`
  — live
- GitHub Sponsors: `https://github.com/sponsors/thom899g` — live for
  early/manual beta access
- Public worker: `https://worker.ontarioprotocol.com/ready` — not resolving
- Ontarioprotocol DMG mirror: `https://ontarioprotocol.com/Quantizy-macos-arm64.dmg`
  — not live
- Local release: signed/notarized `0.1.9` artifact matches the checksum below

Do not pay anyone claiming to sell Quantizy from a different repository. The
public product front is:

```text
https://github.com/thom899g/quantizy
```

## What Buyers Get

- macOS Apple Silicon app
- Offline-verifiable license key
- Release manifest and SHA-256 checksum
- Buyer quickstart
- Current-RAM fit checks and pressure-ladder recipes
- Validated artifact handoff where evidence exists
- Manual support while Stripe automation is still being wired up

## Verify

```bash
shasum -a 256 Quantizy-macos-arm64.dmg
```

Expected SHA-256:

```text
bfc39f8a38893ebe0b75b64c49eb7930c523619fa87ae6f6a3d9d55c9c8baa1e
```

The app and DMG are Developer ID signed, Apple-notarized, stapled, and
Gatekeeper-accepted.

> The checksum is retained so the hosted DMG can be verified once the public
> mirrors are restored. Automatic Stripe checkout must stay closed until the
> worker readiness endpoint and hosted mirror both pass.

## Honest Scope

Quantizy V1 is not a universal "run any giant model on any small PC" miracle.
The sellable V1 promise is narrower and more useful:

- local Mac fit checks that account for current free RAM
- pressure-ladder recipes when IDEs, browsers, and other apps shrink available memory
- memory guardrails for machines already running other heavy local tools
- paid app activation with offline-verifiable licenses
- validated-artifact handoff where measured evidence exists
- clear fallback guidance when a model does not fit

The compression-quality claim has robust evidence on OLMoE and Granite target
gates, but broader buyer-facing models such as Qwen-class targets still need
their own validation before being marketed as a breakthrough.

## 0.1.80 Core Delta

Quantizy now produces a unified `resource_recovery_ladder` in the recommended
action. Instead of scattering the next move across RAM hints, SSD sidecar hints,
and context fallback fields, the report gives one ordered list of practical
choices:

- free RAM for a safer high-context retry
- free SSD space for a better offloaded KV tail
- use the known fitting context fallback
- run the selected recipe when no resource recovery is needed

This makes the "huge model on a crowded machine" workflow clearer: the app can
show the next physical constraint to fix before the user wastes time on a run
that fails for RAM, disk sidecar space, or context budget.

## 0.1.79 Core Delta

Quantizy's KV offload planner is now disk-headroom aware. The fit matrix can
accept either an explicit free-space budget or an offload directory to probe,
then reports whether the SSD sidecar reserve fits before recommending a long
run.

The new `offload_disk_plan` fields include:

- disk headroom status
- probed or declared free MiB
- exact sidecar reserve shortfall when the disk is too tight
- the same plan surfaced in `recommended_action`

This closes another practical gap for smaller machines: the app no longer only
asks "does RAM fit?" when the winning recipe depends on spilling cold KV state
to disk.

## 0.1.78 Core Delta

Quantizy now emits an offload disk plan for tiered KV recipes. When the best
path uses disk-backed KV sidecars, the fit report includes:

- selected sidecar bytes
- current and recommended offload budgets
- recommended SSD reserve with slack
- a rerun command that raises the sidecar budget when a better-quality tail is
  blocked only by the offload limit

This is aimed at the real "lesser PC" case: keep RAM pressure low by spilling
cold KV state to a fast local SSD, while making the disk requirement explicit
before the user starts a long run.

## 0.1.77 Core Delta

Quantizy now reports a concrete free-RAM target for tight high-context recovery
attempts. When a KV recipe is estimated to fit only by a knife-edge margin, the
recovery plan no longer just says "try it"; it shows the safer context rerun and
the extra MiB/GiB to free before retrying full context on a crowded Mac.

That matters for real buyers because the fit check now accounts for the messy
desktop case: VS Code, browsers, local servers, and other IDEs already consuming
memory before a model run starts.

## 0.1.76 Core Delta

Recovery plans now include effort-aware scoring metadata for each next action.
Every recovery command reports:

- `effort_score`
- `effort_tier`
- retained requested-context ratio
- extra context tokens versus the fallback cap
- decision score per effort point
- rationale for the effort tier

The command summary also reports:

- counts by effort tier
- best effort-adjusted command
- lowest-effort strict context-preserving command
- lowest-effort full-context-targeting command

Why it matters: normal users should not jump straight to the heaviest research
path if a cheap rerun or known cap is the right move. Quantizy can now separate
highest-upside actions from lowest-effort practical actions.

## 0.1.75 Core Delta

KV-bottleneck recovery plans now include a DeepSeek-style MHA2MLA budget planning
step when no latent-attention adapter is active.

When KV compression can theoretically close the memory gap, the receipt adds:

- `try_mha2mla_budget=true`
- a ready-to-run `streaming-mlx-mha2mla-budget` command
- automatic output weighting
- attention-error gating
- automatic latent-cache policy search
- turbo/affine latent-cache codec options

Why it matters: if ordinary KV compression is not enough, Quantizy now points
users toward a validated latent-attention adapter planning path before giving
up on the requested context. This keeps the DeepSeek-style method in the core
memory workflow instead of as a separate expert-only tool.

## 0.1.74 Core Delta

Fit receipts now include an `offload_budget_lift_hint` when a better tiered-KV
tail recipe is blocked only by the configured sidecar/offload budget.

The hint reports:

- current offload budget
- required offload budget for the better recipe
- extra sidecar bytes needed
- current versus target KV tail quality score
- target key/value tail bits
- exact rerun command with `--kv-tail-offload-budget-mib`

Why it matters: on smaller PCs, disk/SSD sidecar space is often cheaper than
resident RAM. Quantizy can now tell users when adding a small amount of KV
sidecar budget unlocks a better long-context recipe without increasing RAM.

## 0.1.73 Core Delta

Recovery plans now classify the dominant memory bottleneck for each next action.
The receipt can distinguish:

- `kv_cache_capacity`
- `prefill_workspace`
- `weight_or_non_kv_runtime`
- `mixed_memory_pressure`
- `none`

Each recovery command now carries a `memory_bottleneck` object with the method
family to try, recommended methods, memory relief target, component shares, and
a plain-language rationale.

Why it matters: smaller-PC users need diagnosis, not just failure. Quantizy can
now say whether the next best move is KV compression/offload, chunked prefill,
weight-memory reduction, survival priority, or simply running the known recipe.

## 0.1.72 Core Delta

Recovery hints now detect prompt-prefill workspace failures and recommend an
auto-prefill rerun before forcing users to lower context. If the model weights
and KV cache are close but full prompt prefill blows past RAM, the receipt adds:

- `try_auto_prefill_chunk=true`
- a ready-to-run `--auto-prefill-chunk` command
- the chunk option grid used for the retry
- a scored `try_auto_prefill_chunk` recovery-plan item
- the original blocked prefill bytes and available workspace budget

Why it matters: long prompts can fail because transient attention workspace is
too large even when persistent model memory is manageable. Quantizy now points
normal local users toward chunked prefill as the cheaper first fix.

## 0.1.71 Core Delta

Recovery receipts now include a `survival_ladder` for oversized context runs.
The ladder orders the next commands a crowded local machine should try:

- safest high-context KV rerun with a computed safety margin
- known fitting fallback context cap
- ambitious full-context Auto-KV search
- pressure-aware survival rescan for machines with IDEs/browsers already open

Each ladder entry reports retained context, extra tokens versus the fallback
cap, the multiplier versus that cap, fit status, memory gap, and the exact
command to run.

Why it matters: Quantizy now gives normal users an actionable RAM-pressure playbook
instead of a single "does not fit" answer. It can show when a safer KV rerun
still preserves substantially more context than the hard fallback cap.

## 0.1.32 Core Delta

The core fit engine now turns failed oversized-context checks into executable
recovery hints. When a requested context does not fit, the JSON receipt includes
ready-to-run rerun commands for both practical next attempts:

```bash
python -m moe_squeeze.cli streaming-mlx-fit-matrix /path/to/model --auto-kv-recipe --json
```

```bash
python -m moe_squeeze.cli streaming-mlx-fit-matrix /path/to/model --fit-priority survival --survival-pressure-level auto --json
```

This matters for normal local users: if VS Code, browsers, or other IDEs are
already eating RAM, Quantizy can point them toward pressure-safe survival checks
or tiered/offloaded KV recipes instead of only saying "lower context." The
receipt still marks these as memory-fit guidance, not quality claims.

## 0.1.33 Core Delta

Oversized-context failures now include a ranked recovery plan, not only one
hint. The receipt orders the next attempts as:

1. Try auto-KV recipes to preserve the requested context with tiered/offloaded
   KV.
2. Try survival priority for crowded local RAM when IDEs and browsers are open.
3. Fall back to the known fitting context cap when the original request cannot
   be preserved.

Each recovery item includes an executable command hint, expected context impact,
and an explicit claim scope. That makes Quantizy more useful for normal users:
instead of a dead-end failure, they get a small local optimization plan.

## 0.1.34 Core Delta

Recovery plans now include quantitative memory-impact estimates. For every
oversized-context recovery step, the JSON receipt reports the runtime budget,
expected context, estimated total/KV memory, headroom, and minimum memory relief
needed when the request is over budget.

That makes the lesser-PC story more concrete: users can see whether the next
move is a stretch attempt to preserve context, a pressure-safe rerun for crowded
RAM, or a known fitting context cap with zero extra memory relief required.

## 0.1.35 Core Delta

Recovery-plan items now include machine-readable feasibility verdicts. Quantizy
labels each path as `known_fits`, `needs_recipe_search`, or
`needs_pressure_aware_rescan`, and reports the memory gap that a recipe change
must close.

This makes the optimizer less hand-wavy: preserve-context attempts are clearly
marked as searches that must beat the current memory gap, while context-cap
fallbacks are marked as known fitting paths from the scan.

## 0.1.36 Core Delta

Recovery-plan items now carry deterministic `decision_score`,
`decision_tier`, and `decision_score_reasons` fields. The score is transparent:
it rewards context retention, known fitting paths, recipe-search potential, and
penalizes the current memory gap.

This gives the app a stable way to explain why auto-KV, survival mode, or a
context cap is being suggested first, without pretending the score is a quality
benchmark.

## 0.1.37 Core Delta

Oversized-context receipts now include `best_recovery_action`, a compact summary
of the highest-scored recovery path. It carries the chosen action, score, tier,
memory gap, expected context, claim scope, and runnable command hint.

The full ranked `recovery_plan` is still present, but the app can now show a
clear top-line next move immediately.

## 0.1.38 Core Delta

Recovery receipts now expose both plan order and score order. Each plan item has
`score_rank`, and the receipt includes `recovery_plan_score_order`, a compact
score-sorted view for UI surfaces that want to show highest-confidence actions
first.

This keeps the original exploration order stable while making the scored
optimizer path explicit.

## 0.1.39 Core Delta

`best_recovery_action` now includes an `explanation` object with a short
headline and buyer-safe detail text. The explanation names why the action was
selected, mentions the memory gap when relevant, and keeps quality validation
separate from memory-fit guidance.

That lets the app show a plain-language next step without making unsupported
quality claims.

## 0.1.40 Core Delta

`best_recovery_action` now includes `vs_context_cap`, comparing the selected
path against the known fitting fallback. It reports the context cap, expected
context if the selected path succeeds, extra context tokens preserved, context
multiplier, and fallback fit status.

This makes the tradeoff visible: users can see exactly what they might gain by
trying auto-KV or survival before accepting the safe context cap.

## 0.1.41 Core Delta

The `vs_context_cap` tradeoff now also reports fallback context loss:
requested context, lost tokens, retained-context ratio, and lost-context ratio.

That makes the safe fallback cost explicit, so users can see both sides of the
choice: how much context they lose by accepting the cap, and how much they might
preserve if the best recovery path succeeds.

## 0.1.42 Core Delta

Recovery command hints now include `memory_target` metadata. Each runnable hint
states the expected context, whether it preserves the requested context, runtime
budget, minimum memory relief required, fit status, feasibility verdict, and
decision score.

That ties every suggested rerun to the concrete memory gap it needs to solve.

## 0.1.43 Core Delta

Recovery command hints now also include `target_summary`, a short human-readable
one-liner generated from the same `memory_target` metadata.

This gives CLI and app surfaces a ready explanation beside each rerun command,
such as whether it must recover memory or already fits the current budget.

## 0.1.44 Core Delta

Recovery command hints now include `target_status` labels:
`memory_gap_to_close`, `fits_current_budget`, or `needs_rerun`.

This lets app surfaces badge rerun commands directly without parsing the
human-readable target summary.

## 0.1.45 Core Delta

Oversized-context receipts now include `recovery_command_summary`, grouping
runnable recovery commands by target status. The summary counts commands and
separates gap-closing attempts from commands that already fit the current
budget.

This gives the app a direct "what can I run now?" surface without traversing
every recovery-plan item.

## 0.1.46 Core Delta

Recovery command memory targets now include normalized gap size:
`minimum_memory_relief_ratio` and `minimum_memory_relief_percent`, and the
human-readable target summary includes the percentage of budget that must be
recovered.

That helps distinguish small over-budget misses from major recipe-search
problems.

## 0.1.47 Core Delta

Recovery command memory targets now include
`minimum_memory_relief_severity`, classifying normalized gaps as `none`,
`small`, `moderate`, `large`, or `extreme`.

This gives UI surfaces a direct way to communicate how hard a recovery command
has to work before it can fit.

## 0.1.48 Core Delta

Recovery command summaries now aggregate memory-gap severity across all runnable
recovery commands. Receipts report severity counts, worst severity, and the
highest relief percentage plus the command that requires it.

This lets the app explain whether the overall recovery situation is easy,
moderate, or extreme without inspecting every command row.

## 0.1.49 Core Delta

Oversized-context fit receipts now include a `kv_compression_target`. When a
requested context misses the current RAM budget, Quantizy computes whether
KV-only compression could close the gap and reports the required KV retained
ratio, reduction percentage, and compression factor.

This turns DeepSeek-style latent KV and stronger tiered/asymmetric KV work into
a concrete local target: the app can now say how aggressive KV compression must
be before it can preserve the user's requested context.

## 0.1.50 Core Delta

Recovery plans now consume the `kv_compression_target`. When KV-only compression
can close an oversized-context memory gap, the auto-KV recovery item carries the
target into its command hint, explanation, and decision-score reasons.

This makes the next action less generic: Quantizy can tell the user that a
latent/tiered KV search is the right next attempt and how much KV compression it
must achieve before lowering context.

## 0.1.51 Core Delta

Auto-KV recovery commands are now target-aware. When the receipt proves that
KV-only compression could preserve the requested context, the generated rerun
command automatically enables deeper memory-method searches: latent KV, KV head
sharing, asymmetric/tiered tail precision, tail-axis search, paging, and metadata
search.

That makes the receipt immediately actionable for smaller or crowded machines:
the first rerun searches the methods most likely to hit the computed compression
target instead of only toggling a generic auto-KV mode.

## 0.1.52 Core Delta

Target-aware Auto-KV commands now tune their search grids from the required KV
retained ratio. Very tight targets expand the rerun with more aggressive latent
rank options, larger KV head/cross-layer sharing candidates, lower channel
retention floors, and compact asymmetric tail precision options.

That makes the first recovery rerun more serious: if the receipt says the model
needs roughly 6x KV compression, Quantizy no longer wastes the attempt on a weak
default grid.

## 0.1.53 Core Delta

Target-aware Auto-KV hints now include an explicit `target_aware_profile`.
Receipts label the recovery profile (`aggressive`, `moderate`, or `light`),
carry the required KV retained ratio/compression factor, list the selected
method axes, count tuned option grids, and explain the rationale.

This gives the app a stable summary for users: it can say why Quantizy chose an
aggressive latent/sharing/pruning search instead of forcing the UI to parse raw
CLI arguments.

## 0.1.54 Core Delta

Target-aware KV search profiles now include search-breadth metadata:
per-option value counts, an upper-bound candidate multiplier, and a
`search_cost_tier`.

This lets the app warn when a recovery attempt is intentionally broad. For very
tight memory targets, Quantizy can now say the profile is `very_high` cost
before launching an aggressive latent/sharing/pruning search.

## 0.1.55 Core Delta

Target-aware Auto-KV hints now include staged recovery commands. Very broad
profiles expose a low-cost `quick` command first, then the full target-aware
search as a second stage.

This is better for smaller PCs: users can try a fast latent/head-sharing/tail
precision pass before launching the expensive full search across every selected
KV axis.

## 0.1.56 Core Delta

Staged Auto-KV hints now include `recommended_stage`, `escalation_stage`, and an
`escalation_condition`.

The app can default to the low-cost quick stage and only offer the full expensive
search when the quick stage fails to preserve the requested context.

## 0.1.57 Core Delta

Recovery command summaries now aggregate staged Auto-KV recovery directly. The
receipt reports staged command count, quick/full stage counts, cost-tier counts,
recommended stage counts, and the quick-to-full escalation path with candidate
multipliers.

This makes the local-PC workflow cleaner: the app can show "try the quick
low-cost recovery first, escalate to the full search only if needed" without
digging through every nested command hint.

## 0.1.58 Core Delta

Staged Auto-KV commands now include a `target_fit_estimate` for each stage. The
quick stage can be marked as a cheap screen when the required KV retained ratio
is very tight, while the full stage can be marked as the target-reaching search
when its pruning floors reach the computed memory target.

That matters on lesser PCs: Quantizy can now explain why a fast attempt is worth
trying without overselling it, and why a deeper full search is probably needed
for extreme context requests.

## 0.1.59 Core Delta

Auto-KV hints now include a `stage_target_summary` that names the first staged
command with an explicit target-reaching search, its cost tier, and whether the
recommended quick stage must escalate before the computed KV target is actually
covered.

This makes the app's next-step guidance sharper for cramped machines: users can
start with the cheap stage, but the receipt can plainly say when the full stage
is the first one designed to reach the memory target.

## 0.1.60 Core Delta

Staged Auto-KV target estimates now include byte-level memory closure fields:
estimated retained KV bytes, bytes removed, residual memory gap after the stage,
and whether that stage is estimated to close the current gap.

This makes Quantizy more useful on smaller or already-busy PCs because it can
separate a cheap exploratory stage from a stage whose explicit KV floor is
estimated to make the requested context fit.

## 0.1.61 Core Delta

Staged KV closure estimates now include margin and confidence fields. Receipts
report how many bytes of estimated slack remain after closing the memory gap,
the margin ratio, and whether the fit is `knife_edge`, `thin`, or
`comfortable`.

That helps crowded local machines: Quantizy can now warn when a huge-context
recovery technically closes the gap but leaves almost no buffer for real runtime
noise.

## 0.1.62 Core Delta

Knife-edge staged KV fits now produce a `runtime_safety_action`. When a stage is
estimated to close the gap with almost no margin, the receipt tells the app to
free RAM or reduce context before trusting the fit.

This turns the margin label into an actionable local-PC guardrail instead of a
passive warning.

## 0.1.63 Core Delta

Knife-edge KV recovery stages now include a computed safety-adjusted context
target. The receipt carries the 5% memory-gap safety margin, the safer context
token count, and how many tokens to reduce from the requested context.

That makes the local-PC advice executable: instead of only saying "reduce
context," Quantizy can provide a concrete context target that gives the recovery
stage breathing room.

## 0.1.64 Core Delta

Safety-adjusted KV recovery now includes a ready-to-run command. When a staged
full search is knife-edge, the receipt rewrites the command with the safer
`--context-tokens` value while preserving the selected KV search flags.

This removes another manual step for users on crowded machines: Quantizy can
offer the safer rerun directly.

## 0.1.65 Core Delta

Recovery command summaries now expose safe reruns directly via
`safe_rerun_commands` and `safe_rerun_command_count`. The app no longer has to
walk nested stage metadata to find the safer command for knife-edge KV fits.

This makes the rescue flow easier to automate: show the safe command first,
then let advanced users inspect the full staged plan if they want.

## 0.1.66 Core Delta

Recovery summaries now choose a `best_safe_rerun_command`. When multiple safe
reruns exist, Quantizy prefers the one that preserves the most context, then the
cheaper search tier, then the better-ranked recovery item.

This gives the app one obvious next action for crowded machines instead of a
bag of equivalent-looking rescue commands.

## 0.1.67 Core Delta

`best_recovery_action` now carries its own `safe_rerun_command` when the
highest-ranked recovery path has a safer knife-edge rerun. The command includes
the stage, cost tier, safety action, context reduction, confidence label, and
ready-to-run CLI arguments.

This lets the app show one primary recovery action with the safer command
attached, instead of cross-referencing separate summary sections.

## 0.1.68 Core Delta

Safe rerun commands now include a stable explanation with a headline and detail
text. For knife-edge KV fits, the explanation names the tight margin and states
the computed safer context target plus the token reduction.

This keeps the app's user-facing recovery copy tied to the same memory math as
the command, rather than making each surface invent its own wording.

## 0.1.69 Core Delta

Summary-level `safe_rerun_commands` now carry the same stable explanation as the
best recovery action. Whether the app reads the best action or the command
summary, it gets consistent user-facing copy for knife-edge reruns.

This keeps the automated rescue path coherent across UI surfaces.

## 0.1.70 Core Delta

Safe rerun explanations now compare the safer KV rerun against the known
fallback context cap. The receipt reports extra context tokens preserved and the
context multiplier versus the fallback cap.

This helps users see why the safer KV rerun is still worth trying even after
lowering context for runtime margin.

The public Mac DMG remains `0.1.9`; these sections track the faster-moving open
core until the next packaged app build is cut.

## Release

- Version: `0.1.9`
- Notary submission: `a98eea9a-e985-4506-88a2-06199f9a17e7`
- Signing identity: `Developer ID Application: THOMAS TOBIAS HANSEN (857AHQ92PC)`
- Release manifest: [`Quantizy-release.json`](./Quantizy-release.json)
- Quickstart: [`BUYER_QUICKSTART.md`](./BUYER_QUICKSTART.md)
- Checksums: [`CHECKSUMS.txt`](./CHECKSUMS.txt)

## Paid Beta Access

`0.1.9` is the first paid-ready build: the packaged app embeds the Quantizy
public license key and requires a valid offline license before running the
squeeze/export path. GitHub Sponsors is the near-term checkout path while
automatic Stripe worker fulfillment is being brought online.

For early access, use [GitHub Sponsors](https://github.com/sponsors/thom899g)
or contact the maintainer from the same account that will receive the license.
Automatic Stripe checkout opens only after the worker and public download checks
are green.

Launch copy, reviewer targets, and non-spam outreach templates live in
[OUTREACH.md](OUTREACH.md).

## 0.1.9 Delta

Quantizy now ships as a license-gated, signed, notarized paid beta build.
The route-group prefetch scoring from `0.1.8` remains included: on a crowded
Mac, Quantizy can warm the highest-value offloaded MoE subset instead of
skipping an oversized companion group.

## 0.1.10 Core Delta

The core repo now includes a manual buyer handoff command for the paid beta.
This turns a GitHub Sponsors payment or support reference into a complete
license handoff with license key, DMG link, checksum command, quickstart, and
support URL in one operator step.

```bash
moe-squeeze manual-buyer-handoff \
  --email buyer@example.com \
  --order-id sponsor_reference \
  --email-body
```

The public DMG remains `0.1.9` until a fresh signed/notarized package is cut.

## 0.1.11 Core Delta

The core fit matrix now has a deeper runtime pressure survival ladder:
`current`, `moderate`, `heavy`, `severe`, `critical`, and `emergency`.
That gives smaller or crowded Macs more fallback recipes when IDEs, browsers,
or other local-AI tools suddenly eat RAM after the first fit check.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.12 Core Delta

The core fit matrix can now be run in survival-priority mode:

```bash
moe-squeeze streaming-mlx-fit-matrix /path/to/model \
  --fit-priority survival \
  --survival-pressure-level critical_pressure
```

Quality priority still recommends the best recipe for the requested context.
Survival priority instead asks: "what recipe still runs if this Mac is under
real pressure from IDEs, browsers, and other local tools?" The report now keeps
those two promises separate, including whether the survival context is smaller
than the user originally requested.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.13 Core Delta

Survival priority now has an automatic pressure selector:

```bash
moe-squeeze streaming-mlx-fit-matrix /path/to/model \
  --fit-priority survival \
  --survival-pressure-level auto
```

Auto survival picks the harshest runtime-pressure level that still preserves
the requested context when possible. If the full request cannot fit, the report
falls back to the best reduced-context survival recipe and says so plainly.
This is aimed at the real buyer scenario: the Mac is already running IDEs,
browsers, and other local tools, so the recommendation must survive pressure
without pretending the original request still fits.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.14 Core Delta

The fit matrix now returns a compact recommendation frontier instead of only a
single winner. The frontier labels the best runnable choices by role:

- best quality at the requested context
- smallest memory footprint at the requested context
- maximum current-pressure context
- safest full-context pressure recipe
- maximum survival context

This makes the engine more useful for lesser-PC decisions: a user can choose
whether they want quality, headroom, or context length without manually reading
every fit row.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.15 Core Delta

Survival mode now promotes reduced-context recipes when the requested context is
too large. For example, if a user asks for a 1M-token setup that cannot fit,
Quantizy can still return a primary runnable survival recipe with:

- `ok: true`
- `requested_context_fits: false`
- `selected_context_tokens`
- `context_reduction_tokens`
- a ready export recipe for the reduced context

That turns an impossible request into an actionable "this is what will actually
run on this machine" answer instead of a dead-end failure.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.16 Core Delta

Reduced-context survival recommendations now include a structured explanation:

- requested context tokens
- selected runnable context tokens
- reduced-token count and percentage
- retained-context ratio
- selected pressure level
- runtime budget, total bytes, and headroom
- selected prefill chunk

This makes the smaller-machine answer easier to trust: Quantizy does not only
say "use fewer tokens," it says exactly how much was cut and which RAM-pressure
budget forced that decision.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.17 Core Delta

Validation receipts can now require a strong dynamic baseline:

```bash
moe-squeeze validation-receipt \
  --claim pareto.json:3.5 \
  --require-strong-baseline
```

This blocks a quality claim unless the selected evidence includes a strong
baseline family such as `unsloth_dynamic` or `gguf_dynamic`. Uniform-only wins
can still be useful internally, but they are no longer enough for a serious V1
quality claim when this gate is enabled.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.18 Core Delta

Validation receipts now summarize exactly which comparisons are claimable:

- `claimable_comparisons`
- `strong_claimable_comparisons`
- `claimable_count`
- `strong_claimable_count`

That separates internal wins from public-safe claims. A recipe that only beats
uniform can still be studied, while a recipe that also beats a strong dynamic
baseline is clearly marked as stronger launch evidence.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.19 Core Delta

The fit matrix now returns a `recommended_action` object for the app, CLI, and
buyer-facing UI. Instead of only showing technical rows, Quantizy can now say:

- whether the requested context fits
- whether it selected a reduced-context survival recipe
- exactly which context token count to use
- which runtime pressure level, KV settings, and prefill chunk were selected
- the ready recipe to export with
- short next steps a normal user can follow

This matters for smaller and crowded Macs. If the original request is too big,
Quantizy can now turn the result into a clear action such as “set context to
93,735 tokens and use this critical-pressure recipe,” rather than leaving the
buyer to interpret the matrix by hand.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.20 Core Delta

The MHA2MLA budget planner now returns a `recommended_action` receipt. This is
the DeepSeek-style KV-cache path: instead of only estimating whether a latent
attention adapter might help, Quantizy now summarizes the runnable action:

- selected adapter rank or adaptive rank plan
- whether the quality gates passed
- baseline KV bytes versus adapter KV bytes
- KV bytes saved and savings ratio
- export command hint
- the recipe to use after the adapter is exported
- next steps for quality validation before making a public claim

DeepSeek-V2/V3 made the same broad memory lesson hard to ignore: huge local
models are often blocked by KV cache, not only by weight size. This upgrade
makes Quantizy better at turning that insight into a concrete smaller-machine
adapter plan for compatible MHA/GQA-style models.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.21 Core Delta

Quantizy now has TurboQuant-style MHA2MLA cache codec planning. The new
`--mha2mla-cache-codec turbo` option estimates a scale-overhead-free latent KV
cache, separating it from ordinary affine quantized KV cache planning.

Why it matters: at very low KV bit-widths, per-group scale bytes can eat a large
part of the theoretical memory win. TurboQuant/PolarQuant-style cache designs
target that exact overhead. Quantizy now models that path in:

- KV-cache estimates
- MHA2MLA budget recommendations
- fit-matrix recipes
- CLI and API payloads
- recommended action receipts

In the current synthetic MHA2MLA cache test, the same 4-bit latent cache drops
from 26 bytes to 14 bytes because the turbo codec removes scale overhead. This
is planner support, not yet a claim that a finished MLX TurboQuant runtime
kernel ships in the public Mac app.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.22 Core Delta

Quantizy now auto-searches MHA2MLA latent-cache codecs. The
`--auto-mha2mla-cache-policy` path sweeps both ordinary `affine` and
scale-overhead-free `turbo` cache candidates, then only selects a candidate if
it survives the replay/error gates.

Why it matters: a normal buyer should not need to know which low-level cache
codec to try. Quantizy can now find the smaller valid plan itself and include it
in the CLI args, fit-matrix recipe, app API payload, and action receipt.

Current synthetic policy gates show the practical jump:

- low-rank 4-bit latent cache selected `turbo` and dropped from 28 bytes to 20
  bytes
- adaptive mixed-rank planning at 16-token selection context dropped from 90
  bytes to 38 bytes
- affine-only search remains available for conservative comparison

This is planner and receipt support, not yet a claim that a finished MLX
TurboQuant runtime kernel ships in the public Mac app.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.23 Core Delta

The auto MHA2MLA cache policy now emits a compact `selected_policy_delta`
receipt. That receipt follows the selected cache plan into the fit-matrix
recommended action and the MHA2MLA budget action.

Why it matters: buyers and reviewers should see the actual memory jump without
digging through nested policy candidates. For the current synthetic policy gate,
the receipt says the selected `turbo` plan uses 20 latent-KV bytes, saving:

- 12 bytes versus the unquantized latent cache
- 8 bytes versus the best passing affine-cache candidate
- 28.5714% versus that best affine candidate

This keeps the product honest: Quantizy can show exactly where the smaller-PC
win came from, which baseline it beat, and whether it was a planner win rather
than a finished runtime-kernel claim.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.24 Core Delta

MHA2MLA cache receipts are now projected to the final selected context. That
matters for crowded Macs: if survival mode lowers the context to stay inside
available RAM, the action receipt can show the cache bytes for the context the
user will actually run, not only the original policy sweep context.

For the current synthetic gate at 8 selected context tokens, the action receipt
now reports:

- selected `turbo` latent-KV cache: 20 bytes
- unquantized latent cache at the same context: 32 bytes
- standard KV cache at the same context: 128 bytes
- 108 bytes saved versus standard KV for that selected context

This is another evidence/decision upgrade for smaller PCs: Quantizy can tie the
memory win to the runnable recommendation instead of leaving users to infer it
from raw planner rows.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.25 Core Delta

Auto-KV recipe selection now emits an `auto_kv_recipe_delta` receipt. The
receipt compares the selected KV recipe against the baseline recipe with the
same fit request, then follows that comparison into the recommended action.

Why it matters: this is the exact buyer question for smaller machines: "what
changed, and how much bigger can I run?" In the verified 1,000,000-token
synthetic fit gate, auto-KV selected a tiered offload recipe and reported:

- requested KV bytes dropped from 32,000,000 to 8,539,904
- KV bytes saved versus baseline: 23,460,096
- KV saved ratio versus baseline: 73.3128%
- max-context gain versus baseline: 3,706,476 tokens
- runtime headroom gain at the selected pressure entry: 23,460,096 bytes

This does not claim better model quality. It makes the memory win auditable, so
Quantizy can show exactly why an auto-selected recipe fits a lesser PC better
than the default KV path.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.26 Core Delta

The auto-KV recipe delta now includes `changed_knobs`: a compact explanation of
which memory controls changed versus the baseline recipe.

In the same verified 1,000,000-token synthetic fit gate, Quantizy now reports
that the win came from switching to tiered offload and selecting:

- `kv_window_tokens=2048`
- `kv_sink_tokens=128`
- `kv_tail_offload=true`
- `kv_tail_bits=4`
- `kv_tail_quant_axis=kivi`

Why it matters: this makes the recommendation usable by normal buyers. They can
see not only that Quantizy saved memory, but which exact KV-cache strategy made
the million-token recipe fit on a smaller/crowded machine.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.27 Core Delta

Auto-KV recipe wins now carry an explicit proof boundary:

- `proof_status=memory_fit_proven_quality_unvalidated`
- `claim_scope=memory_fit_only`
- `quality_claim_safe=false`
- `quality_validation_required=true`

Why it matters: Quantizy can now safely say when a recipe makes a huge context
fit a smaller/crowded machine, while also saying that this is not a quality
claim until a perplexity or task-quality gate passes.

This protects the product from overclaiming while still making the memory-fit
win usable and auditable.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.28 Core Delta

Memory-fit recommendations now include a `quality_gate_command_hint` when
auto-KV creates the win. The hint gives users a ready starting command:

```bash
python -m moe_squeeze.cli quality-gate \
  --model /path/to/model \
  --mixed-targets 4 \
  --max-passages 12 \
  --json-out quantizy_quality_gate.json
```

Why it matters: Quantizy now closes the loop from "this huge context fits" to
"run this next before making a quality claim." That keeps the product useful
for smaller machines while preserving the honest validation bar.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.29 Core Delta

Auto-KV memory receipts now include buyer-readable multipliers:

- `kv_size_ratio_vs_baseline`
- `kv_gib_saved_vs_baseline`
- `total_size_ratio_vs_baseline`
- `total_gib_saved_vs_baseline`
- `max_context_multiplier_vs_baseline`

In the verified 1,000,000-token synthetic fit gate, the selected auto-KV recipe
now reports:

- KV cache size ratio versus baseline: 0.266872
- max-context multiplier versus baseline: 3.946352x
- total size ratio versus baseline: 0.763313

Why it matters: Quantizy can now explain the win in the language users care
about: how much smaller the KV path is and how much more context the same
machine can carry.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.30 Core Delta

Recommended fit actions now include a `memory_pressure_summary`. It reports:

- requested context tokens
- selected/runnable context tokens
- whether the requested context fits
- runtime pressure level
- runtime budget bytes
- runtime total bytes
- runtime headroom bytes/GiB/percent
- selected context ratio

Why it matters: this turns the fit result into a crowded-machine answer. A user
can see not only which recipe was selected, but whether it actually has
headroom under the chosen RAM pressure level.

In the verified survival gate, the summary reports a reduced-context
`critical_pressure` recipe with selected context ratio `0.093735`. In the
1,000,000-token auto-KV gate, it reports full requested-context fit with
`29.5376%` runtime headroom.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## 0.1.31 Core Delta

Recommended actions now include a `recovery_hint` when a requested context does
not fit. For an oversized request, the hint reports:

- `reason=requested_context_too_large`
- `action=lower_context_to_cap`
- the exact `context_cap_tokens`
- how many tokens must be reduced
- whether to try `auto_kv_recipe`
- whether to try survival priority

Why it matters: this turns a failed fit into a next action. Instead of only
saying "no," Quantizy can tell the user the safe context cap and point them
toward the stronger memory search paths that may recover the original request.

This is an engine-side upgrade in `quantizy-core`; the public DMG remains
`0.1.9` until the next packaged Mac build is cut.

## License

Quantizy is proprietary software. See [`LICENSE.md`](./LICENSE.md).
