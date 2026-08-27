# Agent skills

Source of truth for reusable agent skills. Each skill is a directory with `SKILL.md` plus optional `references/`. Agents load a skill by reading that directory.

**GitHub description:** Source of truth for reusable agent skills. Each skill is a directory with SKILL.md plus optional references/.

## How to add or enhance a skill

1. Add a new folder at the repo root: `<skill-id>/`.
2. Write `SKILL.md` with YAML frontmatter (`name`, `description` — when to use) plus the recipe steps.
3. Keep recipes generic. Put person-specific IDs in the agent's memory or routines, not in the shared recipe, when you can.
4. Enhance in place via PR. Do not nest skills under `skills/`.

## Catalog

| Skill | When to use | Path |
|---|---|---|
| `full-local-commands` | User must run local git, Xcode, SPM, or rebuild commands — always the full copy-paste recipe | [`full-local-commands/SKILL.md`](full-local-commands/SKILL.md) |
| `mls-listing-input` | A new residential listing needs the Stellar data-entry form filled via Matrix / Wolf Transaction Desk and sent to Coastal listings | [`mls-listing-input/SKILL.md`](mls-listing-input/SKILL.md) |
| `mls-photo-cull` | After a listing photo shoot: cull on Rays Site Media, get Eriq's OK, update Marketing Kit social defaults, then ask Coastal listings to bulk upload | [`mls-photo-cull/SKILL.md`](mls-photo-cull/SKILL.md) |
| `native-mobile-engineer` | Native iOS or Android work. Never ship without a full automated test suite and structured logs | [`native-mobile-engineer/SKILL.md`](native-mobile-engineer/SKILL.md) |
| `prefill-stellar-listing-form` | A new Tampa Bay residential listing needs the Stellar MLS form prefilled from public records, with a client PDF of remaining required fields | [`prefill-stellar-listing-form/SKILL.md`](prefill-stellar-listing-form/SKILL.md) |
| `residential-mls-data-entry` | Tracking a Tampa Bay residential listing from Stellar/Wolf data-entry through Coastal/Matrix until an MLS number is back | [`residential-mls-data-entry/SKILL.md`](residential-mls-data-entry/SKILL.md) |
| `voicedesk-phone-install` | A VoiceDesk walk-ready tip needs to be built and installed on Eriq’s iPhone, or he asks if a new build is on the phone | [`voicedesk-phone-install/SKILL.md`](voicedesk-phone-install/SKILL.md) |
| `voicedesk-verify-without-user` | After a VoiceDesk SHA lands: prove the spoken loop with Linux plus Simulator PCM through the real mic tap | [`voicedesk-verify-without-user/SKILL.md`](voicedesk-verify-without-user/SKILL.md) |

## How an agent loads a skill

1. Read `SKILL.md` first.
2. Then read any files that `SKILL.md` points at (usually under `references/`).
3. Follow that recipe. Do not invent extra rules.

## Native mobile engineer in one line

A build is a claim about reality. Tests are the experiment. Logs are the instrumented timeline.
