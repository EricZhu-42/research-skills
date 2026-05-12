---
name: audit-gated-goal
description: Create Goal Prompt files for exploratory improvement tasks where the user has a difficult, directional, or weakly quantified objective rather than a precise checklist. Use when Codex Goal Mode should keep improving a research direction, method, benchmark, setting, or open-ended engineering target for a time budget, and may stop early only after external Codex sub-agent audit confirms substantive progress. Do not use for clear one-shot tasks such as running a specified experiment, fixing a known bug, or producing a fixed artifact.
---

# Audit-Gated Goal

Use this skill when the user does not have a fully precise target, but wants an agent to explore and improve a difficult direction for a fixed period. The prompt should prevent early stopping after shallow attempts by requiring an external audit before the goal can end early.

## Workflow

### 1. Confirm The Exploration Contract

Confirm or infer:

- Directional objective and source-of-truth files.
- Why the goal is exploratory rather than a fixed checklist.
- Time budget or stop deadline.
- Result-oriented completion criteria.
- Minimum work required before requesting audit.
- Required external auditor type. Default: Codex sub-agent via `spawn_agent`.
- Output path, usually `plans/GOAL_PROMPT.md`.

If the task has a precise, finite target, do not force this skill. Use a normal implementation or experiment prompt instead.

### 2. Separate Plan From Prompt

Keep the prompt concise:

- Put task-specific experiment matrix, dataset choices, method details, and metrics in `WORKTREE_PLAN.md`.
- Put execution principles, stop rules, audit cadence, and auditor prompt in `GOAL_PROMPT.md`.
- `GOAL_PROMPT.md` must be directly copyable by the user into Goal Mode. Do not start it with a title or heading.
- `GOAL_PROMPT.md` must be no more than 4000 characters. In normal cases, target 1000-2000 characters.

This keeps the active goal context small while preserving detailed plans locally.

### 3. Use Result-Oriented Stop Rules

Do not let attempts substitute for success. The prompt should state:

- Checklist completion only allows requesting audit; it is not completion.
- Failure, weak results, local self-audit, and negative/inconclusive findings are not early-stop reasons.
- Early stop requires external audit verdict `PASS_STOP`.
- Any other verdict, including ambiguity, means continue.
- At the time budget, the session may stop and report honestly even without `PASS_STOP`.

Use practical effect-size language when relevant. For ML experiments, say that tiny absolute changes around `0.01` are not strong evidence by themselves; prefer clearly visible effects such as `0.03+` absolute on key metrics or decisive multi-metric practical gains.

### 4. Add Audit Cadence

Prevent audit spam:

- Define an Audit Readiness Gate in the plan or prompt.
- Require substantial new work before re-audit after `FAIL_CONTINUE`.
- For long sessions, use a minimum interval such as 90 minutes plus at least 3 new substantive steps.

### 5. Write The External Audit Prompt

Embed a short auditor prompt. It should require the auditor to read the source-of-truth files, the plan, and new artifacts.

Template:

```text
You are an external auditor for <workstream>.

Read:
- <source-of-truth files>
- plans/WORKTREE_PLAN.md
- all new reports/results/code relevant to this task

Decide whether this exploratory goal may stop before <deadline>.

Return PASS_STOP only if the work achieves the result-oriented Objective Completion Criteria in plans/WORKTREE_PLAN.md and makes substantive progress on the assigned direction. Attempts, shallow aggregation, checklist completion, local self-audit, weak negative results, or tiny metric changes are not enough.

Return exactly one verdict:
- PASS_STOP
- FAIL_CONTINUE

If FAIL_CONTINUE, give the next 3 concrete steps.
```

Require saving the audit result to a stable path such as:

```text
reports/<workstream>_external_audit.md
```

### 6. Write The Goal Prompt

Recommended `GOAL_PROMPT.md` structure. Do not include a leading title; start directly with the executable instruction. Keep the whole prompt under 4000 characters, and normally 1000-2000 characters:

```markdown
Execute `plans/WORKTREE_PLAN.md`. The plan defines the exploratory work; this file defines execution and stopping rules.

## Execution Principles

1. Read <source-of-truth files> and `plans/WORKTREE_PLAN.md`.
2. Use relevant skills for planning, running, monitoring, analysis, and audit.
3. Do not stop after shallow attempts, old-result aggregation, baseline reproduction, or local self-audit.
4. If an attempt fails, continue by changing method, setting, data, or hypothesis. Failure is not a stop reason.

## Stop Rules

Run until <deadline>. Before then, stop only if a Codex sub-agent external audit returns `PASS_STOP`.

`PASS_STOP` must be result-oriented: the work must satisfy `plans/WORKTREE_PLAN.md` Objective Completion Criteria and make substantive progress on the assigned exploratory direction. Checklist completion, tests passing, local self-audit, negative/inconclusive results, or tiny metric changes are not enough.

Do not audit frequently. First audit requires the Audit Readiness Gate. After a failed audit, complete at least <N> substantive new steps and continue for at least <duration> before re-auditing.

## External Audit

Use `spawn_agent` with this prompt and save the result to <audit-report-path>:

<auditor prompt>

Final answer must state stop reason, audit verdict, key changes, key metrics, claim impact, and merge/retry recommendation.
```

## Standalone And Chained Use

Standalone use: create or update one `GOAL_PROMPT.md` for one exploratory direction.

Chained use after `worktree-swarm-plan`: for every generated worktree, read its `WORKTREE_PLAN.md`, create a matching `GOAL_PROMPT.md`, and ensure the prompt refers to that worktree's artifacts and audit report path.
