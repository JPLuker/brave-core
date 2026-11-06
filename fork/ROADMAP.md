# Roadmap — Minimal Chromium Browser Fork

This roadmap is deliberately ordered to protect compatibility and minimize maintenance cost. **A task is not complete merely because the source changed.** Each phase has an acceptance gate. Track changes with focused fork PRs; only merge when the relevant gate passes.

## Phase 0 — Foundation (current)

- [x] Fork `brave/brave-core` as `JPLuker/brave-core`.
- [x] Establish `fork/foundation` development branch, leaving `master` unchanged.
- [x] Define scope, non-goals, initial feature policy, and release principles.
- [ ] Confirm a final project name, logos, product IDs, and website **before** rebranding or publishing installers.
- [ ] Enable GitHub Issues on this fork for tracked work if desired (the fork currently has Issues disabled).
- [ ] Establish actual build-machine specs and required signing/distribution credentials, when ready.

**Gate 0:** This plan is in GitHub and a narrow next development task is identified.

## Phase 1 — Reproducible upstream baseline (must happen before deletions)

- [ ] Provision a suitable Windows and/or Linux build workstation with ample disk, RAM, CPU, and bandwidth.
- [ ] Build the unmodified fork at a pinned upstream commit using the documented Chromium workspace layout.
- [ ] Record upstream tag/SHA, build flags, host OS, toolchain, test command, and resulting binary.
- [ ] Smoke-test browser startup, navigation, two profiles, downloads, bookmarks, passwords, settings, devtools, and Chrome-compatible extensions.
- [ ] Choose a CI strategy: lightweight documentation/static checks on GitHub; full Chromium builds on properly provisioned runners only.
- [ ] Verify project-specific upstream synchronization workflow.

**Gate 1:** A repeatable unmodified build succeeds on at least one desktop OS. No claims of release readiness before this.

## Phase 2 — Remove Brave-specific clutter, one family at a time

- [ ] Audit build flags and dependency graph, including Brave Origin paths; do **not** reuse Brave Origin paid/subscription infrastructure.
- [ ] Disable/remove **Brave Rewards, Brave Ads, sponsored/promoted New Tab content**, and related onboarding.
- [ ] Disable/remove **Brave Wallet, crypto/Web3 integrations**, and related menus/preferences.
- [ ] Disable/remove **Leo AI**, Brave News, Brave Talk upsells, and Brave VPN promotion/integration.
- [ ] Audit referrals, promos, notifications, telemetry/P3A, service calls, defaults, and dead settings links.
- [ ] Decide on Brave Search defaults, built-in adblocking vs uBlock Origin, and Brave Sync after dependency testing.
- [ ] Verify unchanged core Chromium workflows and security/privacy protections after **each** removal family.

**Gate 2:** No targeted surfaces or background service startup remain; browser functionality is verified by tests and manual checks. "Hidden" is not equivalent to "removed."

## Phase 3 — Extensions and first-run experience

- [ ] Verify Chrome Web Store and unpacked extension installation, permissions, updates, and common Manifest V3 extensions.
- [ ] Trace Brave's Manifest V2 exceptions for full uBlock Origin and confirm they can be maintained without proprietary services.
- [ ] Specify a safe, trusted, reliable download, signature, and update path for full uBlock Origin.
- [ ] Implement **first-run only**, optional "Install uBlock Origin" offer with Skip; never silently install or override user choice.
- [ ] Remove competing/duplicate first-run ads, upsells, and promotional messaging.
- [ ] Test install, restart, upgrades, filtering, content scripts, and opt-out behavior on Windows/Linux.

**Gate 3:** Full uBlock Origin is demonstrably installable and updateable in the fork. If not, the blocker must be documented rather than substituted with uBlock Origin Lite without approval.

## Phase 4 — Independent identity and distribution

- [ ] Replace browser name, logos, icons, executable, bundle/package IDs, shortcuts, scheme handlers, crash annotations, profile directories, and About text.
- [ ] Ensure fork **coexists** with standard Brave/Chrome; do not hijack their installation paths, settings, profiles, or update GUIDs.
- [ ] Audit Brave-owned domain calls and make deliberate service-by-service retain/replace/disable choices.
- [ ] Produce reproducible Windows and Linux installers/packages.
- [ ] Define release signing and artifact provenance.

**Gate 4:** Clean install/uninstall, side-by-side usage, first launch, upgrade, and profile migration behavior validated on both targets.

## Phase 5 — Secure maintenance and updates

- [ ] Implement project-owned auto-update mechanism on Windows and Linux, with authenticated release metadata and signed artifacts.
- [ ] Document update channels, failure recovery, rollback plan, and staged release process.
- [ ] Automate upstream Brave/Chromium change notifications and a disciplined integration process.
- [ ] Regression-test security controls, browsing compatibility, privacy defaults, and extensions after every upstream update.
- [ ] Establish an emergency patch release procedure and vulnerability reporting address.

**Gate 5:** Verified update from an older signed build to a newer signed build on both target OSes.

## Phase 6 — Desktop alpha → public release

- [ ] Internal alpha: core workflows, stability, extensions, clean UI.
- [ ] External beta: performance, privacy inspection, compatibility, migration, telemetry/service audit.
- [ ] Accessibility and localization review of changed surfaces.
- [ ] Publish source, licensing/attribution, signing information, release notes, and tested installers.
- [ ] Define sustained maintainer coverage for security releases before recommending it as a daily-use browser.

**Gate 6:** Windows and Linux release checklist passed. Android remains deferred.

## Global rules

1. Never trade security updates for lower merge-conflict volume.
2. Keep `master` aligned with upstream; changes happen in short-lived `fork/*` branches.
3. Prefer targeted build guards and shallow integrations to invasive Chromium changes.
4. Preserve standard extensions; **full** uBlock Origin is an explicit separate milestone, not assumed by the Chromium requirement.
5. When a removal breaks browsing, fix or revert it before starting another feature family.
6. Avoid bundling unrelated feature removal, UI redesign, and branding in a single PR.
