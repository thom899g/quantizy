# Quantizy

Fit a bigger brain on your Mac.

Quantizy is a native Apple Silicon app for local-model fit checks, memory
guardrails, and validated MoE artifact handoff. It is built for normal users who
want to understand what can actually run on their own Mac before they waste time
and disk space downloading huge model weights.

## Download

- macOS Apple Silicon DMG: https://ontarioprotocol.com/Quantizy-macos-arm64.dmg
- Release manifest: https://ontarioprotocol.com/Quantizy-release.json
- Versioned DMG: https://ontarioprotocol.com/quantizy/0.1.9/Quantizy-macos-arm64.dmg
- Buyer quickstart: https://ontarioprotocol.com/quantizy/0.1.9/BUYER_QUICKSTART.md

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

## 0.1.9 Delta

Quantizy now ships as a license-gated, signed, notarized paid beta build.
The route-group prefetch scoring from `0.1.8` remains included: on a crowded
Mac, Quantizy can warm the highest-value offloaded MoE subset instead of
skipping an oversized companion group.

## License

Quantizy is proprietary software. See [`LICENSE.md`](./LICENSE.md).
