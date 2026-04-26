# ProofMeta Dependency Model

## Layer Architecture

```
┌─────────────────────────────────────────┐
│         Execution Layer (Tier 3)        │
│  Chain-specific resolvers, anchors,     │
│  payment rails, delivery mechanisms     │
│  Depends on: primitive-core +           │
│              license-contracts          │
│  Examples: Solana PDA, Stripe, x402     │
├─────────────────────────────────────────┤
│         License Layer (Tier 2)          │
│  Chain-agnostic license semantics,      │
│  contract templates, scope vocabulary   │
│  Depends on: primitive-core             │
│  Guarantees: machine-readable terms     │
│  Repo: proofmeta-license-contracts     │
├─────────────────────────────────────────┤
│         Primitive Layer (Tier 1)        │
│  Signed Envelope, DID identity,         │
│  JCS hashing, ed25519 signatures,       │
│  status lifecycle                       │
│  Depends on: nothing                    │
│  Guarantees: cryptographic integrity    │
│  Repo: proofmeta-primitive-core        │
└─────────────────────────────────────────┘
```

## Rules

1. **Lower layers never depend on higher layers.** Primitive-core has zero knowledge of license semantics or execution details.

2. **Chain-specific code only lives in Execution.** The License layer and Primitive layer must compile and verify without any blockchain SDK installed.

3. **Extension fields bridge layers.** When an Execution Layer needs to attach chain-specific data to a license or envelope, it uses an `extensions` object — never a required top-level field.

4. **Each layer is independently versioned.** A new Execution Layer (e.g., adding Ethereum support) does not require a version bump in Primitive or License.

## Dependency Direction

```
Execution-X  →  primitive-core + license-contracts
Execution-Y  →  primitive-core + license-contracts
license-contracts  →  primitive-core
primitive-core  →  (none)
```

Multiple Execution Layers can coexist. They share the same Primitive and License foundations but differ in how they settle payments, anchor proofs, or deliver content.
