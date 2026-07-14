# Quantizy V1 Payment And Fulfillment

## Current Price

- Quantizy V1 personal license: **$49**
- Reviewer access: free for relevant reviewers who agree to test the actual
  fit-check workflow and share honest results

## Buyer Flow

1. Pay through [GitHub Sponsors](https://github.com/sponsors/thom899g).
2. Open a [license request](https://github.com/thom899g/quantizy/issues/new/choose)
   from the same GitHub account and include the sponsor/payment reference.
3. The maintainer confirms the payment and sends the same V1 handoff every
   time: offline license key, release URL, SHA-256 checksum, buyer quickstart,
   and support path.
4. Download V1 from the
   [GitHub Release](https://github.com/thom899g/quantizy/releases/tag/v1.0.1)
   and verify the checksum before opening the DMG.

V1 download:

```text
https://github.com/thom899g/quantizy/releases/download/v1.0.1/Quantizy-macos-arm64.dmg
```

V1 DMG SHA-256:

```text
03316a1198d154e22faa450c91dbada7191866af16b7e9ad9b7f6cd09f23cc38
```

## Operator Fulfillment

From the core repo, the maintainer can mint a license without exposing the
private signing seed:

```bash
packaging/mint_manual_license_from_keychain.sh buyer@example.com sponsor_reference
```

The command reads the seed from the macOS Keychain and produces the license
handoff. The V1 app verifies that key offline.

## Fulfillment Status

The local payment-to-delivery smoke test passes. Automatic Stripe fulfillment
is **not live**: `worker.ontarioprotocol.com` does not currently resolve, so no
Stripe checkout button is advertised or opened. This is deliberate; a paid
checkout must not accept money until the worker readiness gate passes with the
same V1 version and checksum.

## Delivery Promise

Confirmed buyers receive:

- Signed and notarized macOS Apple Silicon app
- Offline-verifiable license key
- V1 buyer quickstart
- SHA-256 checksum
- Support and license-recovery path

V1 sells fit advice, current-RAM guardrails, and safer local-model decisions.
It does not promise that every large model will run on every Mac or that
Quantizy beats every uniform or dynamic quantization baseline.
