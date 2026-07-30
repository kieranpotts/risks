---
name: complete-report
description: >-
  Land a threat modeling session — merge its pull request into main once
  review is settled. Use when the user says "land this session", "merge the
  report", "the threat model is ready", or "land the report PR". Do not use
  this skill to scaffold a report. Do not use it to update a risk — use
  update-register.
license: MIT
metadata:
  interactive: yes
  preferred_model: prose-writing
---

# Complete report

Use this skill to land a threat modeling session — merge its pull request into `main` once review is settled. Like [`complete-audit`](https://github.com/kieranpotts/audits/tree/main/.agents/skills/complete-audit), there is no "production must be live" gate: a session reports on the present state of a system, it does not describe a future one, so nothing needs to have shipped first. (Register updates that reflect a future state — eg. a mitigation not yet shipped — are handled separately by [`/update-register`](../update-register/SKILL.md).)

## Input

Determine the following information from the surrounding context and
environment, if possible.

- Target — REQUIRED. Infer the session from the checked-out branch
  (`report/<slug>`). If on `main`, use the user's description, or list open
  report pull requests and ask the user to choose.

## Output

The pull request squash-merged into `main` with a `report: <description>`
message, and its branch deleted.

## Instructions

1.  Identify the session.

    Infer the target from the checked-out branch (`report/<slug>`). If on `main`, use the user's description, or list open report pull requests and ask the user to choose:

    ```sh
    gh pr list --search "report:" --json number,title,headRefName
    ```

2.  Verify review is settled.

    Confirm review feedback on the pull request has been addressed. Do not merge over unresolved comments without the user's explicit instruction.

3.  Merge the pull request.

    Confirm with the user that the PR is ready to merge — do not merge without explicit instruction. Once confirmed, squash-merge it with a `report: <description>` message, and delete the source branch on the upstream repository:

    ```sh
    gh pr merge <number> --squash --subject "report: <short lowercase description>" --delete-branch
    ```

    The report, its `INDEX.md` row, and the new `REGISTER.md` rows all land together.

4.  Confirm the branch was deleted.

    In case the branch was not automatically deleted from the upstream repository, delete it directly:

    ```sh
    git push origin --delete report/<slug>
    ```

## Rules

- You MUST NOT merge over unresolved review comments without explicit
  instruction.

- You MUST squash-merge with the conventional message.

  `report: <short lowercase description>`. The report, its `INDEX.md` row, and
  the register rows were all added together in
  [`/scaffold-report`](../scaffold-report/SKILL.md), so no further index or
  register update is needed at merge time.

- You MUST NOT merge without explicit instruction.

## Success criteria

- The pull request is squash-merged into `main` with a `report: <description>`
  message, and the branch is deleted.

- `risks/INDEX.md` on `main` includes the new session's row, and
  `risks/REGISTER.md` includes its new rows (both already present from
  `/scaffold-report`, now landed).
