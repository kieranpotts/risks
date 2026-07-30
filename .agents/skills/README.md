# Agent skills

Skills available to agents in this repository are:

- **[Scaffold report](./scaffold-report/):**
  Scaffolds a report for a threat modeling workshop.

- **[Complete report](./complete-report/):**
  Lands a threat modeling session's report in the `main` trunk.

- **[Update register](./update-register/):**
  Updates the living risk register.

## Conventions

A few structural conventions recur across these skill files:

- **"References" closing sections.**
  Some skills end with a `## References` section linking to related
  documents, such as [`AGENTS.md`](../../../AGENTS.md) or sibling skills, for
  further reading beyond the skill's own instructions.

- **"Transition gates" sections.**
  In repositories where documents move through a lifecycle state machine
  (eg. `PROPOSED` → `ACCEPTED`), the skill that performs a transition
  documents its entry conditions under a `## Transition gates: <FROM> →
  <TO>` heading. This repository's register is a living document with no
  such lifecycle — its rows are updated in place rather than transitioned
  between states — so none of these skills carry a "Transition gates"
  section.

## Compatibility

Agent harnesses are converging on the `./.agents/skills/` path for dynamic
retrieval of project-specific skills. This is compatible with the Agent Skills
convention — see https://agentskills.io/.

As of May 2026, OpenAI Codex, GitHub Copilot, Gemini CLI, Google Antigravity,
OpenCode, and Pi will auto-discover these skills, but Claude Code and Cursor
will not.

You will require workarounds for incompatible harnesses. For Claude Code, you
can simply symlink this directory from `.claude/skills/`. Cursor requires more
effort to transpile these skills into its native "rules" format.
