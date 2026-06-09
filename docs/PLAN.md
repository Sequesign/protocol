# Publication plan

This document tracks the planned sequence of changes that build out the Sequesign protocol specification repository from its initial scaffolding to a complete, implementer-ready v1.0.0 specification.

The repository is being built incrementally. Each PR below is scoped to land as a single focused change with the deliverable in working condition at every step. Anyone reading the repo at any point should find consistent, navigable content.

## Status legend

- `[planned]`: scoped but not yet started
- `[in progress]`: branch open, work underway
- `[shipped]`: merged

## Dependencies

PR 1 and beyond depend on the canonicalization algorithm being finalized in the reference implementation. Specifically, the canonicalization adoption of JSON Canonicalization Scheme (RFC 8785) per the reference repository's roadmap must land before PR 1 of this repository ships, so that section 2.5 of the spec is correct on first publication rather than immediately obsolete.

PRs 4 (worked examples) and 5 (test vectors) depend on the byte-level canonicalization output being stable. If JCS adoption produces byte-identical canonical output to Sequesign's existing canonicalizer (the expected outcome), pre-JCS and post-JCS test vectors are interchangeable. If JCS adoption changes any byte, test vectors must be generated post-JCS.

## Plan

### PR 0: Initial scaffolding `[shipped]`

Repository skeleton: README, LICENSE (CC BY 4.0), NOTICE (patent commitment), .gitignore, directory structure with .gitkeep files, this plan document.

### PR 1: Specification text, restructured for an external reader `[planned]`

Copy the canonical protocol specification from the reference implementation into `spec/protocol.md`. Reorganize for an external reader: strip internal context, add an "About this document" front matter, restructure section ordering so a first-time reader can follow a layered reading path. No new technical content; structural and editorial only.

Deliverable: `spec/protocol.md` is the canonical specification text in its restructured form.

### PR 2: Conceptual diagrams `[planned]`

Architecture overview, trust model, three-tier privacy model. Excalidraw, exported to SVG and PNG. These are the "what is this thing" diagrams referenced from the README and from the early sections of the spec.

Deliverable: three diagrams in `diagrams/` with both source files and exported images. README updated to embed the architecture diagram.

### PR 3: Sequence diagrams `[planned]`

Mermaid, embedded directly in spec sections where flow matters. The flows: direct-mode receipt creation, managed-mode receipt creation, receipt verification, witness log inclusion, completeness checking.

Deliverable: five sequence diagrams embedded in `spec/protocol.md`. Mermaid renders inline on GitHub so no separate image generation step is required.

### PR 4: Worked examples `[planned]`

A series of receipts that build on each other, each shown alongside the diff from the previous one:

1. Minimal receipt: single freeform action, no attestations (L0 integrity only)
2. Witnessed receipt: same shape plus witness attestations (L1)
3. Identity-bound: agent identity registered (L2)
4. Policy-bound: policy context hash threaded through (L3)
5. Human-approved: counterparty workflow with human approval attestation (L4)
6. Counterparty-confirmed: external counterparty signature (L5)
7. Hash-only variant: showing the evidence custody distinction without changing verification levels

Each example includes the receipt JSON, an annotated commentary, and the diff from the prior example.

Deliverable: `examples/01-minimal/` through `examples/07-hash-only/`, each with `receipt.json`, `README.md` commentary, and a `diff-from-previous.md` (where applicable).

### PR 5: Test vectors `[planned]`

Deterministic test vectors extracted from the reference implementation. A conforming implementation must reproduce these byte-identically. Vectors include:

- Canonical JSON of representative envelope shapes
- SHA-256 hashes of action records
- Ed25519 signature inputs (length-prefixed message bytes)
- Chain state transitions

Includes a README explaining what each vector tests and how implementers should use them.

Deliverable: `test-vectors/vectors.json` with deterministic test inputs and expected outputs, plus `test-vectors/README.md` explaining the methodology.

### PR 6: JSON Schemas `[planned]`

Machine-readable JSON Schema documents for the receipt envelope, witness attestations, action records, and the other structured shapes the protocol defines. Lets implementers in any language validate receipts without reading the prose spec.

Deliverable: `schemas/` populated with one JSON Schema file per envelope shape, plus a `schemas/README.md` index.

## After v1.0.0

These are not part of the initial publication plan but are tracked here for visibility:

- **Reference test suite.** A small TypeScript test harness in `test-vectors/` that an implementer can run against their own code to verify conformance. Optional but useful.
- **Conformance badge.** A README badge any implementer can claim once they pass the test vectors. Lowers the bar for "I built a Sequesign implementation" signaling.
- **Internationalization or summary cards.** Short one-page summaries of the protocol for non-implementer audiences (compliance officers, procurement, executives). Possibly in multiple languages.
- **IETF or W3C engagement.** If adoption signals warrant, prepare the specification for formal standards submission.

## Editorial conventions

These apply to all content in this repository.

- Single capital "Sequesign" throughout. Never "SequeSign" or "sequesign" as a standalone product name.
- US spellings.
- No em dashes anywhere. Use commas, semicolons, or sentence breaks.
- The protocol is described in vendor-neutral language: "the Sequesign protocol" rather than "Sequesign". The company is "Sequesign, Inc." when explicit attribution is needed.
- "Patent pending" only; no claims about granted patents until they issue.
- Audit-framing vocabulary: records, captures, finalizes, verified, flagged.
