---
name: complete-report
description: >-
  Land a threat modeling session — merge its pull request into main once
  review is settled. Use when the user says "land this session", "merge the
  session", "the threat model is ready", or "land the session PR". Do not use
  this skill to scaffold a session. Do not use it to update a risk — use
  update-register.
license: MIT
metadata:
  interactive: yes
---

# Complete report

Use this skill to land a threat modeling session — merge its pull request into `main` once review is settled. Like [`land-audit`](https://github.com/kieranpotts/audits/tree/main/.agents/skills/land-audit), there is no "production must be live" gate: a session reports on the present state of a system, it does not describe a future one, so nothing needs to have shipped first. (Register _updates_ that reflect a future state — eg. a mitigation not yet shipped — are handled separately by [`/update-register`](../update-register/SKILL.md).)

**Input:** Target — REQUIRED. Infer the session from the checked-out branch
(`session/<slug>`). If on `main`, use the user's description, or list open
session pull requests and ask the user to choose.

**Output:** The pull request squash-merged into `main` with a `session:
<description>` message, and its branch deleted.

## Instructions

1.  **Identify the session.**

    Infer the target from the checked-out branch (`session/<slug>`). If on `main`, use the user's description, or list open session pull requests and ask the user to choose:

    ```sh
    gh pr list --search "session:" --json number,title,headRefName
    ```

2.  **Verify review is settled.**

    Confirm review feedback on the pull request has been addressed. Do not merge over unresolved comments without the user's explicit instruction.

3.  **Merge the pull request.**

    Confirm with the user that the PR is ready to merge — do not merge without explicit instruction. Once confirmed, squash-merge it with a `session: <description>` message:

    ```sh
    gh pr merge <number> --squash --subject "session: <short lowercase description>"
    ```

    Delete the branch if it is not deleted automatically. The report, its `INDEX.md` row, and the new `REGISTER.md` rows all land together.

## Rules

-   **Never merge over unresolved review comments without explicit instruction.**

-   **Squash-merge with the conventional message.**

    `session: <short lowercase description>`. The report, its `INDEX.md` row, and the register rows were all added together in [`/scaffold-report`](../scaffold-report/SKILL.md), so no further index or register update is needed at merge time.

-   **Do not merge without explicit instruction.**

## Success criteria

- The pull request is squash-merged into `main` with a `session: <description>` message, and the branch is deleted.

- `risks/INDEX.md` on `main` includes the new session's row, and `risks/REGISTER.md` includes its new rows (both already present from `/scaffold-report`, now landed).

## References

- [`AGENTS.md`](../../../AGENTS.md): The full session workflow.

- [`scaffold-report`](../scaffold-report//): Scaffolds the session this skill lands.

- [`update-register`](../update-register/): Keeps the raised risks current afterwards.
