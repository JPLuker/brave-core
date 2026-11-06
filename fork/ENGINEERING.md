# Engineering, builds, security and releases

## Repository policy

- Upstream: `https://github.com/brave/brave-core.git`
- Fork: `https://github.com/JPLuker/brave-core.git`
- Keep `master` as a low-drift upstream reference.
- Develop on `fork/*` branches and merge focused reviewed PRs into the fork's integration branch. **Never aim our fork-only PRs at `brave/brave-core`.**
- Separate baseline/build work, removals, extension support, rebranding, packaging and update logic.
- Do not use upstream Brave signing keys, production update identifiers, or private infrastructure.

## Developer checkout (not yet run for this fork)

The upstream README specifies that `brave-core` must be cloned into `src/brave`, beneath an existing project folder, then initialized. Example with HTTPS and an isolated workspace:

```bash
mkdir -p browser-workspace/src
git clone https://github.com/JPLuker/brave-core.git browser-workspace/src/brave
cd browser-workspace/src/brave
git checkout fork/foundation

# Install prerequisite Node and pnpm versions first.
pnpm run init       # Downloads a substantial Chromium and dependency checkout.
pnpm run build      # Upstream's component build.
pnpm start          # Run local build.
```

Do **not** run these commands inside an existing Brave or Chromium workspace without checking its remotes and local changes. Initialization and synchronization can alter checked-out dependencies. Start in a new isolated folder.

Current `package.json` snapshot (foundation date): Node `>=24.16.0 <25.0.0`, pnpm `>=11.11.0`, Brave Core `1.101.33`, Chromium `157.0.8103.39`. Follow upstream platform toolchain documentation and recheck versions before actual build. Full checkout/compilation has substantial disk and memory requirements.

The full build is **not verified** by GitHub-only edits. Do not report compiled artifacts or test results without actually running them.

## Build strategy

1. Pin an upstream commit/tag and record toolchain, build flags, OS, architecture and actual resource usage.
2. Capture a completely unmodified baseline binary and smoke-test core browsing/extension behavior.
3. Prefer existing GN args or build guards when disabling feature families. Avoid globally defining flags that conflict with upstream versions.
4. Build and run relevant native/JS tests after each family. Review `docs/best_practices.md` and specific upstream guidelines when editing code.
5. Add integration smoke tests for startup, core navigation, permissions, private windows, profiles, Chrome-compatible extensions and full uBlock Origin behavior.
6. Use GitHub Actions only for checks its runners can sustainably support; a complete Chromium build may require dedicated machines or substantially larger runners.

## Extension policy

- Chrome-compatible extensions are a **product requirement**, not just a checkbox in documentation.
- Manifest V3 support, ordinary Chrome Web Store installation, and unpacked extension behavior must be verified.
- Full uBlock Origin requires separate verification of Brave's Manifest V2 support, store/distribution availability, install signing and extension auto-updates.
- The first-run prompt must have **Install** and **Skip**, run only when appropriate, respect choice, and never silently sideload an extension.
- Do not depend on Brave-hosted extension/update endpoints without verifying availability and permission for this independent fork.
- No substitution with uBlock Origin Lite without a deliberate user decision.

## Upstream synchronization

1. Keep `master` aligned with `brave/brave-core` (do not add fork changes directly to `master`).
2. Integrate upstream commits into a short-lived fork branch.
3. Re-run dependency sync only after reviewing upstream changes and committing local work. Do not use `--force` blindly.
4. Run compilation and regression checks; treat Chrome/Brave security releases as priority changes.
5. Merge only after validation; publish fork release notes identifying upstream version/commit.
6. Automate notifications/checklists first, not blind merges or automatic public releases.

## Product identity and updater

Before shipping public binaries:
- Create original, non-Brave branding, product/app IDs, installer GUIDs, package names, shortcuts, Linux desktop entries, and profile directories.
- Avoid overwriting installations or user profiles of Chrome/Brave.
- Ensure any retained network services have clear purpose and privacy policy; remove dependencies on Brave business infrastructure.
- Build separate Windows and Linux release/signing flows with authenticated update metadata, rollback and error recovery.
- Maintain source availability and attribution required by MPL 2.0 and other third-party licenses.

## Release blocking conditions

- Cannot build and smoke-test on target OS.
- Missing upstream security patches or unreviewed changes to trust/security boundaries.
- Extensions fail, including the stated full uBlock Origin capability.
- Browser points at Brave product update identifiers or a nonexistent fork updater.
- Old Brave branding / installer IDs / profile path conflicts remain.
- Feature is only hidden in UI while sending unwanted background requests.
- No repeatable, verified update path for deployed users.

## Current engineering status

This branch contains **planning and audit documents only**. No browser source removal, branding modifications, installer, automated update system or successful build is claimed.
