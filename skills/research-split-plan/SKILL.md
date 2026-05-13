---
name: research-split-plan
description: Split a research topic, engineering objective, or open-ended investigation into independent subtopics or workstreams and write execution plans. Use when the user wants to decompose a direction into parallel tasks, define ownership, success criteria, artifacts, budgets, dependencies, merge criteria, or WORKTREE_PLAN.md files. This skill does not create Git worktrees by itself; chain with worktree-create for worktree setup and audit-gated-goal for strict Goal Mode stop rules.
---

# Research Split Plan

Use this skill to turn a broad objective into clear, independent workstreams. The output may be a discussion-only split, a central plan, or one plan file per future worktree. Do not create worktrees unless the user also invokes or requests `worktree-create`.

## Workflow

### 1. Confirm The Split Contract

Confirm or infer only what is needed:

- Overall objective and source-of-truth files.
- Number of parallel streams, or time/compute budget if the count is undecided.
- Whether the user wants a written plan, a conversational split, or per-worktree `WORKTREE_PLAN.md` files.
- Expected worktree names or IDs, if worktrees already exist.
- Whether the plan should be audit-ready for `audit-gated-goal`.

If the user already provided enough information, proceed without asking.

### 2. Inspect Current Context

Read the relevant project sources before splitting. For research projects, start from the source-of-truth documents the workspace defines, then use status and ledger documents as needed. Use non-destructive commands when checking repository state:

```bash
git rev-parse --show-toplevel
git rev-parse HEAD
git branch --show-current
git status --short
git worktree list
```

Do not mutate Git state in this skill. If worktree creation is needed, hand off to `worktree-create`.

### 3. Design Independent Workstreams

Each workstream should have minimal overlap with the others. Define:

- Scope and non-scope.
- Claim, hypothesis, subsystem, or user-facing outcome it serves.
- Required artifacts.
- Minimum search, implementation, or experiment budget.
- Expected result files and report path.
- Dependencies on other streams.
- Merge or continuation decision criteria.

Avoid assigning the same primary question to multiple streams unless deliberate replication or robustness checking is the goal.

### 4. Choose Output Layout

For discussion-only use, return the split in the final answer.

For written planning without worktrees, default to:

```text
plans/WORKSTREAMS.md
```

When worktrees already exist or are planned, write one plan per stream:

```text
<worktree-path>/plans/WORKTREE_PLAN.md
```

If worktree paths are not created yet but per-stream plans are useful, write them under the current repository:

```text
plans/<stream-id>/WORKTREE_PLAN.md
```

These files can later be moved or copied into worktrees after `worktree-create`.

### 5. Plan Template

Use this structure for each `WORKTREE_PLAN.md`:

```markdown
# Worktree <ID> Plan: <Title>

**Worktree**: `<name or planned path>`
**Time budget**: <deadline or duration>
**Primary responsibility**: <one sentence>

## 1. Anchor
- Source-of-truth files to read first.
- Main thesis, claim, bug, or outcome this stream serves.

## 2. Skill Policy
- Skills to use for planning, running, monitoring, analysis, and audit.

## 3. Stop Rules
- Whether checklist completion is enough.
- Minimum work before reporting completion.
- Audit readiness criteria if using `audit-gated-goal`.

## 4. Work Items
- Concrete variants, datasets, modules, questions, or implementation tasks.

## 5. Metrics And Artifacts
- Required metrics or evidence.
- Result directories.
- Report path.

## 6. Milestones
- Ordered milestones with decision gates.

## 7. Objective Completion Criteria
- Result-oriented definition of success.

## 8. Final Checklist
- Deliverables to verify before reporting.
```

### 6. Chaining

Use this order for the full workflow:

```text
research-split-plan -> worktree-create -> audit-gated-goal
```

If worktrees already exist, skip `worktree-create`. If the task has precise completion criteria and no exploratory stop risk, skip `audit-gated-goal`.

### 7. Summarize

Report:

- Workstreams created.
- Files written, if any.
- Planned worktree paths, if applicable.
- Dependencies and merge criteria.
- Whether the plan is ready for `worktree-create` or `audit-gated-goal`.
