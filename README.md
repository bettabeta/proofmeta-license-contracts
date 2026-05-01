# ProofMeta License Contracts

> **Layer:** License
> **Depends on:** [proofmeta-primitive-core](https://github.com/bettabeta/proofmeta-primitive-core)
> **Guarantees:** Chain-agnostic license semantics

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

---

## The idea in 10 seconds

A license is just four things:

```
scope        → what you're allowed to do
terms        → the fine print (hashed, so it can't change silently)
pricing      → what it costs (or: free)
id           → a name to reference it by
```

That's it. One JSON object. Works for a beat, an API, a dataset, a model, a book — anything.

## What a license looks like

```json
{
  "id": "commercial-standard",
  "scope": ["commercial", "derivative-allowed", "ai-training-excluded"],
  "terms_url": "https://youragent.ai/terms/commercial",
  "terms_hash": "sha256:a1b2c3...",
  "pricing": {
    "model": "one-time",
    "amount": "5.00",
    "currency": "USD"
  }
}
```

An agent reads this and knows everything it needs:
- ✅ Can I use this commercially? → yes (`commercial`)
- ✅ Can I remix it? → yes (`derivative-allowed`)
- ❌ Can I train on it? → no (`ai-training-excluded`)
- 💰 What does it cost? → $5, one-time

No ambiguity. No lawyer needed. No chain required.

## Pricing models

| Model | When to use | Example |
|-------|-------------|---------|
| `free` | Open access | Community tools, open data |
| `one-time` | Pay once, use forever | Stock photos, beats, templates |
| `subscription` | Recurring access | SaaS APIs, premium feeds |
| `usage-based` | Pay per use | AI training data, per-request APIs |

## Ready-made templates

Don't want to write your own? Copy one:

| Template | Scope | Pricing |
|----------|-------|---------|
| [free-attribution](templates/free-attribution.json) | Non-commercial, derivatives OK, credit required | Free |
| [commercial-standard](templates/commercial-standard.json) | Commercial, derivatives OK, no AI training | $5 one-time |
| [commercial-no-ai](templates/commercial-no-ai.json) | Commercial, credit required, no AI training | $10 one-time |
| [ai-training-dataset](templates/ai-training-dataset.json) | Commercial, AI training OK, credit required | $0.01/record |
| [saas-api-access](templates/saas-api-access.json) | Commercial, sublicensable, revocable | $29/month |

## How it fits together

```
┌─────────────────────────────┐
│  Execution Layer            │  ← Stripe, Solana, Lightning, IPFS
│  (how you pay & deliver)    │     chain-specific, pluggable
├─────────────────────────────┤
│  License Layer  ← you are here
│  (what you're allowed to do)│     chain-agnostic, universal
├─────────────────────────────┤
│  Primitive Layer            │  ← Signed Envelopes, DID, ed25519
│  (cryptographic proof)      │     the math
└─────────────────────────────┘
```

The License Layer never touches chains, payments, or delivery. It only answers: **what are the rights, what are the terms, what's the price?**

Everything else is a plugin.

## Scope tags

Defined in [primitive-core/docs/scope-vocabulary.md](https://github.com/bettabeta/proofmeta-primitive-core/blob/main/docs/scope-vocabulary.md). The core set:

| Tag | Means |
|-----|-------|
| `commercial` | Commercial use allowed |
| `non-commercial` | Non-commercial only |
| `derivative-allowed` | Remixes, adaptations OK |
| `attribution-required` | Must give credit |
| `ai-training-allowed` | ML training OK |
| `ai-training-excluded` | No ML training |
| `sublicense-allowed` | Can sub-license |
| `revocable` | Provider can revoke |

Need something custom? Use a URL: `"https://youragent.ai/scope/eu-only"`

## Schemas

- [`schemas/license-type.schema.json`](schemas/license-type.schema.json) — canonical license type
- [`schemas/purchase-intent.schema.json`](schemas/purchase-intent.schema.json) — signed pre-payment quote that binds terms + price
- [`schemas/run-receipt.schema.json`](schemas/run-receipt.schema.json) — deterministic execution proof receipt
- [`schemas/rating.schema.json`](schemas/rating.schema.json) — verified post-consumption rating linked to a receipt

### Why purchase intents + run receipts + ratings are in license-contracts

They define chain-agnostic contract semantics for payment + execution + trust feedback:
- What must be locked before payment (intent: license/constraints/price/expiry)
- What must be proven after a paid run (version, input/output hash, constraints, payment reference, signatures)
- How consumers can submit verifiable ratings tied to actual execution

Execution repos (Solana/x402/Stripe/etc.) implement these contracts, but the contracts themselves stay portable here.

## License

Copyright © 2026 Daud Zulfacar, Pandr UG (haftungsbeschränkt)

Apache License 2.0 — see [LICENSE](LICENSE).
