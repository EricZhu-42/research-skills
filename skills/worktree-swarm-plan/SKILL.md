---
name: worktree-swarm-plan
description: Plan and set up parallel research or engineering sessions across Git worktrees. Use when the user wants to split a long-running task into multiple independent Codex sessions, create or synchronize Git worktrees, keep each worktree aligned to a base commit or branch, verify shared project skills are available, and write per-worktree WORKTREE_PLAN.md files for later execution. Can be chained with audit-gated-goal to also generate strict GOAL_PROMPT.md files.
---

# Worktree Swarm Plan

Use this skill to turn a large objective into several independent worktree sessions with clear ownership, clean starting state, and reusable plan files.

## Workflow

### 1. Confirm The Split

Discuss only the details needed to avoid a bad split:

- Overall objective and source of truth files.
- Number of parallel sessions, or time/compute budget if the number is undecided.
- Desired base branch or commit.
- Worktree parent directory and naming convention.
- Whether existing worktrees should be reused, cleaned, or left untouched.
- Whether goal prompts should also be generated with `audit-gated-goal`.

If the user already provided enough information, proceed without asking.

### 2. Inspect The Repository

Use non-destructive Git commands:

```bash
git rev-parse --show-toplevel
git rev-parse HEAD
git branch --show-current
git status --short
git worktree list
```

Do not run destructive commands such as `git reset --hard` or `git clean` unless the user explicitly asks. If the main worktree is dirty, record that fact and use the selected base commit for new worktrees.

### 3. Design Independent Workstreams

Create workstreams with distinct responsibilities and minimal overlap. For each workstream define:

- Scope and non-scope.
- Claims or success criteria it targets.
- Required artifacts.
- Minimum search or implementation budget.
- Expected result files and report path.
- Dependencies on other workstreams.
- Merge decision criteria.

Avoid assigning the same primary question to multiple worktrees unless deliberate replication is needed.

### 4. Create Or Reuse Worktrees

Prefer names that encode the branch/session role:

```text
<repo>.worktrees/<project>-A
<repo>.worktrees/<project>-B
<repo>.worktrees/<project>-C
```

Typical setup:

```bash
git worktree add -b <branch-name> <worktree-path> <base-sha>
```

If a worktree already exists:

- Inspect `git status --short` and `git rev-parse HEAD`.
- Fast-forward only when safe and requested.
- Preserve existing `plans/` files unless the user asks to rewrite them.
- Do not revert user changes unless explicitly instructed.

### 5. Verify Shared Skills And Context

For each worktree, verify the execution cwd and shared skills/context are available:

```bash
test -f RESEARCH_BLUEPRINT.md || true
test -d .agents/skills || true
rg --files .agents/skills 2>/dev/null | head
```

If project-local skills are missing, report it and choose the least invasive fix: use global skills, copy tracked skill files from the base tree, or ask before symlinking/copying untracked resources.

### 6. Write Worktree Plans

Create one `plans/WORKTREE_PLAN.md` per worktree. Keep it specific enough for an independent Codex session to execute without extra context.

Recommended structure:

```markdown
# Worktree <ID> Plan: <Title>

**Worktree**: `<name>`
**Time budget**: <deadline or duration>
**Primary responsibility**: <one sentence>

## 1. Anchor
- Source-of-truth files to read first.
- Main thesis or claim this worktree serves.

## 2. Skill Policy
- Which skills to use for planning, running, monitoring, analysis, and audit.

## 3. Stop Rules
- State that checklist completion is not sufficient if applicable.
- Define minimum exploration or implementation budget.
- Define audit readiness if using `audit-gated-goal`.

## 4. Work Items
- Concrete variants, datasets, settings, or modules.

## 5. Metrics And Artifacts
- Required metrics.
- Result directories.
- Report path.

## 6. Milestones
- Ordered milestones with decision gates.

## 7. Objective Completion Criteria
- Result-oriented definition of success.

## 8. Final Checklist
- Concrete deliverables to verify before reporting.
```

### 7. Summarize For The User

Report:

- Created or reused worktree paths.
- Branches and base commit.
- Files written.
- Any dirty state intentionally preserved.
- Suggested goal command/prompt for each worktree, or hand off to `audit-gated-goal`.

## Chaining With audit-gated-goal

After writing `WORKTREE_PLAN.md`, use `audit-gated-goal` when the user wants strict stopping rules, external Codex sub-agent review, or long-running sessions that must not stop after shallow progress. The chained output should add `plans/GOAL_PROMPT.md` to each worktree without changing the worktree split unless needed.
