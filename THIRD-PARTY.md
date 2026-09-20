# Third-party and open-source code status (Crypto World's Fair Official Rules §9)

- Runtime dependencies: none. `apps/raven-conformance/package.json` declares no `dependencies` or `devDependencies`. The runner, targets, Judge UI, replay, adapter guard and the owner harness import Node.js built-in modules only (`node:` specifiers). Verified by scanning every import in `apps/`, `api/` and `scripts/`.
- The optional browser verification tool that depended on `puppeteer-core` and an external Chromium is not included in this repository.
- Language runtime: Node.js 22 or newer (MIT-licensed, not redistributed here).
- All code in this repository is Raven's own work, licensed under Apache-2.0 (see `LICENSE`).
- Pre-existing Raven work (receipt-v1, verify-js, ravenattest.com, plugin-raven-verify) is not included in this repository.
