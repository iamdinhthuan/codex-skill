# Codex Skill

Multi-agent operating instructions for Codex.

This repo defines a compact software team workflow in `AGENTS.md`: PM,
Architect, Backend, Frontend, QA, and Review. The goal is for Codex to accept a
natural-language requirement, run the role handoffs automatically, implement
real code when requested, verify the result, and report only the useful output.

## Files

- `AGENTS.md` - main Codex team operating prompt
- `docs/CLIENT_SETUP.md` - how another machine installs and uses this repo as
  its global Codex operating model
- `docs/GLOBAL_BOOTSTRAP.md` - how global Codex instructions, skills, and
  project work logs are bootstrapped into new projects
- `docs/SKILL_REVIEW.md` - review notes for each vendored skill and its
  Codex-specific adaptations
- `docs/WORKLOG.md` - durable task memory for completed, active, pending,
  blocked, and skipped work
- `skills/codex-team-skills/` - Codex-adapted curated local copies of
  essential skills from `affaan-m/everything-claude-code`
- `skills/stitch-skills/` - local copies of Google Stitch skills for
  AI-assisted frontend design and Stitch-to-React workflows
- `global/AGENTS.md` - global Codex instructions copied to `~/.codex/AGENTS.md`
- `global/agents/` - named specialist subagent config copied to
  `~/.codex/agents/`
- `templates/` - project bootstrap templates copied into `~/.codex/templates`
- `bin/codex-bootstrap-project` - helper script for creating project-local
  `AGENTS.md` and `docs/WORKLOG.md`

## Behavior

When a requirement is clear, Codex should proceed through the team workflow
without asking the user to manually trigger each role. When a requirement is
ambiguous enough to affect product behavior, architecture, data, security, or
UX, the PM role asks up to 5 focused questions; otherwise the team continues
with explicit assumptions.

The local skill set is intentionally small. Framework-specific, infrastructure,
benchmarking, and skill-audit skills are excluded until repo evidence shows
they are needed.

## Source Attribution

The curated skill files under `skills/codex-team-skills/` are adapted from:

https://github.com/affaan-m/everything-claude-code

Pinned source commit:

```text
4e66b2882da9afb9747468b08a253ca2f09c85f3
```

The copied upstream license is available at
`skills/codex-team-skills/LICENSE`.

The Stitch skill files under `skills/stitch-skills/` come from:

https://github.com/google-labs-code/stitch-skills

Pinned source commit:

```text
6c0cbdb909b7d256c8b9b3854c8c8f87aab2c140
```

The copied upstream license is available at `skills/stitch-skills/LICENSE`.
