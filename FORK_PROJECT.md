# Minimal Chromium Browser Fork — Project Charter

> **Status:** Foundation / planning. No browser functionality has been modified or tested yet.
> **Working name:** Unnamed browser fork (final product name and branding are intentionally undecided).
> **Source:** [JPLuker/brave-core](https://github.com/JPLuker/brave-core), forked from [brave/brave-core](https://github.com/brave/brave-core).
> **Scope:** Windows and Linux desktop first. Android and macOS are not initial deliverables.

## Product intent

Ship a familiar, fully functional Chromium browser with Brave's useful security/privacy features but without Brave's advertising business, crypto/Web3 features, product upsells, AI assistant, or distracting promotional surfaces.

**This is a conservative fork, not a new browser engine or a UI redesign.** Reuse working upstream functionality, minimize patches, and make security updates sustainable.

### Non-negotiables

- Chromium engine and modern Chrome/Chromium extension compatibility.
- Preserve ordinary browser functionality: profiles, tabs, navigation, downloads, bookmarks, password manager, autofill, settings, and devtools.
- Preserve meaningful privacy protections and safe browsing behavior. Keep Brave Shields as a candidate unless a concrete conflict is demonstrated.
- Keep the **full uBlock Origin** (not merely uBlock Origin Lite) as a goal. Offer a clear **optional first-run installation prompt**, rather than silently installing it. Confirm a technically and legally viable Manifest V2 install/update channel before promising success.
- Remove the user's unwanted Brave-only features without leaving dead controls, broken preferences, or unexplained background services.
- Independent application identity and update infrastructure before public distribution.
- Security fixes and working updates take precedence over cosmetic polish.

### Non-goals for first release

- Android/iOS builds, custom browser engine, custom extension ecosystem, major redesign, sync service replacement, or new cloud account system.
- Re-creating services that belong to Brave (Rewards, Wallet, Leo, VPN, etc.).
- Reusing Brave branding, updater endpoints, proprietary names/icons or installer identities in distributed builds.
- Tracking or monetizing users.

## Source and build caveat

`brave-core` is **not a self-contained Chromium checkout**. As upstream documents, it lives at `src/brave` inside a larger workspace, and `pnpm run init` fetches Chromium and related dependencies. Do not interpret successful GitHub edits as a successful browser build.

At the time of foundation setup, `package.json` reports Brave Core version `1.101.33`, Chromium tag `157.0.8103.39`, Node `>=24.16.0 <25.0.0`, and pnpm `>=11.11.0`. These are a **snapshot**, not permanent project constraints.

## Working agreements

- Keep the fork's `master` branch an upstream reference; make project changes on `fork/*` branches.
- Keep removals narrow and upstream-mergeable; prefer build flags/configuration to broad code deletions where appropriate.
- Maintain an explicit feature inventory and test every removals batch.
- Every completed milestone must have evidence: build logs, executable smoke tests, or integration tests. A checked box does not mean "code committed."
- Preserve copyright notices, MPL 2.0 obligations, and all applicable third-party licenses.
- Do not make telemetry/security/privacy claims that have not been validated.

## Project documents

- [Roadmap](fork/ROADMAP.md) — sequence, completion criteria, release gates.
- [Feature inventory](fork/FEATURE_MATRIX.md) — keep/remove/investigate decisions and code areas.
- [Engineering and release plan](fork/ENGINEERING.md) — worktree, builds, extensions, security updates, packaging.

**Next action:** Obtain a reproducible **unmodified** Windows or Linux development build before changing product behavior.
