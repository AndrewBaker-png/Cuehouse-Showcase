# Testing and validation

Cuehouse uses automated tests with controlled media fixtures and simulated server responses to exercise viewing workflows and failure handling without publishing a personal media library.

This page summarizes **historical results recorded in retained project reports**. The reports and relevant test assertions were reviewed on November 1, 2026. No tests were rerun for this showcase update. The October 3 temporary Xcode result bundles are no longer available, so their counts are attributed to the retained report rather than presented as independently re-extracted results.

## Selected recorded results

| Run date | Scope and environment | Recorded outcome |
| --- | --- | --- |
| October 3, 2026 | Complete iOS unit target on an iPad Pro 13-inch (M5), iOS 27 simulator, after playback-validation fixes | 1,494 passed; zero failed or skipped |
| October 3, 2026 | Complete tvOS unit target on an Apple TV 4K, tvOS 27 simulator, after the same fixes | 176 passed; zero failed or skipped |
| October 3, 2026 | Four selected iPad UI tests in the audit baseline | Four passed; zero failed or skipped |
| September 25, 2026 | iPad compatibility-window large-library grid and list scrolling journeys | Both passed; zero failed or skipped |

These are separate historical runs and scopes, not one combined test total or a statement that the current revision passes every test. The iPad compatibility-window checks predate the October native iPad UI coverage.

## What the tests help demonstrate

### Playback metadata and failure handling

Controlled server responses exercise the boundary between selecting a title and resolving its playback source. The October report records eleven regression tests shared by the iOS and tvOS targets, included in the passing follow-up suites.

Reviewed cases check that:

- Metadata for a different title or conflicting library is rejected before playback-source selection or track changes.
- Empty, malformed, multiple-item, and wrong-type responses do not silently reuse old metadata.
- Unauthorized and forbidden responses remain failures even when the input already contains detailed media information.
- Selected temporary connectivity failures can reuse an already-known media layout, while cancellation and invalid requests remain failures.
- A permission change during a request invalidates both fresh and fallback results.
- Download-source resolution applies the same item-identity checks.

**Why it matters:** a viewing workflow needs predictable failure behavior as well as a successful playback path. These fixtures test decision logic; they do not establish successful live streaming or constitute a security certification.

### iPad navigation and layout

The four recorded UI tests cover:

- Moving between library, downloads, and settings with keyboard shortcuts while preserving the library navigation stack.
- Keeping Play and Back controls reachable across rotation and constrained-width detail layouts.
- Switching tabs and rotating with an empty library.
- Pausing playback with the keyboard and closing a player panel before closing the player itself.

**Why it matters:** these are concrete checks of navigation continuity and control availability under scripted conditions. They do not establish usability across every physical keyboard, iPad, or window configuration.

### Large-library navigation

The September report records passing grid and list scrolling journeys in an iPad compatibility window. It also documents a 520-item TV fixture and a test-isolation issue: that fixture shared cached metadata with a smaller fixture. Giving the fixtures distinct server identities resolved the interference, and the original navigation assertions then passed.

**Why it matters:** the testing process includes checking that synthetic datasets remain isolated and that navigation assertions continue to hold. These results are functional checks, not a frame-rate, memory-use, or maximum-library-size benchmark.

## Evidence and limits

Sources reviewed in the private development repository:

- October 3, 2026 codebase audit and playback-validation follow-up.
- September 25, 2026 reconciliation and validation report.
- Playback metadata validation tests and the four corresponding iPad UI test implementations.

The retained reports state that executed counts were checked against Xcode result bundles at the time. Current test source was used to understand assertions, not to infer that newly added tests passed in older runs.

These results do not establish broad device compatibility, current-release readiness, sustained live Plex playback, physical Picture in Picture/AirPlay/HDR behavior, or minimum-supported-OS coverage. No personal library data, credentials, raw logs, or application/test source code is included in this public summary.

[Back to Cuehouse](README.md)
