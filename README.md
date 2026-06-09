# Sequesign Protocol

> Cryptographic receipts for delegated AI work.

This repository is the canonical specification of the Sequesign protocol: an open, vendor-neutral protocol for producing and verifying cryptographic receipts of work performed by AI agents and the software around them.

A Sequesign receipt is a tamper-evident record of what an agent did, what it had authority to do, who authorized it, and what evidence backs each step. Receipts are independently witnessed at the moment work happens and can be verified offline by any party with the receipt and the relevant public keys.

## What is this repository

This is the protocol specification, not an implementation. It contains:

- **spec/** ? the protocol specification itself
- **diagrams/** ? architecture, sequence, and lifecycle diagrams
- **examples/** ? worked examples that build from minimal receipts to fully attested ones
- **test-vectors/** ? deterministic test vectors that conforming implementations must reproduce
- **schemas/** ? JSON Schema documents for envelope shapes
- **docs/** ? supporting documents including the publication plan

The reference implementation is operated by Sequesign, Inc. The SDK will be published as `@sequesign/sdk` on npm. Other implementations are encouraged; the test vectors are the contract.

## What problem does this solve

When an AI agent acts on someone else's behalf, the people who depend on that work ? the principal, an auditor, an insurer, a counterparty, a regulator ? need to know what the agent actually did and that the record has not been altered after the fact.

Existing approaches address parts of this:

- **Observability platforms** record what happened but the operator holds the logs. The operator can edit them. The records are not portable across services.
- **Audit trails** in compliance products are similarly operator-held and rarely cryptographically bound.
- **Blockchain anchoring** can prove existence at a point in time but does not capture the structure of agent work or the authority under which it was performed.

The Sequesign protocol addresses the gap directly. Every action an agent takes is hashed and witnessed by an independent third party at the moment it happens. Witnessing produces a signed attestation; signatures chain together into a tamper-evident sequence; the resulting receipt is portable and offline-verifiable.

## Design properties

The protocol is designed around six properties:

- **Offline verifiability.** Any party with a receipt and the relevant public keys can verify the receipt without contacting Sequesign or any other service. The verifier is deterministic.
- **Independent witnessing.** The witness is structurally separate from the agent and from the operator. Witnesses sign hashes; they do not hold content.
- **Hash-only by default.** The witness sees only hashes of evidence, never the evidence itself. This is stronger than customer-managed encryption: the witness never has the bytes, encrypted or otherwise.
- **Layered verification.** Receipts can reach increasing verification levels depending on what attestations they carry: integrity only, witnessed, identity-bound, policy-bound, human-approved, counterparty-confirmed.
- **Open protocol.** The protocol is specified in this repository under CC BY 4.0, with an explicit patent commitment from Sequesign, Inc. Anyone can implement it. The test vectors are the contract.
- **Vendor neutrality.** Sequesign, Inc. operates one reference implementation. The protocol does not depend on that implementation existing.

## Status

This specification is at version 1.0.0 as of June 2026. The wire format is locked.

The specification is a work in progress in terms of presentation: this repository is being built out incrementally with diagrams, worked examples, machine-readable test vectors, and JSON Schemas across a planned sequence of changes. See [docs/PLAN.md](docs/PLAN.md) for the publication plan.

The reference implementation is operated by Sequesign, Inc. The SDK is preparing for first public release as `@sequesign/sdk` on npm.

## Reading order

For implementers and reviewers:

1. The spec itself (forthcoming in this repo; the current canonical text lives in the reference implementation repository under `docs/protocol-spec.md`).
2. The worked examples ? to see what a real receipt looks like at each verification level.
3. The test vectors ? to validate your implementation against deterministic byte-level expectations.
4. The JSON Schemas ? for envelope shape validation.

For decision-makers evaluating the protocol:

1. The "What problem does this solve" and "Design properties" sections above.
2. The architecture diagram (forthcoming in `diagrams/`).
3. The "Sequesign as one implementation, the protocol as the contract" framing in NOTICE.

## License and patent commitment

This specification is licensed under [CC BY 4.0](LICENSE).

Sequesign, Inc. has filed a provisional patent application covering aspects of the protocol and commits not to assert these patents against good-faith implementers of the specification. See [NOTICE](NOTICE) for the full commitment.

"Sequesign" is a trademark of Sequesign, Inc. and is not licensed under CC BY 4.0. Implementers may describe their work as "implementing the Sequesign protocol" or "compatible with the Sequesign protocol" in factual reference.

## Contributing

This specification is currently maintained by Sequesign, Inc. External contributions will be welcomed once the contribution model and contributor agreement are established. In the meantime:

- Bug reports against the specification are welcome via GitHub Issues.
- Implementation reports from anyone building a conforming implementation are welcome via Discussions.
- Substantive proposals for changes to the protocol should be discussed in Issues before any PR.

## Contact

- Specification questions: open a GitHub Issue
- Patent or licensing questions: legal@sequesign.com
