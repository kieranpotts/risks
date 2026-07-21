# Agent skills

Skills available to agents in this repository are:

- **[Scaffold report](./scaffold-report/):**
  Scaffolds a threat modeling session.

- **[Finalize report](./finalize-report/):**
  Lands a finalize workshop report.

- **[Update register](./update-register/):**
  Updates the living risk register.

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
