# Raven Conformance — Crypto World's Fair 2026 (Build Stage)

Raven Conformance runs a Raven-owned demo profile against a fixed 12-vector corpus and reports exact expected-versus-observed decisions, CONFORMANT or DIVERGENT, per vector, with the evidence attached. It does not produce a safety score, a certificate, a signed receipt, or a live-chain claim.

## Run it (Node 22 or newer, no dependencies)

Follow `DEVELOPER.md`. In short:

```bash
cd apps/raven-conformance
npm test
npm run conform -- --target CONFORMANT_REFERENCE   # expected: 12 PASS, CONFORMANT, exit 0
npm run conform -- --target BROKEN_SUBTLE           # expected: 10 PASS, divergence on V07 and V08 only, exit 1
npm run replay -- --report ../../evidence/rehearsal/<run_id>.json   # replays a retained report against its bindings (path is relative to apps/raven-conformance)
npm start                                           # Judge UI on http://127.0.0.1:8791/
```

## What is in this repository

- `apps/raven-conformance/` — runner, profile `raven-canonical-envelope/1`, corpus `raven-canonical-envelope-demo-corpus/1` (12 vectors), two Raven-owned demo targets, Judge UI, replay.
- `api/` — the Vercel adapter and its runtime-root guard, with tests.
- `scripts/verify-judge-url.mjs` — the owner harness that checks a served Judge UI.
- `evidence/rehearsal/` — five recorded rehearsal reports, kept on purpose so `replay` works from a clean clone.
- `trust/` — the Fair brief, threat model and what-we-do-not-claim documents.
- `DEVELOPER.md`, `IDENTITY.md`, `LIMITATIONS.md`, `WALKTHROUGH.md`, `EXTERNALS.md`.

The Fair landing page source is not in this repository. Four historical handoff/review notes under `apps/raven-conformance/` (CHALLENGE1_HANDOFF.md, CHALLENGE2_HANDOFF.md, RELEASE-HANDOFF.md, REVIEW-FIXES.md) are retained because the shipped test suite reads them; each carries a HISTORICAL, NON-OPERATIVE banner. The root `DEVELOPER.md` is the only current cold-start authority.

## Built during the Fair window

The conformance loop (target → claimed profile → corpus → execution → evidence report → reproduction), the Judge UI, the 12-vector corpus, the reference and deliberately broken targets, fail-closed handling of crash, timeout, invalid output and output floods, replay and reproduction, and this package.

## Pre-existing work, not part of this submission

Raven receipt-v1, verify-js, ravenattest.com's receipt product and `plugin-raven-verify` predate the Fair and are related Raven work, not this app's output. Nothing here tests a chain, an RPC, a wallet, or third-party software.

## Review

The judge-facing claims in this README and in `DEVELOPER.md`, `LIMITATIONS.md` and `trust/` are subject to independent non-author review. Review records and their SHA-256 digests are maintained outside this product-only repository and published on the Fair landing page.

## Build identity

Run from a git checkout, the local server reports the checkout commit as `commitSource: git_checkout` with `identityStatus: UNVERIFIED_ASSERTION`. That is an asserted checkout identity, not deployment provenance. The public Judge UI reports `identityStatus: UNKNOWN`; its build-info is not proof that the served bytes came from a particular commit.
