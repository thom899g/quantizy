# Quantizy

Fit a bigger brain on your Mac.

Quantizy is a native Apple Silicon app for local-model fit checks, memory
guardrails, and validated MoE artifact handoff. It is built for normal users who
want to understand what can actually run on their own Mac before they waste time
and disk space downloading huge model weights.

## Download

- macOS Apple Silicon DMG: https://ontarioprotocol.com/Quantizy-macos-arm64.dmg
- Release manifest: https://ontarioprotocol.com/Quantizy-release.json
- Versioned DMG: https://ontarioprotocol.com/quantizy/0.1.6/Quantizy-macos-arm64.dmg
- Buyer quickstart: https://ontarioprotocol.com/quantizy/0.1.6/BUYER_QUICKSTART.md

## Verify

```bash
shasum -a 256 Quantizy-macos-arm64.dmg
```

Expected SHA-256:

```text
3c434903611312db20f681576e0a7b456ab05b30c7862e036cb4b49b1eb9a492
```

The app and DMG are Developer ID signed, Apple-notarized, stapled, and
Gatekeeper-accepted.

## Honest Scope

Quantizy V1 is not a universal "run any giant model on any small PC" miracle.
The sellable V1 promise is narrower and more useful:

- local Mac fit checks that account for current free RAM
- memory guardrails for machines already running IDEs, browsers, and other apps
- paid app activation with offline-verifiable licenses
- validated-artifact handoff where measured evidence exists
- clear fallback guidance when a model does not fit

The compression-quality claim has robust evidence on OLMoE and Granite target
gates, but broader buyer-facing models such as Qwen-class targets still need
their own validation before being marketed as a breakthrough.

## Release

- Version: `0.1.6`
- Notary submission: `29fd5155-fd4d-4608-9842-7507ec678fac`
- Signing identity: `Developer ID Application: THOMAS TOBIAS HANSEN (857AHQ92PC)`
- Release manifest: [`Quantizy-release.json`](./Quantizy-release.json)
- Quickstart: [`BUYER_QUICKSTART.md`](./BUYER_QUICKSTART.md)
- Checksums: [`CHECKSUMS.txt`](./CHECKSUMS.txt)

## License

Quantizy is proprietary software. See [`LICENSE.md`](./LICENSE.md).
