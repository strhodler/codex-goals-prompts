---
name: codex-goals-prompts
description: Use when writing, improving, or reviewing Codex /goal prompts for long-running coding, debugging, migration, optimization, research, or verification work.
---

# Codex Goals Prompts

## Overview

Codex Goals are thread-scoped completion contracts. A strong `/goal` does not ask Codex to "keep going"; it defines the durable objective, evidence that proves completion, constraints that must remain true, and what to do when progress is blocked.

Use this for Goals in Codex CLI, Codex durable objectives, long-running agent tasks, migrations, refactors, flaky tests, benchmark tuning, prompt optimization, research audits, and verification loops.

## When To Use

Use a Goal when the task is larger than one prompt, has a clear finish line, and may need Codex to choose the next action after seeing tests, logs, diffs, benchmarks, or artifacts.

Do not use a Goal for one-line edits, simple explanations, short reviews, unrelated task lists, or vague wishes like "make this better" unless the finish line can be made auditable.

## Goal Contract

Every drafted `/goal` should cover these fields:

| Field | Prompt requirement |
| --- | --- |
| Outcome | What must be true when done |
| Verification surface | Tests, benchmark, report, artifact, logs, or command output that proves it |
| Constraints | Behavior, API, data, UX, security, performance, or docs that must not regress |
| Boundaries | Files, tools, data, branches, services, or resources Codex may use or must avoid |
| Iteration policy | How Codex should pick the next action after each attempt |
| Blocked stop condition | When to stop and what evidence, blocker, and next input to report |

## Template

```text
/goal <desired end state>, verified by <specific evidence>, while preserving <constraints>. Use <allowed inputs, tools, files, data, or boundaries>. Between iterations, <record evidence and choose the next focused action>. If blocked, budget-limited, or no valid path remains, stop with <attempts, evidence, blocker, residual risk, and next input needed>.
```

## Drafting Steps

1. Decide whether this is a Goal or a normal prompt. If the next step is obvious and one-turn, do not use `/goal`.
2. Replace fuzzy phrases like "best practices", "production ready", "fully fix", or "improve" with measurable evidence.
3. Name one objective, not a backlog. If there are multiple unrelated outcomes, split them.
4. Include the exact verification surface when known. If unknown, require Codex to discover and report the relevant command before changing code.
5. Add constraints and boundaries before autonomy. Codex can continue only inside the contract.
6. Add a blocked stop condition. Budget limit, missing data, unavailable commands, or unclear product decisions are not success.

## Strong Example

Weak:

```text
/goal Fix the flaky checkout tests and make the suite pass
```

Strong:

```text
/goal Make the checkout test suite pass reliably on the current branch, verified by reproducing the flaky failure when possible and then running the relevant checkout test command plus the broader test suite if feasible, while preserving existing checkout behavior and test coverage. Use the checkout tests, related production code, logs, and existing project test commands; avoid sleeps, retries, skipped tests, broad timeouts, or loosened assertions unless the assertion is demonstrably wrong. Between iterations, record the failure observed, the smallest focused change made, and the next evidence to collect. If the flake cannot be reproduced, the verification command cannot run, or no defensible path remains, stop with attempted reproductions, evidence gathered, residual uncertainty, and the specific input needed.
```

## Baseline Failures To Prevent

| Failure | Fix |
| --- | --- |
| Writing a good normal prompt without `/goal` | Start with `/goal` and define a durable completion contract |
| Saying "production ready" or "best practices" | Translate into explicit tests, migration safety, failure handling, security, and docs evidence |
| Treating green tests as the only finish line | Also preserve named constraints and report residual risks |
| Letting Codex loop forever | Define a blocked, budget-limited, or no-valid-path stop condition |
| Over-scoping into a backlog | Keep one objective; split unrelated work into separate Goals |

## Red Flags

- The prompt could be answered in one normal turn.
- The end state is subjective: "better", "clean", "modern", "robust".
- No command, artifact, benchmark, report, or source material can prove completion.
- The Goal asks Codex to continue but never says when to stop.
- The Goal allows broad rewrites without boundaries.
- Blockers are framed as failure instead of an honest stopping condition.

## Command Surface Notes

`/goal <objective>` sets the active Goal. `/goal` inspects it. `/goal pause`, `/goal resume`, and `/goal clear` control the lifecycle. Goals are thread-scoped durable state, not global memory or project instructions. Completion must be evidence-based; budget-limited or blocked runs should summarize progress and next inputs, not claim success.
