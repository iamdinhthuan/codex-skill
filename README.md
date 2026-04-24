# Codex Skill

Multi-agent operating instructions for Codex.

This repo defines a compact software team workflow in `AGENTS.md`: PM,
Architect, Backend, Frontend, QA, and Review. The goal is for Codex to accept a
natural-language requirement, run the role handoffs automatically, implement
real code when requested, verify the result, and report only the useful output.

## Files

- `AGENTS.md` - main Codex team operating prompt
- `SKILL.md` - compact surgical-coding guideline skill
- `docs/WORKLOG.md` - durable task memory for completed, active, pending,
  blocked, and skipped work
- `skills/everything-claude-code/` - curated local copies of essential skills
  from `affaan-m/everything-claude-code`
- `skills/stitch-skills/` - local copies of Google Stitch skills for
  AI-assisted frontend design and Stitch-to-React workflows

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

The curated skill files under `skills/everything-claude-code/` come from:

https://github.com/affaan-m/everything-claude-code

Pinned source commit:

```text
4e66b2882da9afb9747468b08a253ca2f09c85f3
```

The copied upstream license is available at
`skills/everything-claude-code/LICENSE`.

The Stitch skill files under `skills/stitch-skills/` come from:

https://github.com/google-labs-code/stitch-skills

Pinned source commit:

```text
6c0cbdb909b7d256c8b9b3854c8c8f87aab2c140
```

The copied upstream license is available at `skills/stitch-skills/LICENSE`.
