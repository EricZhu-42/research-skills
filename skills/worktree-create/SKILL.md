---
name: worktree-create
description: Create or reuse Git worktrees for research or engineering sessions without decomposing the task. Use when the user asks to create, set up, inspect, or prepare a worktree from a branch or commit, choose a worktree path and branch name, verify clean starting state, or give a compact manual-work setup. Does not split research objectives or write detailed WORKTREE_PLAN.md files unless explicitly requested; chain with research-split-plan when planning is needed.
---

# Worktree Create

Use this skill for the concrete Git and filesystem part of preparing one or more worktrees. Keep it small: create or reuse the worktree, verify it, and report the path. Do not design a research split unless the user asks for planning too.

## Workflow

### 1. Confirm Only Missing Essentials

Infer defaults when safe. Ask only if a wrong choice would be costly:

- Repository root.
- Base branch or commit. Default: current `HEAD`.
- Worktree path and branch name.
- Whether to create a new branch, use an existing branch, or reuse an existing worktree.
- Whether project-local skills or context files need to be checked.

For a quick manual setup request, prefer a concise default such as a sibling directory under `<repo>.worktrees/`.

### 2. Inspect Non-Destructively

Use read-only Git commands first:

```bash
git rev-parse --show-toplevel
git rev-parse HEAD
git branch --show-current
git status --short
git worktree list
git branch --list <branch-name>
```

If the main worktree is dirty, record that fact and still use the selected base commit. Do not copy uncommitted changes unless the user explicitly asks.

### 3. Create Or Reuse The Worktree

Typical new branch setup:

```bash
git worktree add -b <branch-name> <worktree-path> <base-ref>
```

If the branch already exists and is not checked out elsewhere:

```bash
git worktree add <worktree-path> <branch-name>
```

If the worktree path already exists:

- Inspect `git -C <path> status --short` and `git -C <path> rev-parse HEAD`.
- Reuse it only when it matches the user request.
- Preserve existing files and plans.
- Do not run `git reset --hard`, `git clean`, or checkout over user changes unless explicitly requested.

### 4. Verify The New Session

Run a compact verification:

```bash
git -C <worktree-path> rev-parse --show-toplevel
git -C <worktree-path> rev-parse HEAD
git -C <worktree-path> status --short
test -f <worktree-path>/RESEARCH_BLUEPRINT.md || true
test -d <worktree-path>/.agents/skills || true
```

If project-local skills are required but missing, report it and choose the least invasive fix only if requested: use global skills, copy tracked skill files, or create symlinks.

### 5. Optional Hand-Off

If the user also wants a task split or plan files, use `research-split-plan` after the worktree exists. If the user wants strict long-running stop rules, use `audit-gated-goal` after a plan exists.

### 6. Report

Summarize only what matters:

- Worktree path.
- Branch and base commit.
- Whether it was created or reused.
- Dirty state intentionally preserved.
- Any missing context or skill availability issue.
