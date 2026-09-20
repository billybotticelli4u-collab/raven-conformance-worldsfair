# Developer — stranger cold start

Requires **Node ≥ 22** (rehearsed on **Node 22.18.0** darwin-arm64).

```bash
node --version   # expect v22.18.0 (or any Node ≥ 22)
cd apps/raven-conformance
npm test                  # expect pass (skips OK)
npm run conform -- --target CONFORMANT_REFERENCE   # exit 0, 12/12
npm run conform -- --target BROKEN_SUBTLE          # exit 1, V07+V08 BEHAVIORAL_DIVERGENCE
npm run replay -- --report ../../evidence/rehearsal/<run_id>.json
npm start                 # http://127.0.0.1:8791
```

A1 guard tests (disposable dirs only; never point runtime root at os.tmpdir() itself).
Paths below are from the **package root** — leave the app dir first:

```bash
cd ../..
node --test api/vercel-adapter-guard.test.mjs
```

HTTPS-aware owner harness (against a base URL you control — e.g. the local server from `npm start`):

```bash
node scripts/verify-judge-url.mjs http://127.0.0.1:8791/
```

## Running the harness from a git checkout (measured)

From a clone of this repository, `npm start` resolves the checkout through `git rev-parse HEAD`, so `/api/build-info` reports `commitSource: git_checkout`, `identityStatus: UNVERIFIED_ASSERTION` and `fairBuildCommit: <your HEAD>`. That is an asserted checkout identity, not deployment provenance. Because the identity is not the exact `UNKNOWN`/`null` state, the harness reports **8 PASS / 0 LOCAL-UNBOUND / 1 FAIL** on the `EXPECTED_COMMIT binding` row until you pin the commit:

```bash
EXPECTED_COMMIT=$(git rev-parse HEAD) node scripts/verify-judge-url.mjs http://127.0.0.1:8791/   # measured: 9 PASS / 0 LOCAL-UNBOUND / 0 FAIL
```

The two-row LOCAL-UNBOUND result described above applies only when the app runs outside a git checkout (for example from the unpacked judge package).

## D6 — identity rows (LOCAL-UNBOUND is loopback-only)

`scripts/verify-judge-url.mjs` prints **LOCAL-UNBOUND** only when **all** of these hold:

1. the base URL host is loopback (`localhost`, `127.0.0.1`, or `::1`);
2. `/api/build-info` `identityStatus` is exactly `UNKNOWN` or `null`;
3. `fairBuildCommit` is `null`;
4. `EXPECTED_COMMIT` is unset.

Any **non-loopback** base URL with `EXPECTED_COMMIT` unset **FAIL**s (exit ≠ 0). Owner / production commands must pin a 40-hex `EXPECTED_COMMIT`:

```bash
EXPECTED_COMMIT=<40-hex> node scripts/verify-judge-url.mjs https://<deployed-host>/
```

Corrupt `identityStatus` on a non-loopback host never soft-passes. Prove with:

```bash
node --test scripts/verify-judge-url.identity.test.mjs
node scripts/verify-judge-url.d6-red.mjs
```

