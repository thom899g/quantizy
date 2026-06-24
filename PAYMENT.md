# Quantizy Payment And Fulfillment

## Current Price

- Early paid beta: **$19**
- Planned V1 personal license: **$49**

## Buyer Flow

1. Sponsor `thom899g` for **$19 or more**:
   https://github.com/sponsors/thom899g
2. Download the signed DMG:
   https://github.com/thom899g/quantizy/releases/download/v0.1.9/Quantizy-macos-arm64.dmg
3. Open a license request:
   https://github.com/thom899g/quantizy/issues/new/choose
4. The maintainer verifies the sponsor/payment account and sends an offline
   Quantizy license key manually.

## Fulfillment Status

Manual fulfillment is live. Automatic Stripe fulfillment remains blocked until:

- `https://worker.ontarioprotocol.com/ready` resolves and returns healthy.
- The Ontarioprotocol DMG mirror returns the same SHA-256 as the GitHub release.
- The webhook creates and returns a valid offline license key after payment.

## Delivery Promise

Buyers should receive:

- Signed and notarized macOS Apple Silicon app
- Offline-verifiable license key
- Buyer quickstart
- SHA-256 checksum
- Support path for license recovery
