# Agent skills

This repository ships a small set of [agent skills](https://agentskills.io/) —
invoked as slash commands through agentic tools such as Claude Code — that
automate the risk register workflow.

The skills automate the recurring mechanics of running a threat modeling
session, landing its report, and keeping the register current. The available
skills are:

- **[scaffold report](./scaffold-report/)**:
  Scaffolds a threat modeling session. Cuts a `session/<slug>` branch from
  `main`, runs the session against the scoped system (using the STRIDE and other
  frameworks per [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054)),
  writes the report, seeds new rows into the register, and opens a pull request.

- **[finalize report](./finalize-report)**:
  Lands a session. Confirms review is settled, squash-merges the pull request
  to `main` with a `session: <description>` message, and deletes the branch.

- **[update register](./update-register/)**:
  Updates the living register between sessions — a mitigation landing, a risk
  reassessment, or a scheduled review — on a `register/<slug>` branch, without
  touching any merged session report.

A typical session journey runs `/scaffold-report` → review via normal pull request
comments → `/finalize-report`. Thereafter, risks are kept current with
`/update-register`. Like the sibling `audits` repository, sessions have no
lifecycle state machine and no discussion-thread requirement — a session is
scoped, held, and merged in a single pass. The register, by contrast, is living
documentation, so `/update-register` is used repeatedly over a risk's lifetime.

## Skills path

Agent harnesses are converging on the `./.agents/skills/` path for dynamic
retrieval of project-specific skills compatible with the
[Agent Skills](https://agentskills.io/) conventions. This is the primary path
detected by OpenAI Codex, and is supported as an agent-agnostic alternative by
GitHub Copilot / VS Code, Gemini CLI, Google Antigravity, OpenCode, and Pi.

As of May 2026, Claude Code and Cursor do NOT dynamically retrieve skills from
this path. If you use these harnesses you will need a workaround, eg. symlinks
from `.claude/skills/`.

See <https://github.com/kieranpotts/skills> for a template for AI skills.
