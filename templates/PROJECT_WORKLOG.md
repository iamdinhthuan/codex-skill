# Work Log

This file is durable project memory for future Codex conversations. Keep it
short, factual, and current. Update it whenever a meaningful task starts,
finishes, changes direction, or leaves follow-up work.

## Status Legend

- `Done` - implemented and verified, or intentionally completed with notes.
- `In Progress` - currently being worked on in this branch/session.
- `Pending` - known next work that has not started.
- `Blocked` - cannot continue without a missing dependency, credential, answer,
  or external action.
- `Skipped` - intentionally not done; include the reason.

## Current State

- Active branch: `[fill when work starts]`
- Open PR: `[none yet]`
- Base branch: `[fill when known]`
- Unrelated worktree changes: `[none observed]`

## Tasks

| Status | Task | Notes | Verification |
| --- | --- | --- | --- |
| In Progress | Bootstrap project work log | Created this file so future sessions can inspect task status and decisions. | Pending first task update. |

## Decisions

- Use project-level `AGENTS.md` only for project-specific rules, commands,
  architecture constraints, or domain context.
- Use global `~/.codex/AGENTS.md` for shared Codex behavior, team workflow, and
  global skill triggers.
- Keep unrelated worktree changes out of commits unless the user explicitly
  includes them in scope.
- For multi-agent work, use structured handoffs with one status label
  (`[DONE]`, `[BLOCKED]`, `[HANDOFF]`, or `[NOOP]`), changed files,
  verification, blockers or risks, and the next coordinator action.

## How To Update This Log

When starting a task:

1. Add or update a row with `In Progress`.
2. Include the branch, PR, or affected files when useful.
3. State the intended verification before implementation if the task is risky.

When finishing a task:

1. Change status to `Done`, `Skipped`, or `Blocked`.
2. Add the commit, PR, command output, or manual check that proves the outcome.
3. Note any remaining risk or unrelated worktree change.

Keep entries concise. This file is context for future agents, not a full chat
transcript.
