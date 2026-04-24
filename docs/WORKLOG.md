# Work Log

This file is the durable project memory for future Codex conversations. Keep it
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

- Active branch: `codex/add-stitch-skills`
- Open PR: https://github.com/iamdinhthuan/codex-skill/pull/1
- Base branch: `main`
- Unrelated worktree change: `SKILL.md` is deleted but intentionally not staged
  or included in the Stitch PR.

## Tasks

| Status | Task | Notes | Verification |
| --- | --- | --- | --- |
| Done | Add Google Stitch skills to the repo | Vendored upstream `google-labs-code/stitch-skills` into `skills/stitch-skills/` with README and LICENSE attribution. | Compared copied files with upstream checkout; only local root README/LICENSE additions differ. |
| Done | Integrate Stitch into Frontend Agent workflow | Updated `AGENTS.md` with triggers for `stitch-design`, `taste-design`, `enhance-prompt`, `design-md`, `react:components`, `stitch-loop`, and `shadcn-ui`. | `rg` checks confirmed Stitch references in `AGENTS.md`. |
| Done | Document Stitch source attribution | Updated `README.md` with upstream repo, pinned commit `6c0cbdb909b7d256c8b9b3854c8c8f87aab2c140`, and license path. | Reviewed README diff. |
| Done | Push Stitch integration to GitHub | Created branch `codex/add-stitch-skills`, committed `a45a4bc`, pushed to origin, and opened draft PR #1. | `git push -u origin codex/add-stitch-skills`; `gh pr create --draft`. |
| Done | Install team and Stitch skills globally for all local Codex projects | Copied curated skills into `~/.codex/skills/codex-team-skills` and Stitch skills into `~/.codex/skills/stitch-skills`; appended global team/Stitch instructions to `~/.codex/AGENTS.md`. | Verified 23 curated skills and 8 Stitch skills exist globally. |
| Done | Add durable work log for future conversations | Added this file so future sessions can quickly inspect completed, active, pending, blocked, and skipped work. | Created in `2c873e1`, marked complete in `7b32541`, and pushed to PR #1. |
| Done | Add global bootstrap for new projects | Added global templates for `docs/WORKLOG.md` and project `AGENTS.md`, plus `~/.codex/bin/codex-bootstrap-project`; updated global `~/.codex/AGENTS.md` to create project work logs when meaningful work starts. | Tested bootstrap against `/tmp/codex-bootstrap-test`; it created `AGENTS.md` and `docs/WORKLOG.md`. |
| Done | Rename and review Codex skill bundle | Renamed the curated `everything-claude-code` bundle to `codex-team-skills`, adapted Claude-specific runtime assumptions for Codex, synced the global skill install, and added `docs/SKILL_REVIEW.md`. | Commit `f03ec89`; `git diff --check`; verified 23 repo skills and 23 global skills; `rg` found no stale Claude runtime references outside the review report/source attribution. |

## Decisions

- Use global `~/.codex/AGENTS.md` for behavior that should apply to every
  project. New projects should inherit this automatically.
- Use project-level `AGENTS.md` only for project-specific rules, commands,
  architecture constraints, or domain context.
- Bootstrap `docs/WORKLOG.md` automatically for meaningful work in new projects
  so task state survives across conversations.
- Keep vendored skill sources under `skills/` with source attribution and pinned
  commits. Do not silently replace them with floating upstream content.
- Do not assume Stitch MCP is available. If MCP tools are missing, continue with
  local prompt/design-system/React conversion work that can be done from files.
- Keep unrelated worktree changes out of commits. The current `SKILL.md`
  deletion remains outside the Stitch branch commit scope.

## How To Update This Log

When starting a task:

1. Add a row with `In Progress`.
2. Include the branch or affected files when useful.
3. State the intended verification before implementation if the task is risky.

When finishing a task:

1. Change status to `Done`, `Skipped`, or `Blocked`.
2. Add the commit, PR, command output, or manual check that proves the outcome.
3. Note any remaining risk or unrelated worktree change.

Keep entries concise. This file is context for future agents, not a full chat
transcript.
