---
name: VoiceDesk verify without user
description: "Use after a VoiceDesk SHA lands: prove the spoken loop with Linux plus Simulator PCM through the real mic tap. Transcript injects do not count. Do not ask Eriq to walk."
---

# VoiceDesk verify without user

Product bar is spoken conversation: he talks, Eve answers intent, interrupt drops playback and becomes the next turn.

## Default (no user)

1. Confirm SHA.
2. Pull on Mac.
3. Linux: `swift test --package-path VoiceDeskLogic`.
4. `ClientVoiceSpeech` must be write → `playPCM16`, not `synthesizer.speak()`.
5. No `resumeCapture` / `rearmTap` / `audio.start` after TTS.
6. Boot sim.
7. `xcodebuild test` VoiceDesk scheme on iOS Simulator.

Spoken loop is not a phone walk. Must pass XCTest through the real tap callback, not a transcript string.

## Required sim

- One engine start
- Tap rate non-zero during `AVAudioPlayerNode` playback and after drain
- Speech-shaped PCM in that tap is the next turn
- Barge-in drops player buffers; tap still firing

Transcript-only tests do not count.

Report pass/fail + SHA. Do not name a walk SHA until Linux **and** that tap-PCM test are green.

## Phone

Phone only if a real Gmail/Google session or something the sim cannot prove.

Do not ask Eriq to walk first-hear / barge-in. Do not claim hear from Linux phrase tests.
