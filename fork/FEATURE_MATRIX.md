# Feature policy and initial source inventory

**This is a planning inventory, not an assertion that features have already been removed.** Areas below were observed in the fork's source tree or existing build configuration. Exact dependencies must be verified before implementation.

| Product area | Decision | Initial source leads / notes |
| --- | --- | --- |
| Chromium engine, tabs, navigation, downloads, history, bookmarks, profiles, settings, devtools | **KEEP** | Preserve normal Chromium behavior and update paths |
| Chrome-compatible extensions (MV3 and permitted MV2) | **KEEP — mandatory** | `extensions/`, `browser/extensions/`, Chromium extension interfaces |
| Full uBlock Origin | **ADD optional offer; verify support** | Determine trusted install/update source; Brave MV2 exception is not sufficient alone |
| Brave Shields, fingerprinting/tracker protection | **KEEP pending compatibility tests** | `components/brave_shields/`, `browser/brave_shields/`; do not accidentally remove the adblock engine |
| HTTPS/security and Safe Browsing behavior | **KEEP / verify** | Must remain secure after cleanup and debranding |
| Passwords, autofill, browser permissions | **KEEP** | Core browsing functionality |
| Brave Rewards / BAT | **REMOVE** | `components/brave_rewards/`, `browser/brave_rewards/` |
| Brave Ads and sponsored content | **REMOVE** | `components/brave_ads/`, `ui/brave_ads/`, `browser/brave_ads/`, New Tab promo dependencies |
| Brave Wallet / crypto / Web3 integrations | **REMOVE** | `components/brave_wallet/`, `components/brave_wallet_ui/`, `browser/brave_wallet/` |
| Leo AI/chat | **REMOVE** | `components/ai_chat/`, `browser/ai_chat/` |
| Brave News | **REMOVE** | `components/brave_news/`, `browser/brave_news/` |
| Brave VPN / upsells | **REMOVE** | `components/brave_vpn/`, `browser/brave_vpn/` |
| Brave Talk marketing/integration | **REMOVE** | `components/brave_talk/` |
| Promotional onboarding, referral features | **REMOVE/replace** | `components/brave_welcome/`, `components/brave_referrals/` |
| New Tab page | **KEEP functional, remove ads/promotions** | `components/brave_new_tab_ui/`; retain useful search/navigation |
| Brave-specific telemetry, P3A, analytics, background network services | **AUDIT / disable unless justified** | `browser/brave_browser_main_extra_parts_p3a.cc`; verify actual requests and defaults |
| Brave Search default and associated promotion | **DECIDE after audit** | User should be able to choose search provider; no forced promotion |
| Brave Sync and account dependencies | **INVESTIGATE** | Existing sync may depend on Brave-owned infrastructure; do not promise a fork-owned sync service |
| Brave Origin | **REFERENCE ONLY** | `components/brave_origin/`, `browser/brave_origin/`, `is_brave_origin_branded` flag; do not ship Brave Origin branding/IDs or paid subscription plumbing |
| Brave service domains, keys, updater | **REPLACE/AUDIT** | Build/runtime calls, signing, update endpoints and application identity must belong to this fork before release |

## Definition of removed

The feature must not be reachable through the UI, menus, preferences, onboarding, background tasks, URL handlers, or its disabled service integration. **Do not delete shared libraries solely because a feature imports them.** Remove or guard entry points first, verify dependent browser paths, and only then prune unreachable code.

## Audit method for each removal family

1. Locate source entry points and build flag(s).
2. Trace start-up registration, profile services, prefs, menus, WebUI routes, tests, networking, and build dependencies.
3. Make one small coherent change and build.
4. Test clean install, upgrade from previous fork build (once available), normal and private windows, relevant settings, and affected extension scenarios.
5. Record what is truly removed versus merely disabled, including any remaining remote calls.

## Open decisions to resolve after baseline

- Final browser name and visual assets.
- Which search engine to ship as default.
- Whether Brave Shields and full uBlock Origin both stay enabled by default, or one yields to the other to avoid duplication.
- Whether any Brave-hosted services (notably Sync) are supportable and appropriate for this fork.
- Extension installation/update distribution design for Manifest V2.
