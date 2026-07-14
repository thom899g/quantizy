# Quantizy V1 Odysseus Handoff

This is a controlled, targeted outreach packet for the local-AI audience. It
is not permission to mass-send, scrape private contacts, or claim that a
message was sent without evidence.

## Product

- Front: https://github.com/thom899g/quantizy
- Release: https://github.com/thom899g/quantizy/releases/tag/v1.0.1
- DMG: https://github.com/thom899g/quantizy/releases/download/v1.0.1/Quantizy-macos-arm64.dmg
- DMG SHA-256: `03316a1198d154e22faa450c91dbada7191866af16b7e9ad9b7f6cd09f23cc38`
- Buyer guide: https://github.com/thom899g/quantizy/blob/main/BUYER_QUICKSTART.md
- Payment: https://github.com/sponsors/thom899g
- License request: https://github.com/thom899g/quantizy/issues/new/choose
- Price: `$49` personal V1 license
- Core research head: https://github.com/thom899g/quantizy-core/commit/759ae8a
- Core verification: `946 passed, 2 warnings`; this research head adds
  receipt-gated binary sparse-KV indexing, token-max needle scoring,
  mass-segmented region quotas, and bounded history-aware region scoring. It
  also bounds temporary quantization-sensitivity tensors for crowded Macs. It
  is not part of
  the signed V1.0.1 DMG until a deliberate versioned rebuild is published.
- Real-model gate: still unproven. A bounded OLMoE 6.92B full-precision
  sensitivity probe reached 5% system-wide free memory on the development Mac
  and was stopped; no quality result was recorded.

## Reviewer Offer

Offer a free reviewer license only to a person or team who already publishes
relevant local-AI, MLX, GGUF, Ollama, llama.cpp, or Apple Silicon work. The
reviewer may publish negative results. The test request is narrow: current-RAM
fit check, pressure fallback, install/activation, and whether the guide is
clear.

Do not promise a compression-quality breakthrough. Do not offer a Stripe coupon:
automatic Stripe checkout is closed because `worker.ontarioprotocol.com` does
not currently resolve. Reviewer licenses are manual offline licenses issued by
the maintainer after relevance is checked.

## Channel Rules

- Email: use only a public editorial or contact address and one personalized
  message.
- SitePoint and Towards AI: use their submission/editorial workflow manually;
  do not invent an email address or automate the form.
- YouTube: comments are manual and only when the video is directly relevant;
  no repeated comments or bulk creator spam.
- Reddit: post only in a self-promotion or weekly thread where the rules allow
  it, disclose that Quantizy was built by the poster, and do not use unrelated
  discussion threads as ads.
- GitHub: use Discussions or an issue only when the project welcomes tooling
  feedback; never post promotional comments in unrelated issues.
- Stop after one message per target. Follow up only after a reply or explicit
  invitation.

## Priority Batch

Use the full ranked list and exact copy in
[`OUTREACH_SEND_QUEUE.md`](OUTREACH_SEND_QUEUE.md). Start with five high-fit
targets, then wait for signal:

1. Contra Collective local-AI author/contact channel
2. SitePoint AI editorial submission
3. Towards AI submission flow
4. One MLX/GGUF Apple Silicon creator
5. One GitHub local-AI maintainer who welcomes integration feedback

The queue currently contains 30 ranked targets across publishers, video,
GitHub, and communities. “Ready” means drafted, not sent.

## Message

```text
Hi <name> — I’m building Quantizy V1, a signed Apple Silicon Mac app for
current-RAM local-AI fit checks and safer fallback recipes.

It helps users decide what can run while IDEs and other apps are already using
memory. It does not claim to beat every quantization method or make every huge
model fit.

I can provide a free reviewer license for an honest test of install, activation,
fit guidance, and the buyer workflow. The release is here:
https://github.com/thom899g/quantizy/releases/tag/v1.0.1

Would that be useful for your audience or project?
```

## Evidence And Status

The handoff itself performs no external action. Keep each queue row `ready`
until the exact channel URL, sent timestamp, and reply or outcome are recorded.
Never convert a draft into `sent` based on an email outbox assumption or an
agent log alone.

For a reviewer license, the maintainer may use the Keychain-backed command in
the core repo:

```bash
packaging/mint_manual_license_from_keychain.sh reviewer@example.com reviewer-reference
```

Do not print, paste, or commit the signing seed. Do not mint a paid license
until payment is confirmed. Do not mark revenue until a real payment receipt
exists.

```yaml
external_action_performed: false
payment_verified: false
reviewer_licenses_issued: 0
targets_ready: 30
targets_sent: 0
```
