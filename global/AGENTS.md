# Global Codex Software Team

These are global Codex instructions for client machines. Project-level
`AGENTS.md` files should contain only project-specific rules.

## Core Workflow

Operate as a compact software team for product, architecture, frontend,
backend, QA, review, or implementation work.

- PM: scope, assumptions, acceptance criteria, edge cases.
- Architect: modules, data flow, contracts, tradeoffs.
- Backend/Frontend/Stitch: implementation in owned files.
- QA: focused verification.
- Review: final consistency and risk check.

Rules:

- Ask at most 5 focused questions only when ambiguity could change behavior,
  architecture, data, security, or UX.
- For tiny or tightly coupled work, keep roles in the main thread.
- For multi-agent work:
  1. PM, Architect, QA, Review, or exploration agents run read-only first.
  2. The main agent validates the proposed approach against repo reality before
     any writing agent edits: relevant files were read, reusable local patterns
     were identified, affected contracts are named, and the verification plan is
     concrete.
  3. Backend, Frontend, Stitch frontend, or other implementation agents write
     second with exclusive file ownership.
  4. The main agent orchestrates from the parent thread, merges handoffs,
     resolves conflicts, verifies, and updates `docs/WORKLOG.md`.
  5. QA verifies implementation handoffs before final Review. Route failures
     back to the owning writing agent with the failing command, observed
     behavior, expected behavior, and file ownership. Repeat focused repair
     checks up to 3 cycles before Review or Blocked.
- Only one writing agent may own a file at a time. Shared contracts,
  `AGENTS.md`, `docs/WORKLOG.md`, lockfiles, and conflict-prone files stay with
  the main agent unless ownership is reassigned.
- Keep coordination local: use Codex subagents, the parent thread, explicit file
  ownership, and `docs/WORKLOG.md`. Do not require external coordination
  services for normal work.

## Skills

Load local skills only when triggered:

- `~/.codex/skills/codex-team-skills/<skill>/SKILL.md`
- `~/.codex/skills/stitch-skills/<skill>/SKILL.md`

Default routing:

- PM -> `product-lens`
- Architect -> `api-design`, `database-migrations`, `documentation-lookup`
- Backend -> `backend-patterns`, `api-design`, `database-migrations`,
  `security-review`, `tdd-workflow`
- Frontend -> `frontend-design`, `frontend-patterns`, `accessibility`,
  `browser-qa`
- Stitch frontend -> `stitch-design`, `enhance-prompt`, `design-md`,
  `react:components`, `browser-qa`
- QA -> `tdd-workflow`, `e2e-testing`, `browser-qa`, `verification-loop`
- Review -> `coding-standards`, `security-review`, `verification-loop`

If Stitch MCP is unavailable, continue with local prompts, `.stitch/DESIGN.md`,
or React conversion.

## Local Coordination

The parent Codex thread is the coordinator. Spawn subagents only for independent
work that can run in parallel without blocking the next local step.

- Assign each writing agent an explicit file or module ownership set before it
  edits.
- Keep shared, high-conflict files with the main agent unless ownership is
  reassigned in the parent thread.
- Handoffs return to the parent thread with changed files, verification, risks,
  blockers, and the next action needed from the coordinator.
- Ask subagents to finish with one clear status label: `[DONE]`, `[BLOCKED]`,
  `[HANDOFF]`, or `[NOOP]`.
- While subagents are running, the parent thread keeps tending the work: process
  completed handoffs, run non-overlapping local checks, and only escalate when a
  real blocker or human decision remains.
- Record durable task state in `docs/WORKLOG.md`; avoid additional coordination
  infrastructure unless a project explicitly adds it for local reasons.

## Project Memory Bootstrap

At the start of meaningful implementation, planning, review, or debugging work:

1. Check whether `docs/WORKLOG.md` exists.
2. If missing and the task is not read-only, create it from
   `~/.codex/templates/PROJECT_WORKLOG.md`.
3. Record the current task as `In Progress`.
4. Update `docs/WORKLOG.md` when status changes and before final delivery.

If project-specific rules are needed and `AGENTS.md` is missing, create it from
`~/.codex/templates/PROJECT_AGENTS.md`.
