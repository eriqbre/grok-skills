# Grok Bot usage discipline

Load this when planning work, when the weekly bar is moving fast, or before any job that will take more than a few tool calls.

Exact weekly token numbers are unpublished. Do not invent them. Usage is agent steps plus tokens. Long jobs, fat context, and retry loops are the dominant cost.

## Physics

- Inference costs energy. Tokens are the billable unit of that energy.
- A message is cheap. A 40-step agent that re-reads a repo and retries a failed install is not.
- Context you load is paid for again when it is still sitting in the window on the next step.
- The Bot VM is persistent. Disk and memory are cheaper than re-discovering the same tree every session.
- Cursor monthly pools, grok.com weekly pool, and Bot weekly pool do not refill each other.

## Pre-flight (required on any non-trivial job)

Write these four lines before tools:

1. Outcome
2. Success check (command, test, file content, or human decision)
3. Inputs you will read (paths/URLs, not "the codebase")
4. Kill condition (e.g. two failed installs, missing login, success check already green)

If you cannot write those four lines, the job is not scoped. Ask Eric one question or drop it.

## Context hygiene

- `ls` / `find` / `rg` with a tight pattern before `cat`.
- Read the function or file slice you need. Do not slurp 2k-line files "for awareness."
- After you know the layout, stop listing it.
- Prefer checksums and `git diff --stat` over pasting full diffs into chat.
- Put large outputs on disk. Reply with the path and the last 20 relevant lines.
- Do not attach screenshots, video, or whole build logs to the conversation unless Eric asked for that artifact.

## Tool-loop hygiene

- Same command failed twice → change the hypothesis or stop. Do not third-try with a paraphrased prompt.
- Package install, brew, apt, npm, gradle, cocoapods → run once, log the result to a file, reuse.
- Tests are the oracle. Run the smallest test that can falsify the change. Do not start a new reasoning novel when the test is red; read the failure.
- Do not open extra browsers or extra repos "in case."
- Scheduled routines must have a max runtime and a single output path. No open-ended "keep improving X."

## Persistence on the VM

Maintain a short durable notebook (create if missing):

```text
~/MEMORY.md
```

Store only facts that save future tokens:

- Active repos and local paths
- Logins already completed (never store passwords or tokens in plaintext if a keychain/secret file exists)
- Decisions Eric already made
- Current P0 and the success check
- Known landmines (flaky test, broken env, rate limits)

At session start, read MEMORY.md before exploring. At session end, append what changed in a few bullets. Cap the file. Archive old weeks to `~/memory/YYYY-MM-DD.md` rather than letting MEMORY.md grow without bound (a huge memory file becomes its own token tax).

## Batching

- Combine related lookups into one pass.
- Do not start five tiny jobs that each reload the same repo.
- If Eric sends a pile of asks, rank them, propose a batch of one or two, and park the rest.
- Status updates to Eric should be aggregated, not a ping per tool call.

## What never belongs on Bot this week unless Eric overrides

- Re-summarizing a document already summarized in MEMORY.md
- Exploratory "see what else we could build" with no success check
- Re-implementing a feature that already has a passing test
- Polishing prose, renaming for taste, or drive-by refactors
- Multi-model bake-offs
- Pulling the entire monorepo into context to answer a one-file question

## When the bar is high

If usage is already elevated or Eric said he hit the weekly limit:

1. Checkpoint in-flight work to disk.
2. Write a one-screen handoff. What is done, exact next command, blockers.
3. Do not start new research.
4. Tell Eric the next cheapest surface for the leftover work (Cursor inner loop, Grok Build, wait for weekly reset, or a capped on-demand dollar amount he sets).
5. Do not improvise a plan-upgrade pitch.

## Measurement (so this skill is falsifiable)

After a job, log one line to `~/usage-log.md`:

```text
YYYY-MM-DD job | outcome | tool-calls-approx | files-read | budget-risk low/med/high | note
```

If two consecutive jobs are high risk for little outcome, tighten scope further. The CoS is failing the brief if the log is all high-risk exploration.
