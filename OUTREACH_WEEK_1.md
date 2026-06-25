# Quantizy Week 1 Outreach Plan

This is the first legitimate outreach pass. Do not automate this into bulk DMs.
Send small batches, personalize each message, and stop when a channel says no.

## Goal

Find honest early reviewers and the first paid beta users for Quantizy.

Primary promise:

> Quantizy helps Mac local-AI users know what their machine can actually run
> before downloading giant model files.

Do not lead with breakthrough compression claims. Lead with fit checks, current
RAM guardrails, staged recovery, and clear fallback decisions.

## Daily Send Limits

- Day 1: 5 highly relevant publisher/creator messages
- Day 2: 5 creator/community messages
- Day 3: 5 GitHub/community messages where the rules allow it
- Day 4: post one public launch note
- Day 5: follow up only with people who replied or explicitly invited contact

Never send the same message repeatedly. Never post in unrelated GitHub issues.

## Reviewer Offer

Use this for credible reviewers:

```text
I can give you a free reviewer license. You can publish criticism if it fails;
the only ask is that you test the actual fit-check/current-RAM workflow.
```

Use this for paid beta users:

```text
Paid beta is $19 through GitHub Sponsors. It includes the signed Mac build,
buyer quickstart, checksum verification, and manual offline license delivery.
```

## Batch 1: Send First

### 1. Contra Collective

Subject/DM:

```text
Reviewer license for a Mac local-AI fit-check app?
```

Message:

```text
Hi — your Apple Silicon MLX vs llama.cpp coverage is exactly the audience I’m
building for.

Quantizy is a Mac paid beta for local-AI fit checks, current-RAM guardrails, and
staged recovery decisions before users download giant model files.

It does not claim every huge model will run. The point is more practical: show
what fits, what needs RAM recovery, and when to lower context.

Would you be open to a free reviewer license and a brutally honest look?

https://github.com/thom899g/quantizy
```

### 2. SitePoint AI

Subject/DM:

```text
Tool idea for local LLM readers choosing models for Apple Silicon
```

Message:

```text
Hi SitePoint team — Quantizy is a Mac paid beta for people running local LLMs.

It checks current free RAM, model/context pressure, and safer fallback recipes
before users download or launch huge local model files.

Your local LLM guides already explain the model-choice problem. Quantizy tries
to answer the next question: will this run on my machine right now?

I can provide reviewer access if useful.

https://github.com/thom899g/quantizy
```

### 3. MLX / GGUF YouTube Benchmark Creator

Comment or email:

```text
Your MLX/GGUF benchmarks are exactly why I built Quantizy.

Once users know runtimes are fast, they still need to know whether a model fits
their current Mac RAM before downloading 20-100GB of weights.

Quantizy is a $19 paid beta for fit checks, pressure recipes, and staged KV/RAM
recovery decisions. I’m offering free reviewer licenses for honest tests:

https://github.com/thom899g/quantizy
```

### 4. r/LocalLLaMA Weekly/Self-Promo Thread

Only post if the rules allow self-promotion:

```text
Disclosure: I built this.

Quantizy is a Mac paid beta for local-AI fit checks. It checks current free RAM,
model/context pressure, staged KV recovery, and fallback context before you
download or try to run a huge local model.

It is not a miracle compressor and does not claim every giant model runs on
every Mac. The goal is simpler: avoid bad downloads and bad expectations.

I’m looking for honest early testers, especially 16GB/32GB/64GB Apple Silicon
users.

https://github.com/thom899g/quantizy
```

### 5. GitHub Maintainer / Local-AI Tooling

Use only in discussions or issues where integration feedback is welcome:

```text
Hi — I’m building Quantizy, a Mac local-AI fit-check utility.

It produces current-RAM fit reports, context fallback decisions, and staged
recovery plans before users load/download large local models.

Would a Quantizy fit report be useful as an optional companion artifact for
your users? I’m not asking for integration work; I’m looking for feedback on
whether this would help people choose safer model/settings combinations.

https://github.com/thom899g/quantizy
```

## Public Launch Post

Use on X, LinkedIn, Mastodon, or a personal blog:

```text
I’m opening the Quantizy paid beta.

It is a Mac app for local-AI users who want to know what their machine can
actually run before downloading giant model files.

V1 focuses on:
- current-RAM fit checks
- memory guardrails
- staged KV/RAM recovery decisions
- clear context fallback guidance
- signed Mac release + offline license

No miracle claims. The goal is fewer failed downloads and better local-model
decisions.

Paid beta: $19
https://github.com/thom899g/quantizy
```

## Reply Templates

### If They Ask For A License

```text
Great — thanks. Please sponsor at the reviewer tier if one is available, or send
your GitHub username and I’ll issue a reviewer license manually.

The useful test is:
1. pick a local model you would normally try,
2. run the fit/current-RAM check,
3. see if Quantizy correctly predicts fit, context fallback, or RAM recovery.
```

### If They Ask What Makes It Different

```text
Quantizy is not trying to replace MLX, llama.cpp, Ollama, or LM Studio.

It sits before the runtime decision: current-RAM fit checks, context pressure,
staged KV/RAM recovery decisions, and fallback guidance before the user wastes
time downloading or launching a model that will not fit.
```

### If They Challenge The Compression Claim

```text
That challenge is fair. I’m not currently marketing a broad “beats uniform
quantization” claim.

The sellable V1 claim is fit guidance and memory recovery for local Mac users.
Quality/compression wins need model-specific validation before being promoted.
```

## Tracking Log

Copy one row per send.

| Date | Target | Channel | Message variant | Status | Notes |
| --- | --- | --- | --- | --- | --- |
| YYYY-MM-DD |  |  |  | ready/sent/replied/declined |  |

