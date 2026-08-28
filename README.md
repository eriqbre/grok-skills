# grok-skills

Skills for Grok. Each skill is a directory with a `SKILL.md` plus optional `references/`.

## Skills

| Skill | Use when |
|---|---|
| [chief-of-staff](chief-of-staff/SKILL.md) | CoS operating instructions. Triage, routing off Bot, weekly Grok Bot usage discipline. Load first on Bot. |
| [native-mobile-engineer](native-mobile-engineer/SKILL.md) | Native iOS or Android work. Never ship without a full automated test suite and structured logs. |

## How Grok should load this

Point Grok at this repo (`eriqbre/grok-skills`) and tell it which skill to apply.

- On Grok Bot, the Chief of Staff loads `chief-of-staff/SKILL.md` first, then `chief-of-staff/references/bot-usage.md` before any non-trivial job.
- For native iPhone or Android implementation, review, debugging, or build handoff, load `native-mobile-engineer/SKILL.md`, then `native-mobile-engineer/references/`.

## Standing rules

- A build is a claim about reality. Tests are the experiment. Logs are the instrumented timeline.
- Grok Bot weekly usage is a fuel tank. Scope, persist, and route work so the bar lasts the week.
