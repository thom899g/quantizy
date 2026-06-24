<p align="center">
  <img src="assets/quantizy-hero.png" alt="Quantizy compresses huge local AI models into a smaller Mac memory footprint" width="100%">
</p>

# Quantizy

**Fit a bigger brain on your Mac.**

Quantizy is a native Apple Silicon app for local-model fit checks, memory
guardrails, and validated MoE artifact handoff. It is built for normal users who
want to understand what can actually run on their own Mac before they waste time
and disk space downloading huge model weights.

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

## What Buyers Get When Checkout Opens

- macOS Apple Silicon app
- Offline-verifiable license key
- Release manifest and SHA-256 checksum
- Buyer quickstart
- Current-RAM fit checks and pressure-ladder recipes
- Validated artifact handoff where evidence exists

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

## License

Quantizy is proprietary software. See [`LICENSE.md`](./LICENSE.md).
