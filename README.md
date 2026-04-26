# ProofMeta License Contracts

> **Layer:** License
> **Depends on:** [proofmeta-primitive-core](https://github.com/bettabeta/proofmeta-primitive-core)
> **Guarantees:** Chain-agnostic license semantics — machine-readable terms, scope vocabulary, and contract templates that work with any payment, delivery, or anchoring backend.

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Status: Draft](https://img.shields.io/badge/Status-Draft-yellow)]()

---

## What is this?

This package defines the **license semantics layer** of the ProofMeta protocol. It builds on the Signed Envelope primitive from `proofmeta-primitive-core` and adds:

- **License contract templates** — reusable, composable license definitions
- **Scope vocabulary extensions** — beyond the core tags defined in the primitive layer
- **Terms resolution** — how agents discover, compare, and agree on license terms
- **Contract lifecycle** — how license terms evolve (versioning, deprecation, migration)

## Design Constraints

1. **Chain-agnostic** — No chain-specific required fields. No `0x` addresses, no `programId`s, no chain IDs in the core schema.
2. **Extension-friendly** — Chain-specific data (Solana PDAs, EVM contract addresses, etc.) belongs in optional `extension` fields, scoped to the respective Execution Layer.
3. **Primitive-first** — Every license contract is a Signed Envelope. Verification uses the same `did:key` + `ed25519` + JCS pipeline from `primitive-core`.

## Repository Structure

```
/docs                   → Specification and design documents
/docs/DEPENDENCY_MODEL.md → Layer dependency model
```

## License

Copyright © 2026 Daud Zulfacar, Pandr UG (haftungsbeschränkt)

Licensed under the Apache License 2.0 — see [LICENSE](LICENSE) for details.
