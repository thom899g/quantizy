# Quantizy V1.0.0

Quantizy V1 is a signed and notarized Apple Silicon Mac fit-advisor release.

## Included

- Current-RAM fit checks for models already on the Mac
- RAM reserve and pressure-ladder guardrails for busy desktops
- Safer context, quantization, KV, paging, and prefill recipes
- Offline license verification and buyer start guidance
- Developer ID signing, Apple notarization, stapled tickets, and SHA-256 manifest

## Honest Scope

V1 helps normal local-AI users decide what can run with their available memory.
It does not claim that every huge model fits on every Mac, or that Quantizy
beats Unsloth, GGUF dynamic quants, or the best uniform bit-width on every
quality benchmark. That research gate remains open.

## Install

1. Download `Quantizy-macos-arm64.dmg`.
2. Verify SHA-256:

   `51b5b2f1bd1923d5d427c859f407840e1020031d39d312d85dedec71e7474622`

3. Open the DMG, drag Quantizy to Applications, launch it, and enter the
   offline license key from the buyer handoff.
4. Keep `Use current RAM` enabled and run `Fit Matrix` before export.

## Payment

V1 personal access is $49 through GitHub Sponsors with manual license delivery
after payment confirmation. Automatic Stripe checkout is closed until the
external fulfillment worker passes its readiness gate.
