# Native Mobile Logging

Use this file when adding, reviewing, or using logs to diagnose native iOS or Android issues.

## Goal

If a build fails in the field, an engineer can reconstruct what happened from logs alone — without a repro on their desk and without the user describing every tap.

## What Every Feature Logs

At minimum:

- Operation started (name, trigger, correlation id)
- Decision points that change user-visible state
- Boundary call in and out (API route or method, HTTP status or domain error, duration)
- Persistence write/read failures
- Permission request + result
- Background work start, stop, defer, expire
- Error with typed code, underlying cause, and the UI state shown to the user
- Operation finished (success or failure)

Do not log every frame, every list bind, or every keystroke.

## Structure

Prefer key-value (or JSON) over interpolated prose.

Required fields when available:

- `timestamp` (system logger usually supplies this)
- `level`
- `event` (stable name, dotted or snake, not a sentence)
- `correlation_id` / `request_id` / `session_id`
- `feature` or `screen`
- `outcome` (`ok`, `error`, `cancelled`)
- `error_code` (app-owned, not a raw exception string only)
- `duration_ms` for I/O and user operations

Example event names:

`auth.login.start`
`auth.login.success`
`auth.login.failure`
`listing.sync.start`
`listing.sync.page`
`listing.sync.failure`

Keep event names stable. Treat them like an API.

## Levels

- Debug — high-volume flow useful while developing. Off or sampled in production.
- Info — milestones a human would use to tell the story of a session.
- Error — something failed that a user felt or that needs a fix.
- Fault / Assert — invariant broken. Should be rare and loud.

Do not log expected empty states as errors. An empty inbox is Info or Debug.

## iOS

- Use `Logger` / `os.Logger` (`os` / Unified Logging), not leftover `print`.
- Subsystem = bundle id (or a short app subsystem). Category = feature (`Auth`, `Sync`, `Payments`).
- Signposts (`OSSignposter`) for intervals you will inspect in Instruments.
- Privacy — use public/private interpolations correctly. Default to private. Mark only non-PII as public.
- Crash + breadcrumb — pair with a crash reporter if the project already has one; still keep OSLog so local Console.app works.
- How to read — Console.app, `log stream --predicate`, Xcode Instruments. Document the subsystem and categories in the handoff.

## Android

- Use a thin wrapper (Timber or equivalent) over `Log`, with a production tree that can attach crash/reporting sinks.
- Tag by feature, not by class name soup. Keep tags short and greppable.
- `Log.isLoggable` or build-type gating so Debug noise is not a production flood.
- Pair with Logcat filters documented in the handoff (`tag:Auth`, `correlation_id=`).
- For release, consider a rolling on-device file or reporter breadcrumb buffer so a ticket can attach the last N events. Do not store secrets there.

## Correlation Across Tiers

One user action should produce one `correlation_id` that appears in:

- Client logs
- HTTP header sent to backend (agreed name, e.g. `X-Correlation-Id`)
- Backend logs

If iOS and Android both exist, use the same header and the same event name catalog.

## Privacy and Safety

Never log:

- Access tokens, refresh tokens, passwords, API keys
- Full Authorization headers
- Government IDs, card numbers, precise GPS, raw health data
- Full email bodies or message contents unless the user explicitly asked for a debug build that does so

Redact emails/phones to a hash or last-n if a support workflow truly needs a handle. Prefer opaque user ids.

Assume any log line may end up in a screenshot, a ticket, or a third-party crash tool.

## Performance

- Logging on the hot path must be allocation-light.
- Do not stringify large payloads "just in case."
- Sample chatty events (scroll, location tick, sensor).
- If you need a body for a failing API, log status + error payload size + a truncated error code from the server, not the entire JSON.

## Diagnosis Protocol

When something goes wrong:

1. Collect correlation id, time window, app version, OS version, device class.
2. Pull the timeline of events for that id.
3. Find the first event that is not `ok`.
4. Only then form a hypothesis and write a failing test that encodes it.
5. Fix the cause, not the log line.

A fix without a new or tightened test is incomplete.

## Handoff Log Notes

Include a short "how to trace this feature" block:

- Subsystem / tags / event names
- Where correlation id is created and propagated
- How to export from a device
- What a healthy session looks like vs a failed one (5–10 lines of example log)
