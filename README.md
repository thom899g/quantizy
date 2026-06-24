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

## License

Quantizy is proprietary software. See [`LICENSE.md`](./LICENSE.md).
