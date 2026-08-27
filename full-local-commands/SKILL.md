---
name: Full local commands
description: Use whenever the user must run local git, Xcode, SPM, or rebuild commands — always the full multi-line copy-paste recipe, never a shortened subset.
---

# Full local commands

Use this whenever the user needs to run something on their machine (git checkout, rebuild, package resolve, install, dogfood walk).

## Rule

Always send the complete copy-pasteable command sequence. Never a shortened subset, never a one-liner that omits steps they have needed before.

## Required shape

A fenced bash block with each step on its own line:

1. `cd` to the project
2. `git status`
3. `git stash` only if dirty (comment: skip if clean)
4. `git fetch origin`
5. `git checkout <branch>`
6. `git pull --ff-only origin <branch>`
7. `git log -1 --oneline` with the expected SHA in a comment
8. Clean the caches this project has needed (DerivedData, SPM, `.swiftpm`, etc.)
9. Resolve packages / install deps
10. Open the project
11. After the block: wait conditions then build/run
12. A short walk / verify list

If a step is optional, keep it in the block with a comment.

```bash
cd <project>
git status
git stash   # skip if clean
git fetch origin
git checkout <branch>
git pull --ff-only origin <branch>
git log -1 --oneline   # expect <SHA>
# Clean the caches this project has needed (DerivedData, SPM, .swiftpm, etc.)
# Resolve packages / install deps
# Open the project
```

After the block: wait conditions, then build/run. Then a short walk / verify list.

## Do not

- Send only `git checkout` + `git pull`
- Collapse into `&&` one-liners
- Assume they remember SPM / cache wipes
