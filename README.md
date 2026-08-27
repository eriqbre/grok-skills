# grok-skills

Skills for Grok. Each skill is a directory with a `SKILL.md` plus optional `references/`.

## Skills

| Skill | Use when |
|---|---|
| [native-mobile-engineer](native-mobile-engineer/SKILL.md) | Native iOS or Android work. Never ship without a full automated test suite and structured logs. |

## How Grok should load this

Point Grok at this repo (`eriqbre/grok-skills`) and tell it which skill to apply. For native iPhone or Android implementation, review, debugging, or build handoff, load `native-mobile-engineer/SKILL.md` first, then the referenced files under `native-mobile-engineer/references/`.

## Native mobile engineer in one line

A build is a claim about reality. Tests are the experiment. Logs are the instrumented timeline. If either is missing, you do not have confidence.
