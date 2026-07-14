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

## 0.1.256 Core Delta

Quantizy now carries a DeepSeek Sparse Attention-style phase-layout contract for
lookahead sparse KV. The runtime config records separate prefill and decode
cache/indexer peaks, and method selection rejects a candidate when either phase
exceeds the current effective RAM budget.

This prevents a local-PC false positive: a sparse-KV plan can look resident-fit
while the decode indexer cache or active prefetch set still pushes the machine
over budget.

## 0.1.255 Core Delta

Quantizy's `memory-method-readiness` CLI can now run the launch-aware guard
directly. Pass `--current-effective-runtime-budget-bytes` with the machine's
current effective RAM budget and the command will return
`free_ram_or_rebenchmark` when the validated method no longer has enough
headroom to launch.

This makes the local workflow enforceable outside the app: scripts and users
can check the same current-RAM launch condition before starting a large model.

## 0.1.254 Core Delta

Quantizy readiness checks can now include the current effective runtime budget.
When that budget is supplied, `memory_method_run_readiness` runs the launch
guard and returns `free_ram_or_rebenchmark` instead of `ready_to_run` if the
machine no longer has the measured safe headroom.

This makes the local-PC flow stricter: with browsers, IDEs, and other apps open,
"ready" now means the validated method can launch under current memory pressure,
not merely that old benchmark paperwork exists.

## 0.1.253 Core Delta

Quantizy now exposes a runtime launch-guard validator. A finalized memory-method
config can be checked against the machine's current effective runtime budget
before launching the model, returning an explicit authorization decision and
RAM shortfall.

This is the enforcement side of the previous launch guard: a method that was
safe during validation can be blocked later if IDEs, browsers, or other local
processes have consumed the headroom the proof depended on.

## 0.1.252 Core Delta

Quantizy now carries benchmark proof into the finalized runtime config as a
launch guard. The final config records the required effective runtime budget,
measured peak envelope, measured/estimated savings, latency ratio, and
repeat-run stability bounds that were proven by the activation receipt.

This turns validation into an enforceable runtime condition: the app can reject
launches when the current machine no longer has the measured safe headroom,
instead of trusting an old proof after memory pressure changes.

## 0.1.251 Core Delta

Quantizy now checks benchmark stability across repeated measurements before a
memory method can activate. When multiple runs are present, measured peak RAM
and decode speed must stay within configured stability bounds.

This targets crowded local PCs directly: a method that only fits once, or whose
latency swings under pressure, is not reliable enough to recommend. Quantizy now
records peak and latency stability ratios in the benchmark receipt and blocks
activation when repeat runs are unstable.

## 0.1.250 Core Delta

Quantizy now blocks memory-method activation when measured RAM savings collapse
versus the selector estimate. Benchmark receipts record the measured savings
realization ratio and require the measured win to reach the configured fraction
of the planned win before activation can pass.

This prevents another practical local-run failure: an advanced method may look
good in planning, but allocator overhead, runtime buffers, or hidden cache
state can erase most of the promised RAM benefit. Quantizy now checks the real
win before treating the method as usable.

## 0.1.249 Core Delta

Quantizy now gates HISA/MISA sparse-indexer activation on measured selector
latency. The benchmark receipt must include selector latency evidence, and the
selector work must fit the configured per-decode budget before the method can
pass.

This closes another small-PC failure mode: a sparse indexer can save memory but
still make local decode unusably slow. Quantizy now requires memory savings,
token-selection overlap, workspace fit, and selector latency fit together.

## 0.1.248 Core Delta

Quantizy now requires measured selector evidence before HISA/MISA-style sparse
indexers can activate. The benchmark receipt must prove top-k overlap against
the reference sparse selector and must show that measured selector workspace
does not exceed the planned HISA/MISA workspace budget.

This tightens the DeepSeek-style sparse-attention path: a clever indexer can no
longer pass just because memory and perplexity look acceptable. It also has to
prove it is selecting the same important tokens often enough to be trusted.

## 0.1.247 Core Delta

Quantizy now adds a HISA-style coarse-to-fine sparse-indexer path. For
DeepSeek-DSA-shaped long-context runs, the planner can propose block-pooled
coarse filtering followed by token-level refinement inside the selected blocks,
reducing selector workspace without changing the downstream sparse-attention
branch contract.

The HISA path is receipt-gated like the other advanced memory methods: the
quality gate must match the exact coarse block size, selected block count,
refinement token count, local window, branch signature, fit-gain receipt, and
baseline comparison evidence before activation.

## 0.1.246 Core Delta

Quantizy now requires per-passage quality evidence before sparse-KV methods can
claim a win over equal-budget uniform and dynamic baselines. Average-only
receipts are treated as insufficient evidence, and noisy wins are blocked when
the passage-level win rate is too low or any held-out passage regresses beyond
the configured quality/KL limits.

This reflects the current DeepSeek sparse-attention lesson: long-context
indexers and KV pruning are useful only when they survive repeated prompt-level
checks against strong dynamic baselines, not when one averaged benchmark looks
good.

## 0.1.245 Core Delta

Quantizy now audits transient prompt-prefill workspace inside memory-method
benchmark receipts. A method can fit steady-state decode KV and still crash
while ingesting a long prompt; the benchmark gate now records selected prefill
workspace bytes, available prefill workspace, and prefill chunk size, then
blocks activation when prefill workspace exceeds the measured budget.

This makes the long-context path safer on crowded local machines: chunked
prefill must actually bring the transient spike under budget before the method
is considered runtime-ready.

## 0.1.244 Core Delta

Quantizy now benchmarks FlashMemory-style lookahead sparse-KV with
method-specific evidence. The runtime benchmark receipt must include measured
query-critical recall and measured neural-indexer overhead for
`lookahead_neural_sparse_kv`, then checks those against the selected contract.

This closes the loop after planning: the method is rejected if it looks good in
the selector but the real run loses recall, exceeds indexer overhead, overruns
peak memory, or regresses latency beyond the configured limit.

## 0.1.243 Core Delta

Quantizy now treats FlashMemory-style lookahead sparse-KV as a first-class
memory-method candidate. The selector validates the lookahead receipt, adds the
measured neural-indexer overhead back into runtime peak, subtracts that overhead
from gross KV savings, and only authorizes the method when the net memory win
is still positive.

This is an important small-PC guardrail: a neural indexer can look impressive
while quietly costing more memory than it saves. Quantizy now ranks it only
when it improves the actual fit math.

## 0.1.242 Core Delta

Quantizy now has a receipt builder and strict activation check for
FlashMemory-style neural memory indexers. A lookahead sparse-KV runtime config
can no longer be enabled by a generic pass receipt when it carries a neural
indexer contract. The receipt must prove query-critical recall at or above the
contract and measured indexer overhead at or below the planned budget.

This matters for smaller PCs because the method only helps if the indexer saves
more KV memory than it costs. Quantizy now rejects the path when recall is too
low or the indexer state eats the memory win.

## 0.1.241 Core Delta

Quantizy now turns the newer DeepSeek/FlashMemory direction into a concrete
runtime contract instead of a loose research note. The sparse-KV planner emits
a backbone-free neural memory indexer contract for lookahead residency: chunk
size, predicted query count, embedding table bytes, query-encoder overhead,
required query-critical recall, target physical KV ratio, and the exact
validation requirement are all bound into the quality gate.

The MoE side also now emits a speculative expert-prefetch runtime config, not
just an advisory. It carries the selected estimator, source paper, overhead
bytes/class, small-PC overhead fit, required future hit rate, bandwidth overlap
status, and receipt contract. Both paths remain gated: they are executable
targets for validation, not unproven marketing claims.

## 0.1.240 Core Delta

Quantizy now binds speculative expert-prefetch receipts to the selected
estimator and its overhead budget. A receipt must echo the estimator identity,
source paper, runtime overhead bytes, overhead class, small-PC overhead fit,
hit rate, hidden bytes, remaining bytes, fallback mode, and bandwidth status
before activation is allowed. This prevents a receipt for a cheap predictor
from accidentally enabling a heavier learned estimator that would hurt the
memory win on a smaller machine.

## 0.1.239 Core Delta

Quantizy now makes speculative expert-prefetch estimator selection overhead
aware. Each predictor candidate carries estimated runtime overhead bytes,
overhead class, and a small-PC fit decision, so the planner can avoid selecting
a learned hidden-state estimator when its state would erase the memory win. In
those cases it prefers lower-overhead same-block trace rules or Fate-style
cross-layer gate evidence when the artifact trace supports them.

## 0.1.238 Core Delta

Quantizy now ranks speculative expert-prefetch estimator styles instead of
treating them as one generic research path. The advisory can choose between
same-block trace transition rules, Fate-style cross-layer gate prediction, and
hidden-state future-expert estimation based on actual saved route evidence,
observed hit rates, runtime support, and quality risk. This makes offloaded MoE
planning more actionable on lesser PCs: use the lowest-risk predictor the
artifact trace actually supports before asking for more RAM.

## 0.1.237 Core Delta

Quantizy now routes expert-offload recovery through speculative prefetch when
the trace proves there are blocking misses that a future-expert estimator could
hide. Instead of jumping straight from bandwidth failure to "add RAM / slower
decode / faster storage", the recovery ladder now places a receipt-gated
speculative-prefetch validation step first, while keeping the no-offload and
lower-TPS fallbacks behind it. This makes the lesser-PC MoE path more useful:
try proven overlap before requiring more resident experts.

## 0.1.236 Core Delta

Quantizy now makes speculative expert prefetch receipt-bound instead of leaving
it as a loose research hint. The new validator only authorizes the path when the
estimator receipt proves the required future-expert hit rate, exact-router
fallback, speculative slot count, hidden blocking bytes, remaining blocking
bytes, and bandwidth-overlap status. This keeps the MoE offload path usable on
smaller PCs without silently enabling an estimator that was not actually proven
on the buyer's artifact and trace.

## 0.1.235 Core Delta

Quantizy now adds a speculative expert-prefetch advisory for offloaded MoE
experts. When the trace-derived resident/prefetch plan still has blocking
offload misses, the planner estimates a receipt-gated future-expert estimator
path inspired by speculative MoE inference: hidden-state prediction,
async sidecar prefetch, exact-router fallback, required future-hit rate, and
bandwidth overlap pressure. This targets the real lesser-PC bottleneck for huge
MoE models: keeping most experts off RAM while hiding more of the offload cost.

## 0.1.234 Core Delta

Quantizy now adds a StreamIndex-style exact top-k advisory for DeepSeek
CSA/DSA sparse selectors. Instead of treating chunked indexer workspace as a
fixed cost, the planner can propose a streaming partition-merge selector that
never materializes the full indexer score tensor and binds partition size,
key-tile size, partition top-k, exact-topk mode, and peak bytes to the runtime
receipt. On the 524k-token DSA fixture this lowers selector peak from
`2,097,152` bytes to `1,148,160` bytes without reducing global top-k.

## 0.1.233 Core Delta

Quantizy now adds a SPIN-style locality-aware sparse-KV offload check. Lookahead
residency planning can account for page fetch amplification, bucketed-LRU hit
rate, and active-set metadata before accepting an offloaded sparse-KV path. This
keeps SSD/CPU offload honest on smaller PCs: a candidate that fits by raw bytes
can still be blocked if irregular page fetches miss the decode latency budget,
and runtime activation must match the exact page/LRU/metadata assumptions.

## 0.1.232 Core Delta

Quantizy now models a MISA-style routed indexer for DeepSeek Sparse Attention
paths. Instead of assuming every DSA query must scan the prefix with all 64
indexer heads, the planner can emit a gated `misa_routed_*` runtime config that
uses query-dependent head routing, block-pooled router statistics, and an exact
receipt match for active heads, router block size, router overhead, branch
signature, and fit-gain receipt. On the 524k-token DSA fixture this cuts the
estimated selector peak from `2,097,152` bytes to `1,310,720` bytes while keeping
the same sparse token path and requiring a separate quality gate.

## 0.1.231 Core Delta

Quantizy now imports the key DeepSeek-V4 CSA/HCA runtime guardrails into the
sparse-KV planner. Aggressive compressed sparse attention, heavily compressed
attention, and shared-KV hybrid paths now carry explicit stabilizer requirements:
query/KV RMSNorm, partial RoPE handling, local sliding-window fallback, attention
sink logits, mixed BF16/FP8 KV storage, and shared-KV MQA grouped projection
where applicable. The selector only authorizes those paths when the quality
receipt echoes the same stabilizer bundle, branch signature, fit-gain receipt,
and free-RAM proof.

## 0.1.230 Core Delta

Quantizy now adds a sparse-KV offload prefetch recovery ladder. When old KV
pages are planned for SSD sidecars, the lookahead residency report now ranks
what to do if decode would stall: keep more global sparse KV resident / lower
offload recall, lower the target decode TPS, or move sidecars to faster storage
/ reduce IO contention. This makes long-context offload planning practical on
crowded desktops instead of only saying that the plan is blocked.

## 0.1.229 Core Delta

Quantizy now models a DeepSeek-V4-style shared key/value sparse-KV profile.
The planner keeps the existing conservative c4/c128/local-window path, then
adds a gated `shared_kv_hybrid_c4_c128_sparse_indexed_kv` advisory for runtimes
that can share key/value cache state with the required inverse-RoPE handling.
On the 524k-token planner fixture, this lowers estimated KV retention from
12.89% to 6.45% and raises same-KV logical context capacity to about 15.5x.
It remains runtime-support and quality-gate required.

## 0.1.228 Core Delta

Quantizy now emits a ranked MoE offload recovery ladder. If cold-expert
streaming is unsafe on a crowded local machine, the artifact report orders the
next actions: no-offload RAM fallback, safer resident candidate, lower decode
TPS, or faster storage / reduced expert streaming. This builds on the same
lesson from DeepSeek's newer long-context work: compression is only useful when
the runtime also accounts for bandwidth, locality, and recoverable failure
paths.

## 0.1.227 Core Delta

Quantizy now quantifies the RAM cost of the no-offload MoE fallback. When expert
offload is bandwidth-unsafe, the safe fallback config reports the extra resident
bytes/GiB required, whether all experts fit the current runtime budget, and the
next fallback action. That makes the recovery path actionable: free/add RAM for
the no-offload route, or keep pursuing faster storage/lower decode TPS.

## 0.1.226 Core Delta

Quantizy now emits a safe no-offload fallback config when MoE expert offload is
bandwidth-unsafe. If the primary runtime config is rejected because cold-expert
streaming would stall decode, the report includes a `safe_fallback_config` that
keeps all experts resident and disables expert offload. It may require more RAM,
but it gives runners a deterministic safe alternative instead of only a failure.

## 0.1.225 Core Delta

Quantizy now validates MoE expert offload runtime configs before execution.
`validate_expert_offload_runtime_config` checks schema, resident/offload expert
counts, resident/offload overlap, unsafe pressure states, and whether the runtime
receipt contract still matches the config. This prevents stale or manually edited
offload configs from being run as if they were current planner output.

## 0.1.224 Core Delta

Quantizy now emits a runtime-facing MoE expert offload config. Artifact
inspection returns `expert_offload_runtime_config` with resident expert IDs,
offloaded expert IDs, prefetch slots, target decode TPS, bandwidth budgets,
planned stream/blocking bytes, and the matching runtime receipt contract. This
gives local runners a concrete execution contract instead of forcing them to
reconstruct the plan from diagnostic fields.

## 0.1.223 Core Delta

Quantizy now emits and validates a runtime receipt contract for MoE expert
offload. The artifact report includes the required receipt fields, planned
streamed bytes, blocking miss bytes, target decode TPS, and pass conditions; the
new validator rejects runs whose observed expert stream exceeds the plan or
misses the target speed. This keeps the smaller-PC claim evidence-based: a plan
is not treated as proven until the local runtime returns a matching receipt.

## 0.1.222 Core Delta

Quantizy now turns MoE expert offload pressure into a concrete action. Artifact
inspection emits an `expert_offload_pressure_action` that says whether to run
the current resident/prefetch plan, measure local IO, switch to a safer resident
candidate, lower decode TPS, use faster storage, or add RAM. This closes the
loop from "expert offload is bandwidth-bound" to a direct smaller-PC recovery
step instead of leaving the user with raw bandwidth numbers.

## 0.1.221 Core Delta

Quantizy artifact inspection can now use measured local IO bandwidth for MoE
expert prefetch pressure. Pass a JSON bandwidth receipt from the existing local
probe flow, or enable path-classified automatic bandwidth, and the resident-set
and prefetch planners use that measured/estimated disk speed instead of a manual
guess. This makes cold-expert offload decisions more machine-specific on lesser
PCs, especially when models live on external SSDs or crowded local storage.

## 0.1.220 Core Delta

Quantizy now feeds expert prefetch pressure back into automatic resident-set
selection. Each candidate resident hit-rate plan carries the resulting streamed
expert IO, blocking miss bytes, required bandwidth, and pressure status, so the
auto selector can prefer bandwidth-safe resident sets instead of choosing from
RAM fit and routing hit-rate alone. This makes the MoE offload planner more
useful on machines where the bottleneck is not just memory, but whether local
storage can feed cold experts quickly enough.

## 0.1.219 Core Delta

Quantizy now separates total streamed MoE expert IO from blocking expert misses.
The planner no longer treats "not prefetched" as "not loaded"; every offloaded
expert demand is counted against stream bandwidth, while prefetch hit-rate only
decides how much of that IO can be overlapped before decode needs the expert.
This makes expert paging estimates more honest for smaller PCs: a plan can be
memory-feasible but still too slow if the unhidden offload stream overwhelms
host or SSD bandwidth.

## 0.1.218 Core Delta

Quantizy now makes automatic MoE expert prefetch slot selection bandwidth-aware.
When several trace-derived prefetch plans are possible, it no longer picks the
highest hit-rate plan blindly; it prefers candidates that fit the lazy-cache byte
budget and stay inside the configured host/SSD bandwidth reserve. On crowded
Macs, this avoids warming too many offloaded experts when the extra prefetching
would burn the IO headroom needed for decode, swap, and model reads.

## 0.1.217 Core Delta

Quantizy now checks MoE expert prefetch bandwidth pressure. For resident/offload
artifacts with saved routing traces, it estimates streamed expert bytes per
decoded token, required GiB/s at a target decode rate, and whether the configured
host/SSD prefetch budget passes, burns reserve headroom, or fails. This moves
the DeepSeek/FluxMoE-style expert-paging idea into a practical smaller-PC guard:
offloaded experts are only useful if the machine can actually feed them fast
enough.

## 0.1.216 Core Delta

Quantizy now runs an offload pressure preflight for KV sidecar plans. It checks
the recommended SSD reserve against declared or probed free disk plus swap/OS
headroom, warning when an offloaded long-context recipe would technically fit
but leave too little room for macOS swap, model reads, and filesystem overhead.

## 0.1.215 Core Delta

Quantizy now adds a retrieval-aware KV budget policy inspired by CompressKV and
InfoKV. Fit planning can attach a runtime contract that gives richer sparse/KV
metadata to semantic-retrieval heads and later retrieval-heavy layers while
keeping the rest of the cache cheaper, with a required quality gate before any
activation claim.

## 0.1.214 Core Delta

Fit planning now has an explicit DeepSeek-inspired long-context preset. It
combines sparse/latent/tiered KV recipe search, measured offload bandwidth,
conservative IO contention, long-context ladders up to 1M tokens, and runtime
support warnings so Quantizy can plan more realistically for DeepSeek V3.2/V4
style local long-context workloads.

## 0.1.213 Core Delta

Fit-matrix planning can now run a small sparse-KV bandwidth probe automatically.
When live-RAM planning is used, the local app measures a bounded sample from the
model drive, uses that measured MiB/s value for lookahead sparse-KV offload
decisions, and binds the resulting receipt into runtime activation.

## 0.1.212 Core Delta

Quantizy can now generate measured sparse-KV bandwidth receipts. The new
`sparse-kv-bandwidth-probe` command reads a bounded sample from the target model
drive, writes a JSON bandwidth receipt, and feeds the measured MiB/s value back
into lookahead sparse-KV offload planning.

## 0.1.211 Core Delta

Sparse-KV offload planning can now use measured IO bandwidth receipts. Instead
of relying only on path heuristics, Quantizy can consume a local disk probe JSON
receipt, plan prefetch budgets from the measured MiB/s value, and bind that
measurement into the runtime activation receipt.

## 0.1.210 Core Delta

Lookahead sparse-KV offload planning now reserves IO headroom. When live-RAM
planning is used, Quantizy leaves bandwidth for OS, model loading, swap, and
other disk activity before accepting an offloaded KV residency plan, so external
drives are handled more conservatively.

## 0.1.209 Core Delta

Lookahead sparse-KV offload planning now auto-classifies model storage speed.
When live-RAM planning is used, Quantizy treats models under `/Volumes` as
external/slower storage unless a measured bandwidth is supplied, then binds that
bandwidth assumption into the residency quality-gate receipt.

## 0.1.208 Core Delta

Current-RAM planning now has automatic crowded-desktop pressure reserves. When
the local app uses live available RAM, Quantizy reserves extra headroom before
recommending sparse-KV and model-fit plans, which better reflects Macs already
running IDEs, browsers, and other memory-heavy tools.

## 0.1.207 Core Delta

Crowded-desktop pressure reserve controls are now exposed through the CLI and
local app API. Fit-matrix runs can reserve an extra fraction or minimum GiB of
current RAM before recommending model, KV, and sparse attention plans.

## 0.1.206 Core Delta

Current-RAM fit planning can now reserve extra pressure headroom for crowded
desktops. When IDEs or other apps are already open, Quantizy can reduce the
effective runtime budget before recommending sparse/KV plans, lowering the risk
of swap, stalls, or crashes.

## 0.1.205 Core Delta

Sparse indexer buffers can now be page-aligned in the fit planner. Quantizy can
charge allocator slack for selector score buffers and indexer caches, which
makes tight-RAM recommendations more realistic on smaller machines.

## 0.1.204 Core Delta

Sparse-KV fit planning can now charge a separate indexer-cache overhead. This
matters for DeepSeek-style sparse attention paths where the lightning/indexer
module may need its own cached keys or metadata, so Quantizy no longer has to
treat selector score buffers as the only extra memory cost.

## 0.1.203 Core Delta

Memory-method selection now ranks sparse-KV candidates by baseline evidence
strength. A barely passing sparse result stays high risk, while a repeated win
with larger perplexity and KL margins can compete more strongly with safer
latent-KV paths.

## 0.1.202 Core Delta

Sparse-KV activation is now bound to the strong baseline evidence contract.
Quantizy will not enable or claim a sparse/mixed KV quality win unless the
quality receipt includes a passing equal-budget comparison against uniform and
dynamic-GGUF-style baselines.

## 0.1.201 Core Delta

Sparse-KV quality claims are now gated against strong equal-budget baselines.
Quantizy only allows a mixed sparse-KV win claim when repeated passages beat
both uniform and dynamic-GGUF-style baselines on perplexity and KL divergence.

## 0.1.200 Core Delta

Sparse-KV residency can now honor a minimum decode-speed target. Quantizy turns
the requested tokens/second into a stricter fetch-latency budget and rejects
offloaded layouts that would fit RAM but miss the speed target.

## 0.1.199 Core Delta

Runtime activation is now bound to the sparse-KV residency tradeoff receipt.
Quantizy rejects activation if the receipt changes the selected resident ratio
or hides a different RAM-vs-prefetch decision than the planner made.

## 0.1.198 Core Delta

Sparse-KV residency plans now include a machine-readable tradeoff receipt.
Quantizy records which resident ratios were rejected by RAM, which were blocked
by prefetch bandwidth, and why the selected ratio was chosen.

## 0.1.197 Core Delta

The sparse-KV residency ladder now includes a 90% resident fallback. When RAM
allows but offload bandwidth is weak, Quantizy can spend more RAM to reduce
fetch traffic instead of rejecting the runtime path outright.

## 0.1.196 Core Delta

The lookahead residency planner now has a safer bandwidth-pressure ladder.
Quantizy can choose a more resident, lower-risk sparse-KV layout when offload
bandwidth is tight instead of failing or over-offloading by default.

## 0.1.195 Core Delta

Lookahead residency activation is now bound to the exact prefetch assumptions
validated by the quality receipt. Quantizy rejects runtime activation if the
receipt changes bandwidth, latency, recall, allocator slack, or prefetch bytes.

## 0.1.194 Core Delta

Lookahead sparse-KV residency is now bandwidth-aware. Quantizy blocks residency
candidates that fit RAM but would exceed the configured offload prefetch budget,
so slow disk or CPU-memory paths do not get treated as usable runtime wins.

## 0.1.193 Core Delta

Sparse-KV residency estimates are now allocator-aware. Quantizy charges page
slack and per-entry rounding overhead into the resident peak before deciding
whether a DeepSeek-style mixed cache layout actually fits current RAM.

## 0.1.192 Core Delta

Quantizy now plans DeepSeek V4-style bucketed cache packing for mixed sparse-KV
layouts. The fit report records shared logical-token blocks, page-bucket use,
and estimated pool-count reduction before enabling c4/c128/SWA residency paths.

## 0.1.191 Core Delta

Lookahead sparse-KV residency now has receipt-bound runtime validation.
Quantizy rejects mismatched, missing, or failed residency receipts before
enabling the offload/residency runtime path.

## 0.1.190 Core Delta

Lookahead residency now has an executable quality-gate plan. Quantizy emits the
runtime config, implementation checklist, and validation command needed to test
the sparse-KV residency/offload path before any runtime adoption.

## 0.1.189 Core Delta

Quantizy now plans FlashMemory-style lookahead residency for sparse KV. In tight
RAM cases it estimates how much global sparse KV can be offloaded while keeping
query-critical chunks resident, and reports whether that would remove the free
RAM requirement.

## 0.1.188 Core Delta

Selector relief is now MISA-aware. When selector workspace is too large, Quantizy
searches not just smaller chunks and lower fine top-k, but also routed active
indexer-head counts, then emits a quality-gate plan for the safest fitting
candidate.

## 0.1.187 Core Delta

Explicit sparse local windows now degrade gracefully under RAM pressure.
Quantizy tries the requested recency window first, then falls back to smaller
windows only when needed, recording the fallback in the runtime config and
quality-gate receipt.

## 0.1.186 Core Delta

Inspired by recent sparse-indexer research, DSA selector heads are now
budget-aware. Quantizy keeps all 64 indexer heads when memory allows, but can
route down to 32, 16, or 8 active heads under tight RAM budgets so the sparse-KV
path can still fit on smaller machines.

## 0.1.185 Core Delta

DSA coarse compression is now budget-aware too. Quantizy preserves selected-fine
fanout first, then chooses the least aggressive coarse compression ratio that
fits the active RAM budget before escalating to harsher sparse compression.

## 0.1.184 Core Delta

DSA selected-fine blocks are now budget-aware. Quantizy keeps the full 2048
fine-block fanout when memory allows, but can step down to the largest fitting
fanout before falling back to harsher sparse compression.

## 0.1.183 Core Delta

DSA sparse-KV activation is now receipt-bound to the exact branch identity.
Quantizy records branch mode and branch weights in the runtime config and rejects
quality receipts that try to activate a different sparse-attention structure.

## 0.1.182 Core Delta

Quantizy now models a DeepSeek-V3.2/NSA-style sparse attention path for huge
contexts: compressed coarse memory, selected fine blocks, and a local sliding
window. In constrained RAM cases it can choose this DSA middle path before
falling to more extreme compression, with quality-gate receipts still required.

## 0.1.181 Core Delta

Memory-method readiness is now exposed through the CLI. The app and scripts can
load selector, benchmark, activation, and finalization JSON receipts, then get a
machine-readable next action or a ready-to-run verdict before enabling runtime
memory methods.

## 0.1.180 Core Delta

Memory-method readiness now includes actionable next-step payloads. The app can
show a concrete benchmark, remediation, activation, finalization, or ready-to-run
payload instead of inferring instructions from a status string.

## 0.1.179 Core Delta

Memory-method runs now have a readiness summary. Quantizy reports the next
missing evidence step before runtime activation, such as planning a method,
running a benchmark, fixing a failed benchmark receipt, building activation, or
finalizing the runtime config.

## 0.1.178 Core Delta

Authorized memory-method activation can now finalize a runtime config. The
runtime config is only marked enabled and validated when an activation receipt
passes, and it carries the measured peak, measured savings, latency ratio, and
embedded activation receipt for auditability.

## 0.1.177 Core Delta

Runtime memory-method activation is now gated by benchmark evidence. Quantizy
creates an activation receipt only when the selector receipt and measured
benchmark receipt both pass for the same method/profile, preventing validated
planning from being mistaken for validated runtime activation.

## 0.1.176 Core Delta

Quantizy now has runtime benchmark receipts for selected memory methods. A
selector decision can be checked against measured peak memory, baseline peak
memory, latency ratio, runtime budget, and selector-estimated peak before the
fit claim is treated as supported.

## 0.1.175 Core Delta

Memory-method selection now emits an auditable selection receipt. Each run
records the selected method, gate order, authorized and blocked methods,
minimum-relief target, runtime budget, and selected metrics so a buyer-facing
fit decision can be explained after the fact.

## 0.1.174 Core Delta

The memory-method selector is now quality-aware. It can take a minimum memory
relief target, blocks methods that do not clear the required gap, and otherwise
prefers the lower-risk validated method instead of chasing maximum compression
when a safer method already solves the fit problem.

## 0.1.173 Core Delta

The unified memory-method selector is now peak-aware. It reports each
candidate's estimated runtime peak and headroom, and blocks an otherwise valid
method when its peak exceeds the active runtime budget, falling back to a lower
peak authorized method instead.

## 0.1.172 Core Delta

Quantizy now has a unified memory-method selector. Sparse indexed KV and
MHA-to-MLA latent KV are ranked together, but only receipt-authorized methods
can be selected; missing or mismatched receipts stay visible as blocked
candidates instead of being silently activated.

## 0.1.171 Core Delta

MHA-to-MLA latent-cache policies now have activation receipts. Quantizy can
convert a passing latent-cache sweep into a receipt only when the selected
policy meets the retained-KV target and reconstruction-error threshold; tampered
or under-target receipts are rejected.

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

## 0.1.170 Core Delta

Sparse KV preflight can now build the activation receipt after RAM recovery is
proved. A valid preflight resource receipt is converted into the exact sparse
quality receipt required by activation; under-target RAM receipts raise instead
of producing a misleading runtime receipt.

## 0.1.169 Core Delta

Sparse KV preflight now has a resource-receipt validator. A runner can check a
free-RAM receipt against the RAM recovery plan before launching the sparse
quality gate, proving both freed bytes and post-free effective budget meet the
activation thresholds.

## 0.1.168 Core Delta

Sparse KV resource guards now include a RAM recovery plan. The guard tells the
app exactly how many bytes must be freed, the post-free effective budget that
must be reached, the receipt thresholds required for activation, and the resume
action after a valid free-RAM receipt exists.

## 0.1.167 Core Delta

Sparse KV preflight now includes a resource guard. The app gets sparse peak
bytes, required post-free effective budget, current effective budget, launch
deficit, block reason, and a launch-allowed flag, so memory-tight runs can be
blocked before the machine thrashes.

## 0.1.166 Core Delta

Sparse KV planning now emits a preflight action for the app. It combines the
run-readiness verdict, free-RAM target, quality-gate command, receipt contract,
and runtime config into one object so the app can tell the user exactly whether
to run the quality gate now, free RAM first, measure RAM, or hold the sparse
runtime path.

## 0.1.165 Core Delta

Sparse KV planning now emits a run-readiness verdict. The planner classifies
whether a selected sparse profile is ready for a quality gate, needs RAM freed
first, needs selector relief, needs a measured runtime budget, or should remain
research-only, using the physical fit tier, selector pressure, and fit-gain
receipt.

## 0.1.164 Core Delta

Sparse KV activation now validates the fit-gain receipt itself. Quality receipts
must match the selected profile's memory-gain envelope, so stale or inflated
claims about saved peak memory or context multiplier are rejected before sparse
runtime activation.

## 0.1.163 Core Delta

Sparse KV planning now emits a fit-gain receipt for the selected profile. It
reports same-context KV saved, net peak memory saved after selector overhead,
context multiplier at the same KV budget, extra logical context tokens, and a
fit-gain tier, so larger-model claims can be tied to explicit memory evidence.

## 0.1.162 Core Delta

Sparse KV activation now binds quality receipts to the exact branch layout. A
receipt must match the selected DeepSeek-style branch signature, including
global/coarse/local branch roles, compression ratios, indexed-branch flags, and
compressed-token counts, before activation can proceed.

## 0.1.161 Core Delta

Sparse KV planning now exposes branch-aware physical memory components for
DeepSeek-style hybrid attention. The planner separates global retrieval,
heavily compressed global memory, and local recency branches, and sizes selector
workspace from indexed branches only. This lowers false peak estimates for
local-window profiles and gives future runtime code concrete per-branch budgets.

## 0.1.160 Core Delta

Sparse KV receipt building now validates measured overrides against the receipt
contract by default. Under-reported freed RAM, under-target post-free effective
budget, or a non-passing status raises before a malformed receipt can reach
selector activation. `strict=False` remains available for negative validation
tests.

## 0.1.159 Core Delta

Quantizy now exposes `build_sparse_kv_quality_receipt`, a helper that turns a
sparse KV quality-gate command hint into the exact receipt shape required for
activation. This makes the sparse path executable end-to-end: command hint,
receipt contract, generated receipt, selector validation, and target adoption
now use one shared contract.

## 0.1.158 Core Delta

Sparse KV quality-gate command hints now include a receipt contract. The hint
spells out required status, selector parameters, freed-RAM proof, post-free
effective-budget proof, and the receipt path so the generated quality receipt
can be validated without ambiguity.

## 0.1.157 Core Delta

Sparse adoption diagnostics now include post-free effective-budget fields:
required effective budget, receipt effective budget, and whether the effective
budget precondition passed. Activation holds can now explain whether the user
failed to free enough RAM or whether the measured post-free budget still does
not fit the sparse KV physical peak.

## 0.1.156 Core Delta

Sparse free-RAM receipts now validate the post-free effective runtime budget,
not just reported freed bytes. A receipt must show enough effective budget to
cover the selected sparse KV physical peak plus margin before activation can
proceed, closing the gap between "I freed RAM" and "the run now fits."

## 0.1.155 Core Delta

Sparse target adoption checks now surface free-RAM receipt diagnostics directly.
When activation is held because required RAM was not freed, the adoption result
and adoption receipt include required freed bytes, receipt freed bytes, and the
`free_ram_precondition_met` flag. This keeps UI and runner decisions explainable
at the activation layer.

## 0.1.154 Core Delta

Sparse activation validation now enforces free-RAM preconditions. If a sparse KV
runtime config requires freeing RAM, a passing quality receipt is not enough:
the receipt must report enough freed bytes before activation is authorized. This
prevents a memory-tight sparse path from being enabled on stale or incomplete
runtime evidence.

## 0.1.153 Core Delta

Sparse KV policy output now includes a recommended quality-gate command hint
with runtime action and free-RAM preconditions. Method-stack consumers also see
the sparse command directly, so a UI can prevent launching sparse validation
until the required RAM has been freed.

## 0.1.152 Core Delta

Sparse KV policies now promote the selected free-RAM target and runtime action
to top-level fields, and mirror the free-RAM target into the runtime config.
Apps and CLIs can now show the exact next step (`run_sparse_kv_after_quality_gate`
or `free_ram_before_sparse_kv_run`) without digging through nested fit details.

## 0.1.151 Core Delta

Sparse KV physical-fit checks now emit exact free-RAM targets when a candidate
does not fit the effective current budget. If all sparse candidates are over
budget, selection prefers the candidate requiring the least additional RAM to
free, giving the user a concrete "close apps/free X MiB" path instead of a dead
end.

## 0.1.150 Core Delta

Sparse indexed KV selection is now current-RAM-aware. When the fit report comes
from `--use-current-ram`, the policy keeps the nominal runtime budget but ranks
candidates against an `effective_runtime_budget_bytes` derived from current
available memory minus reserve. This prevents optimistic sparse KV choices when
IDEs, browsers, or other local tools have already consumed RAM.

## 0.1.149 Core Delta

Sparse indexed KV selection now uses physical fit tiers when ranking candidates.
Under tight runtime budgets, candidates that close the theoretical memory gap
but exceed the physical KV plus selector peak budget lose to lower-footprint
profiles with headroom. This turns the 0.1.148 fit object into an actual
selection signal for lesser-PC runs.

## 0.1.148 Core Delta

Sparse indexed KV candidates now expose a `physical_kv_fit` decision with
runtime budget, physical KV bytes, selector peak bytes, combined peak, headroom,
budget ratio, and fit tier. This moves the DeepSeek-style sparse KV path closer
to real lesser-PC selection: the advisor can rank candidates by whether they fit
the current memory budget, not just by compression ratio.

## 0.1.147 Core Delta

Sparse indexed KV runtime configs now carry physical KV footprint estimates:
requested logical context, requested KV bytes, estimated physical KV bytes,
bytes removed, per-token KV cost, and same-budget context expansion. This makes
DeepSeek-style sparse memory plans usable by fit advisors and UIs in actual RAM
numbers instead of abstract ratios only.

## 0.1.146 Core Delta

Sparse target adoption holds now surface exact receipt/config drift at the
adoption layer. If a quality receipt says pass but carries stale selector
parameters, callers get the required value and the receipt value without
digging through nested selector diagnostics.

## 0.1.145 Core Delta

Sparse target adoption validation now has explicit mismatch coverage for
receipts that pass status but do not match the target runtime config. The
validator holds activation and reports both the failed sparse quality-gate
precondition and the receipt/config mismatch.

## 0.1.144 Core Delta

Sparse target adoption validation now emits an adoption receipt and stamps it
onto the activation config when authorized. This gives paper-target sparse KV a
runtime audit trail showing standard KV status, user opt-in, selector receipt
authorization, missing preconditions, and the final activation action.

## 0.1.143 Core Delta

Quantizy now exposes a sparse target adoption validator. Runners can ask one
decision function whether paper-target sparse KV may activate, and it checks
standard KV failure/rejection, explicit user opt-in, and the matching sparse
selector quality receipt before returning an activation config.

## 0.1.142 Core Delta

Sparse paper-target adoption order now includes enforceable preconditions. For
very-high-risk target paths, runners must see standard KV failure or rejection,
explicit paper-target selection, a passing sparse target receipt, and receipt
parameters matching the target runtime config before activation.

## 0.1.141 Core Delta

Fit Matrix now emits a sparse paper-target adoption-order recommendation. When
the best target action is very-high implementation risk, the run policy keeps it
behind standard KV failure and an explicit sparse quality gate instead of
promoting it as the default first path.

## 0.1.140 Core Delta

Fit Matrix run-policy cost summaries now expose the best sparse paper-target
action directly. The summary includes the method rank, current sparse method,
priority score, impact fields, and target runtime config so UI/CLI surfaces can
show the strongest optional sparse upgrade without traversing the full method
stack.

## 0.1.139 Core Delta

The paper-target sparse KV next action now includes a deterministic priority
score. The score rewards estimated fit impact and discounts implementation risk
plus the required quality gate, giving downstream runners a stable way to rank
the aggressive sparse path against other recovery options.

## 0.1.138 Core Delta

The Fit Matrix paper-target next action now carries the sparse fit-impact tier,
same-budget context multiplier, extra context estimate, and same-context extra
weight/RAM budget. Runners no longer need to inspect nested advisory metadata
to prioritize whether the aggressive sparse path is worth validating.

## 0.1.137 Core Delta

Paper-target sparse KV advisories now classify estimated fit impact as small,
moderate, material, or major headroom. The tier considers both same-budget
context gain and same-context KV GiB saved, giving the UI a concise signal for
whether the aggressive sparse path is likely worth validating.

## 0.1.136 Core Delta

Paper-target sparse KV advisories now estimate same-context KV bytes saved and
the equivalent extra weight/RAM budget. Fit Matrix can now explain the tradeoff
as either more context at the same KV budget or more model payload room at the
same context.

## 0.1.135 Core Delta

Paper-target sparse KV advisories now estimate same-budget context gain. Fit
Matrix can report how much extra context the DeepSeek/FlashMemory-style target
path may support compared with the conservative sparse recommendation, while
still marking it as an estimate that requires quality validation.

## 0.1.134 Core Delta

Sparse indexed KV runtime config generation now uses one shared builder for the
conservative and paper-target paths. This keeps profile, selector, local-window,
paper-target, quality-gate, and activation metadata consistent as new sparse
runtime modes are added.

## 0.1.133 Core Delta

The paper-target sparse KV runtime config now carries its own quality-gate
contract: receipt path, required status, implementation parameters, and selector
activation rule. This makes the aggressive DeepSeek/FlashMemory-style path
self-contained and verifiable instead of just selectable.

## 0.1.132 Core Delta

The paper-target sparse KV next action now carries a ready runtime config for
the target profile. A runner or UI can switch from conservative sparse planning
to the DeepSeek/FlashMemory-style target path without reconstructing planner
state, while activation remains disabled until the sparse quality gate passes.

## 0.1.131 Core Delta

Quantizy now promotes the sparse KV paper-target advisory into the Fit Matrix
method stack as a concrete next action. Instead of hiding the aggressive
DeepSeek/FlashMemory-style option inside policy metadata, downstream UI/CLI
surfaces `rerun_sparse_kv_with_paper_target_mode` with the expected target
profile, retained-ratio reduction, implementation risk, and quality-gate flag.

## 0.1.130 Core Delta

Quantizy now emits a paper-target advisory when conservative sparse KV planning
fits memory but leaves a substantially larger physical KV footprint than an
available DeepSeek/FlashMemory-aligned hybrid plan. The advisory reports the
target profile, retained-ratio savings, implementation risk, quality-gate
requirement, and the exact `sparse_kv_target_mode: paper_target` hint.

## 0.1.129 Core Delta

Quantizy sparse indexed KV planning now has an explicit `paper_target` mode.
The default remains conservative, preferring the least risky memory-closing
profile. When a user needs maximum long-context compression, target mode can
prefer a DeepSeek/FlashMemory-aligned hybrid plan that reaches the reported
physical-KV target zone, while still labeling it high risk and requiring a
quality gate before runtime activation.

## 0.1.128 Core Delta

Quantizy now labels sparse indexed KV plans against the new DeepSeek-style
long-context target zone instead of treating every memory reduction equally.
Planner output reports whether a candidate is above, near, or inside the
reported 10%-13.5% physical KV range, and the runtime config carries that label
forward.

This turns the DeepSeek/FlashMemory insight into a measurable local planning
check while keeping activation behind Quantizy's own quality gate.

## 0.1.127 Core Delta

Quantizy can now validate the recommended sparse indexed KV runtime config
directly. The validator converts `quantizy.runtime.sparse_indexed_kv_config.v1`
into the selector activation schema internally, so runners can pass the
planner-emitted runtime config plus a receipt and get the same activation action.

This removes another handoff gap between sparse planning and sparse runtime
activation.

## 0.1.126 Core Delta

Quantizy now emits a runtime-facing sparse indexed KV config for the recommended
sparse profile, not only for selector-relief cases. The config captures the
profile, hybrid attention mix, indexer settings, local-window setting, recency
risk, and quality-gate requirement, and it is surfaced through the sparse method
stack entry.

This moves sparse KV planning closer to executable prototype handoff while still
keeping activation disabled until validation passes.

## 0.1.125 Core Delta

Quantizy sparse selector activation checks now return an `activation_action`.
The runner gets a direct decision: enable the sparse selector runtime, rerun the
sparse selector quality gate, or reject the runtime config because it drifted
from the validated selector/window/risk settings.

This turns sparse activation validation from a passive report into an executable
runtime decision for smaller-PC long-context experiments.

## 0.1.124 Core Delta

Quantizy sparse selector activation checks now separate missing receipt fields
from mismatched fields. Failed activation reports `missing_parameters` for absent
required values and `parameter_mismatches` only for present-but-wrong values.

This makes sparse runtime failures actionable: the runner can tell whether the
quality receipt is incomplete or whether it was produced for the wrong selector,
local-window, or recency-risk setting.

## 0.1.123 Core Delta

Quantizy now binds sparse selector activation to the selected local-recency risk
tier as well as the local-window size. Runtime configs and validation receipts
must agree on `local_recency_quality_risk` before sparse selector activation is
authorized.

This prevents a pass from one sparse recency-quality profile from authorizing a
different risk profile during runtime activation.

## 0.1.122 Core Delta

Quantizy now binds sparse selector activation to the selected local-window size.
Runtime configs include `local_window_tokens`, implementation checklists require
it, and validation receipts must match it before sparse selector activation is
authorized.

This closes a quality-safety gap: a quality gate for one recency window can no
longer accidentally authorize a different sparse local-window tradeoff.

## 0.1.121 Core Delta

Quantizy now uses sparse local-recency quality risk during sparse KV selection.
When candidates fit, the planner ranks recency risk explicitly before maximizing
retained KV ratio, so a memory fit that preserves a safer local window is
preferred over a more brittle recency tradeoff.

This moves the sparse planner from pure memory fitting toward quality-aware
memory fitting, which is required before these methods can be trusted on smaller
PCs.

## 0.1.120 Core Delta

Quantizy now labels the sparse local-window tradeoff with explicit recency
quality-risk metadata. Each sparse profile reports the retained local-window
ratio and a `local_recency_quality_risk` tier, and the recommended plan promotes
those fields to the top-level sparse policy.

This keeps the planner honest: shrinking the recency window can make a huge
context fit, but the quality risk is now visible instead of hidden inside the
memory estimate.

## 0.1.119 Core Delta

Quantizy now searches sparse local-window candidates when the runtime does not
provide an explicit window. The planner evaluates 16k, 8k, 4k, 2k, and 1k local
windows, then picks the largest recency window that still fits within the
least-risky sparse profile.

This improves the lesser-PC path because sparse KV no longer hardcodes one
recency/fit tradeoff. It can keep more local context when there is headroom and
shrink the local window when that is what makes a long-context run fit.

## 0.1.118 Core Delta

Quantizy now treats sliding-window layers in the sparse KV planner as bounded
local memory instead of full-history KV. The policy defaults to a 4096-token
local window and honors `sparse_local_window_tokens` or `kv_window_tokens` when
the runtime supplies one.

This makes the DeepSeek-style hybrid plan materially more realistic for
lesser-PC long-context runs: local recency layers no longer erase the memory
benefit of compressed sparse and heavily compressed global-memory layers.

## 0.1.117 Core Delta

Quantizy now models the DeepSeek-style sparse KV path as a hybrid attention plan
instead of one generic sparse profile. Profiles now expose their CSA/HCA/sliding
window layer mix, weighted effective compression ratio, rollout stage, and
runtime requirements.

This improves the lesser-PC planning story because the estimator can now
distinguish retrieval-capable compressed sparse layers from heavily compressed
coarse memory and local recency windows before recommending a risky long-context
prototype.

## 0.1.116 Core Delta

Quantizy now includes a sparse selector runtime activation validator. A planned
DeepSeek-style selector config is not authorized just because it exists: the
validator checks the receipt status and verifies that the gate receipt matches
the exact `indexer_top_k` and `chunked_indexer_chunk_tokens` settings before
returning an enabled runtime config.

This makes sparse/indexed KV work safer for RAM-crowded local PCs: memory-saving
selector relief can move toward runtime use only after the matching quality gate
has passed.

## 0.1.115 Core Delta

Quantizy now binds sparse selector runtime configs to an explicit validation
receipt. The config records the required receipt path, expected pass status, and
activation rule before the selector can be enabled.

This closes another safety gap: sparse selector relief is not just planned, it
now has a concrete quality receipt requirement before runtime activation.

## 0.1.114 Core Delta

Quantizy now emits a runtime-facing sparse selector config artifact for
adoptable relief candidates. The config includes execution mode, chunk size,
top-k, quality-gate requirement, validation state, and the expected receipt path.

This moves sparse selector relief from planner-only metadata toward a concrete
runtime handoff object that can be persisted, inspected, and enabled only after
validation.

## 0.1.113 Core Delta

Quantizy now includes an implementation checklist with adoptable sparse selector
relief. The checklist names the required runtime wiring, chunked top-k wiring,
and quality-gate step, each tied to the selected selector parameters.

This makes the sparse relief path less ambiguous: it is now a gated execution
sequence, not just a candidate and a validation command.

## 0.1.112 Core Delta

Quantizy now includes explicit sparse selector implementation parameters with
the quality-gate hint and run-policy summary. The planner carries the selected
`indexer_top_k`, chunk size, retained top-k ratio, retrieval risk, and selector
pressure as a compact object.

This reduces ambiguity between planning and implementation: the runner can see
exactly which sparse selector settings must be implemented before validation.

## 0.1.111 Core Delta

Quantizy now promotes the sparse-selector quality-gate command into the
run-policy cost summary as `quality_gate_command`. Callers no longer need to
parse the nested command-hint object to execute the validation step.

This is a small but practical step toward making memory-saving sparse relief
paths runnable and auditable from the top-level planner output.

## 0.1.110 Core Delta

Quantizy now attaches a sparse-selector quality-gate command hint when a relief
candidate is adoptable. The command uses the existing `quality-gate` runner and
carries the selected `(chunk_tokens, top_k)` candidate as structured metadata.

This makes an adoptable sparse relief path executable: implement the relief
candidate, then run the generated quality gate before trusting it.

## 0.1.109 Core Delta

Quantizy now carries sparse selector relief into the run-policy cost summary.
When sparse KV needs selector relief, the summary reports the adoption decision,
relief status, recommended candidate, and whether a sparse quality gate is
required.

This makes the top-level planning output clearer for constrained machines:
standard KV search, sparse research, and resource recovery are now easier to
compare from one summary object.

## 0.1.108 Core Delta

Quantizy now surfaces sparse selector relief adoption in the top-level method
stack. The sparse/indexed KV entry includes the relief status, adoption decision,
and selected relief candidate when selector pressure blocks a direct prototype.

This lets the runner see whether sparse KV is immediately prototypeable, needs a
quality-gated relief candidate, or should remain research-only.

## 0.1.107 Core Delta

Quantizy now adds an adoption decision to sparse selector relief plans. A
candidate that lowers memory is no longer automatically treated as usable: the
planner distinguishes candidates that can proceed after a sparse quality gate
from candidates that should remain manual research or be rejected for extreme
retrieval risk.

This makes sparse relief recommendations closer to something a local runner can
trust instead of just a list of smaller memory numbers.

## 0.1.106 Core Delta

Quantizy now scores sparse selector relief candidates for retrieval quality
risk. Lowering `indexer_top_k` saves memory, but it can also remove useful
retrieval targets, so candidates now expose retained top-k ratio and a
moderate/high/very-high retrieval-risk tier.

The relief recommendation prefers lower retrieval risk before chasing the
smallest possible selector workspace.

## 0.1.105 Core Delta

Quantizy's sparse selector relief plan now searches both lower `indexer_top_k`
and smaller chunked-indexer tile sizes. This matters because selector workspace
comes from both the merge state and the score tile, not top-k alone.

The planner now reports combined `(chunk_tokens, top_k)` candidates and picks
the lowest-pressure candidate that still preserves the estimated memory fit.

## 0.1.104 Core Delta

Quantizy now emits a selector relief plan when sparse/indexed KV is blocked by
top-k workspace pressure. The planner tests lower `indexer_top_k` candidates
and reports whether any lower-workspace option can keep the memory fit.

This turns a blocked DeepSeek-style sparse route into an actionable tuning path:
reduce selector workspace first, then prototype only if the lower-pressure
candidate still closes the memory gap.

## 0.1.103 Core Delta

Quantizy now lets selector workspace pressure affect sparse/indexed KV
prototype decisions. A sparse profile can still report that it closes the memory
gap, but if the chunked top-k selector workspace is high or extreme relative to
the runtime budget, the planner downgrades it to research-only until selector
workspace is reduced or more RAM is available.

This keeps the huge-context path honest on constrained PCs instead of treating
all theoretical memory closures as equally runnable.

## 0.1.102 Core Delta

Quantizy now classifies sparse/indexed KV selector workspace pressure against
the actual runtime budget. The sparse policy reports whether the chunked top-k
workspace is low, moderate, high, extreme, or unknown because no runtime budget
was supplied.

This helps avoid another local-PC failure mode: a profile can be technically
peak-aware but still too workspace-heavy for a machine already running IDEs or
other memory-hungry tools.

## 0.1.101 Core Delta

Quantizy now ranks sparse/indexed KV profiles with peak-aware selection logic.
If multiple profiles can fit, it picks the least risky one that still closes the
memory gap. If none fully fit, it picks the profile with the smallest remaining
peak-memory gap instead of falling through to an arbitrary research profile.

This makes the DeepSeek-style sparse route more useful for real constrained
machines: the recommendation now tracks both implementation risk and actual
memory recovery.

## 0.1.100 Core Delta

Quantizy's sparse/indexed KV policy is now peak-memory aware. It separates the
old KV-only estimate from the safer estimate that adds chunked top-k indexer
workspace back into the memory gap.

That prevents a false positive where sparse KV appears to fit because cache
bytes shrink, but the selector workspace still makes the run unsafe on a
RAM-crowded local machine.

## 0.1.99 Core Delta

Quantizy now plans sparse/indexed KV with StreamIndex-style execution risk
included. The planner estimates the materialized sparse-attention score matrix
and compares it with a chunked partition/merge top-k path before recommending
the DeepSeek-style research route.

This matters for smaller PCs because sparse attention can save KV cache while
still failing from selector workspace memory. Quantizy now exposes that hidden
memory risk instead of only reporting the retained KV-cache ratio.

## 0.1.98 Core Delta

Quantizy's sparse/indexed KV policy now includes a `prototype_decision`. The
planner distinguishes:

- prototype only after standard KV search fails
- keep as research-only gap reduction
- do not prototype for the current case

This keeps the DeepSeek-V4-inspired path useful without letting a research
estimate override cheaper, already-validated recovery paths.

## 0.1.97 Core Delta

Quantizy now adds a DeepSeek-V4-inspired `sparse_indexed_kv_policy` for
ultra-long-context KV bottlenecks. When context is very large and KV cache is the
dominant pressure source, the planner estimates whether compressed sparse /
indexed KV retention profiles could close the memory gap.

The method stack now exposes this as a research-grade option after standard KV
search and before fallback. It is intentionally marked as validation-required:
this is a planning signal for future sparse-attention work, not a claim that V4
kernels are already implemented in Quantizy.

## 0.1.96 Core Delta

Quantizy's `recovery_decision` now explains why the selected RAM/context recovery
path is worth taking. It reports:

- extra context tokens preserved versus the fallback cap
- context tokens gained per extra MiB reclaimed
- the free-RAM requirement
- a concise decision rationale

This makes the crowded-RAM recommendation easier to trust: users can see what
they gain by closing apps instead of immediately dropping to the known context
fallback.

## 0.1.95 Core Delta

Quantizy's run-policy cost summary now includes a `recovery_decision` beside the
close-apps target. When a high-context retry is possible but tight, the planner
can explicitly choose:

- free RAM, then run the safer context
- run the safer context immediately
- use the known fitting context fallback

This turns staged recovery into an executable decision instead of just telemetry.

## 0.1.94 Core Delta

Quantizy's run-policy cost summary now includes a `close_apps_target` when a
safe high-context retry needs more RAM. It reports:

- extra RAM to reclaim in bytes/MiB/GiB
- target safety margin
- estimated margin after the staged KV search
- runtime safety action
- closure confidence

This turns crowded-desktop guidance into a concrete instruction: how much memory
to free before trusting the higher-context run.

## 0.1.93 Core Delta

Quantizy's `run_policy.cost_summary` is now RAM-suitability aware. It reports
the memory gap as bytes, GiB, ratio, and percent of runtime budget, plus a
`ram_suitability` tier:

- `crowded_ram_requires_recovery`
- `tight_ram_monitor_closely`
- `comfortable_ram_after_policy`
- `unknown`

This makes the policy clearer on real desktops where IDEs and browsers are
already consuming memory before the model run starts.

## 0.1.92 Core Delta

Quantizy's `run_policy` now includes a cost summary for staged recovery:

- method and command counts
- search-stage count
- first-stage candidate multiplier
- total candidate multiplier
- worst search-cost tier
- whether the policy starts with a low-cost screen

This helps smaller machines avoid surprise full sweeps: the app can show when a
plan begins cheaply and when escalation would become expensive.

## 0.1.91 Core Delta

Quantizy now emits a `run_policy` for bottleneck recovery. The method stack still
shows every stage, but the app also gets a concise execution policy:

- first method and command
- escalation gate
- stop-after-validation method
- fallback method
- whether quality validation is required

For KV-dominated huge-context failures, this defaults to a cheap screen first and
escalates only when that screen cannot close the memory gap.

## 0.1.90 Core Delta

Quantizy now adds escalation gates to the bottleneck `method_stack`. The planner
can say whether a stage is only a screen, whether it can stop after validation,
or whether the user should fall back to resource recovery or lower context.

This follows the practical lesson from MLA and paged-KV systems: attack KV memory
directly, but do it in staged steps so smaller machines do not pay for the
largest search unless the cheap screen fails.

## 0.1.89 Core Delta

Quantizy's bottleneck `method_stack` is now quality-aware. Each recovery method
can report:

- memory-fit risk
- quality risk
- whether quality validation is required
- the quality-gate command hint to run after memory-fit experiments

This keeps aggressive KV methods useful without overselling them: the planner
can recommend latent/head-sharing/pruning searches for bigger-model fit, while
also marking them as memory-fit steps that need validation before quality claims.

## 0.1.88 Core Delta

Quantizy now adds a ranked `method_stack` to the bottleneck diagnosis. For
KV-dominated huge-context failures, the recommendation no longer stops at
"try KV compression"; it sequences the work:

- quick latent/head-sharing/tail-precision screen first
- full target-aware KV search only when the quick screen is not enough
- explicit resource-recovery fallback when the fit is still tight

This saves time on smaller machines by avoiding a full aggressive sweep as the
first move, while still preserving a path to the high-upside search.

## 0.1.87 Core Delta

Quantizy now reports a top-level `bottleneck_diagnosis` in the fit-matrix
recommendation. Instead of making users infer why a huge-model run failed, the
planner identifies the dominant pressure class and points at the right remedy:

- KV-cache capacity: latent KV, head sharing, cross-layer KV, pruning, or tiered
  offload
- prefill workspace: chunked prefill before giving up on context
- weight/non-KV memory: lower weight memory, lazy sidecars, or a smaller model
- no bottleneck: run the selected recipe as-is

This is the product becoming more model-aware: long-context failures can now say
"KV dominates, try KV compression first" directly.

## 0.1.86 Core Delta

Quantizy now returns a single `resource_recovery_top_choice` beside the full
resource ladder. The app can show one plain first move while still exposing the
full ladder for inspection:

- close apps/free RAM when that preserves a large amount of context
- free SSD space when it buys a better offloaded KV tail
- run the confirmed sidecar plan when disk headroom is already available
- fall back to the known fitting context when that is the practical choice

This is another normal-user usability step: the output now has a direct
"do this first" object instead of only a list of possible recovery paths.

## 0.1.85 Core Delta

Quantizy now reports a RAM recovery tradeoff for high-context retries. When a
KV recipe can run only after freeing memory or lowering context, the
`resource_recovery_ladder` includes `ram_recovery_tradeoff` with:

- extra context tokens preserved versus the fallback cap
- required free RAM
- context tokens gained per extra MiB
- RAM value tier
- a direct recommended choice

This makes the crowded-desktop case clearer: users can see whether closing apps
is worth it, or whether they should use the known fitting context fallback.

## 0.1.84 Core Delta

Quantizy now turns SSD-sidecar tradeoff scores into a direct choice hint. The
`disk_fitting_tradeoff` object reports a `recommended_choice` such as:

- `free_ssd_for_target_tail`
- `prefer_disk_fitting_fallback`
- `use_disk_fitting_fallback`
- `needs_user_choice`

This lets the app guide users without asking them to interpret quality-per-GiB
numbers manually.

## 0.1.83 Core Delta

Quantizy now scores the storage efficiency of SSD-sidecar upgrades. When a
better offloaded KV tail needs more SSD reserve than the disk-fitting fallback,
`disk_fitting_tradeoff` reports:

- quality-score gain per extra GiB
- storage value tier: `excellent`, `good`, `thin`, or `no_quality_gain`

That makes the recovery advice sharper: users can see whether freeing storage is
a high-value move or whether they should stick with the already-fitting tail
recipe.

## 0.1.82 Core Delta

Quantizy now reports the quality/storage tradeoff when a better offloaded KV
tail is blocked by SSD headroom. The `offload_disk_plan` includes a
`disk_fitting_tradeoff` object that compares:

- target tail quality score
- disk-fitting fallback tail quality score
- quality-score delta
- extra SSD reserve needed to unlock the better tail
- target versus fallback key/value tail precision

This turns SSD recovery from "free some space" into a concrete decision: how
much storage buys how much better KV tail precision.

## 0.1.81 Core Delta

Quantizy now includes a disk-fitting fallback inside `offload_disk_plan` when a
higher-quality KV tail is blocked by SSD sidecar headroom. If the best tail
needs more sidecar reserve than the machine has, the report now names the best
already-fitting offload recipe too.

That means the user gets a real choice:

- free the exact SSD shortfall to unlock the better KV tail
- run the best lower-sidecar tail recipe that fits the declared/probed disk

This makes the offload path more resilient for small machines with tight local
storage, especially when RAM can be saved only by moving cold KV state to disk.

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
