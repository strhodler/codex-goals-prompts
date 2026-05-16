# Codex Goals Prompts

[![skills.sh](https://skills.sh/b/strhodler/codex-goals-prompts)](https://skills.sh/strhodler/codex-goals-prompts)

A practical skill for writing strong Codex `/goal` prompts.

Codex Goals are useful when a task should keep progressing across turns until an evidence-based stopping condition is met. This skill helps turn vague long-running requests into durable completion contracts with measurable outcomes, verification surfaces, constraints, boundaries, iteration policy, and blocked stop conditions.

## Why Use This Skill

Most weak Goals fail in predictable ways: they say "keep going", "make it production ready", or "fix everything" without defining what completion means. That makes Codex more likely to stop too early, drift into unrelated work, or claim success without evidence.

This skill helps agents draft Goals that answer:

- What should be true when the work is done?
- What evidence proves the result?
- What must not regress?
- What files, tools, or data may Codex use?
- How should Codex choose the next step after each attempt?
- When should Codex stop and report a blocker instead of continuing?

## Best For

- Code migrations and large refactors
- Flaky test investigations
- Benchmark-driven optimization
- Prompt and eval optimization loops
- Debugging tasks with uncertain next steps
- Research or audit work that needs evidence-backed conclusions
- Long-running Codex CLI tasks where `/goal` is a better fit than a normal prompt

## Not For

- One-line edits
- Simple explanations
- Short code reviews
- Unrelated task lists
- Goals with no test, benchmark, artifact, report, log, or source material that can prove completion

## Install

After publishing this repository on GitHub, install it with the `skills` CLI:

```bash
npx skills add strhodler/codex-goals-prompts
```

> [!NOTE]
> Skills appear on skills.sh automatically after users install them with `npx skills add <owner>/<repo>`. There is no separate upload command.

## Usage

Ask your coding agent to use the skill whenever you need to write, review, or improve a Codex Goal prompt.

Example request:

```text
Use the codex-goals-prompts skill to turn this into a strong /goal:
I want Codex to keep working on our flaky checkout tests until it either fixes them or can explain what blocks progress.
```

Expected style of output:

```text
/goal Make the checkout test suite pass reliably on the current branch, verified by reproducing the flaky failure when possible and then running the relevant checkout test command plus the broader test suite if feasible, while preserving existing checkout behavior and test coverage. Use the checkout tests, related production code, logs, and existing project test commands; avoid sleeps, retries, skipped tests, broad timeouts, or loosened assertions unless the assertion is demonstrably wrong. Between iterations, record the failure observed, the smallest focused change made, and the next evidence to collect. If the flake cannot be reproduced, the verification command cannot run, or no defensible path remains, stop with attempted reproductions, evidence gathered, residual uncertainty, and the specific input needed.
```

## Goal Template

```text
/goal <desired end state>, verified by <specific evidence>, while preserving <constraints>. Use <allowed inputs, tools, files, data, or boundaries>. Between iterations, <record evidence and choose the next focused action>. If blocked, budget-limited, or no valid path remains, stop with <attempts, evidence, blocker, residual risk, and next input needed>.
```

## What The Skill Teaches

| Goal field | Purpose |
| --- | --- |
| Outcome | Defines what must be true when done |
| Verification surface | Names the tests, benchmark, report, artifact, logs, or command output that prove completion |
| Constraints | Protects behavior, APIs, data, UX, security, performance, or docs from regression |
| Boundaries | Limits files, tools, data, branches, services, or resources Codex may use |
| Iteration policy | Tells Codex how to choose the next focused action after each attempt |
| Blocked stop condition | Prevents false success when budget, missing data, or unclear requirements block progress |

## Example Transformations

Weak:

```text
/goal Improve performance
```

Strong:

```text
/goal Reduce p95 checkout latency below 120 ms, verified by the checkout benchmark, while keeping the correctness suite green. Use only checkout service code, benchmark fixtures, and related tests. Between iterations, record what changed, what the benchmark showed, and the next focused experiment. If the benchmark cannot run or no valid paths remain, stop with attempted paths, evidence gathered, blocker, and next input needed.
```

Weak:

```text
/goal Make the database layer production ready
```

Strong:

```text
/goal Migrate the database layer so existing persistence behavior remains correct and safe, verified by focused migration tests and the broader affected test suite when feasible, while preserving data semantics, public repository contracts, transaction behavior, error handling, and security guarantees. Use existing schema, DAO/repository code, migration files, tests, docs, and build commands; avoid unrelated UI, networking, or feature changes. Between iterations, record evidence observed, the smallest focused change made, and the next verification target. If the target architecture, migration path, or verification command cannot be determined, stop with attempts made, evidence gathered, blocker, residual risk, and the decision needed.
```

## Repository Structure

```text
.
├── README.md
└── SKILL.md
```

`SKILL.md` contains the actual agent-facing instructions. `README.md` explains the purpose, installation, and examples for humans browsing GitHub or skills.sh.

## Publishing To skills.sh

1. Create a public GitHub repository, for example `codex-goals-prompts`.
2. Add `SKILL.md` and `README.md` to the repository root.
3. Push the repository to GitHub.
4. Install it once with:

```bash
npx skills add strhodler/codex-goals-prompts
```

After installation telemetry is collected, skills.sh can list the skill automatically and rank it by aggregate installs.

## Official Documentation

- [OpenAI Codex: Follow a goal](https://developers.openai.com/codex/use-cases/follow-goals)

## Credits

This skill is based on OpenAI Codex Goals concepts: durable objectives, evidence-based completion, scoped constraints, lifecycle controls, and honest blocked stop conditions.
