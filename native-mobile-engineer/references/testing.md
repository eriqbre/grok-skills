# Native Mobile Testing

Use this file when designing, writing, or reviewing tests for native iOS or Android.

## Goal

Every product requirement has an automated proof. The suite is the reason a build can be handed over with confidence.

## Test Layers

1. Unit — pure logic, mappers, validators, reducers, use cases. Milliseconds. No UI toolkit, no real I/O.
2. Integration — repositories, API clients, database, keychain/keystore wrappers. Fake or fixture the far side of the boundary.
3. UI / E2E — the smallest set of screen flows that a product owner would accept as "this requirement works."

Do not use UI tests to prove business math. Do not use unit tests to prove a button is tappable.

## Requirement Traceability

For each requirement write one line:

`REQ-ID | behavior in one sentence | test type | test name`

Example:

`REQ-14 | expired session sends user to login and clears token | unit + UI | sessionExpired_clearsTokenAndShowsLogin`

If a requirement cannot be named this way, the requirement is still mush. Tighten it before coding.

## Determinism Rules

- Inject time, UUID, random, locale, and connectivity.
- Replace network with fakes or recorded fixtures. Never hit staging from CI unit/UI suites unless the user explicitly wants a contract test job, isolated from the merge gate.
- No `sleep` to "wait for UI." Wait on conditions, ids, or test schedulers.
- Reset app state between UI tests. One test, one launch story.
- Avoid depending on animation clocks. Disable or control animations in test configuration.

## iOS Toolkit

- Unit / integration — XCTest, or Swift Testing (`@Test`, `#expect`) on current Xcode.
- UI — XCUITest. Prefer accessibility identifiers over brittle label or index queries.
- Async — `async` tests or expectations. Do not spin `RunLoop` by habit.
- Fakes — protocols at the boundary (networking, persistence, keychain, analytics).
- Snapshot / image diffs — only for visual contracts the design actually owns. Treat them as goldens with a review rule, not as a dump of every screen.
- Scheme — a dedicated Test scheme that runs unit + UI on a pinned simulator in CI.

Commands to document in a handoff:

```text
xcodebuild test -scheme App -destination 'platform=iOS Simulator,name=iPhone 16' -resultBundlePath TestResults.xcresult
```

## Android Toolkit

- Unit — JUnit 4/5, MockK or fakes, Turbine for Flow, Coroutines `runTest` + test dispatcher.
- JVM integration — Room (in-memory), DataStore fakes, OkHttp MockWebServer.
- Instrumented — AndroidX Test, Espresso for Views, Compose Test rule for Compose, UI Automator only when crossing app boundaries.
- Robolectric — acceptable for some Android-framework unit tests; do not use it as a substitute for a real-device smoke on critical paths.
- Ids — `testTag` (Compose) or stable resource ids. Do not select by raw text if the copy can change.

Commands to document in a handoff:

```text
./gradlew testDebugUnitTest
./gradlew connectedDebugAndroidTest
```

## What Must Be Tested Per Feature

- Happy path that matches the requirement
- Empty, loading, and error UI
- Auth / session failure
- Offline or timeout
- Permission denied and permission granted (if the feature needs a permission)
- Process death / recreation where state matters
- Deep link or notification entry if that is how users arrive
- Localization-sensitive formatting only when the requirement is about format

## What Not to Test

- Framework internals (RecyclerView layout math, SwiftUI diffing)
- Getters that only return a field
- Third-party SDK internals
- Pixel-perfect animation curves unless the product sells the animation

## CI Gate

Merge and "ready for QA / stakeholder" require:

- Unit + integration green on both platforms touched by the change
- Critical-path UI tests green on at least one pinned simulator / emulator image
- The test matrix checked in or attached to the PR

If only one platform changed, still state what was not run and why.

## Review Checklist

- Can a stranger map each requirement to a test name in under a minute?
- Would a failure message explain the broken user outcome?
- Are tests isolated from device locale, time zone, and leftover accounts?
- Did anyone add a test that only mirrors the implementation line-for-line?
- Are UI tests fewer than the unit tests, and are they the ones that protect revenue or trust paths?
