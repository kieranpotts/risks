---
name: complete-report
description: >-
  Land in `main` the report from a threat modeling workshop. Use this skill when
  the user says something like "complete report", "land this report",
  "merge the report", or "the threat modeling workshop report is done". Do not
  use this skill to update the risk register without also landing a new
  workshop report.
license: MIT
metadata:
  interactive: yes
  preferred_model: prose-writing
---

# Complete report

Land a report from a threat modeling workshop in the `main` trunk.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Target — REQUIRED.** Infer the session from the checked-out branch
  (`report/<slug>`). If on `main`, use the user's description, or list open
  report pull requests and ask the user to choose.

## Success criteria

You will achieve the following outcomes:

<!-- The pull request squash-merged into `main` with a `report: <description>`
message, and its branch deleted. -->

- The pull request is squash-merged into `main` with a `report: <description>`
  message, and the branch is deleted.

- `risks/INDEX.md` on `main` includes the new session's row, and
  `risks/REGISTER.md` includes its new rows (both already present from
  `/draft-report`, now landed).

## Instructions

1.  Identify the session.

    Infer the target from the checked-out branch (`report/<slug>`). If on `main`, use the user's description, or list open report pull requests and ask the user to choose:

    ```sh
    gh pr list --search "report:" --json number,title,headRefName
    ```

2.  Verify review is settled.

    Confirm review feedback on the pull request has been addressed. Do not merge over unresolved comments without the user's explicit instruction.

3.  Confirm the PR is not a draft.

    ```sh
    gh pr view <number> --json isDraft
    ```

    If it is still a draft, stop and direct the user to
    [`/review-report`](../review-report/SKILL.md) first.

4.  Merge the pull request.

    Confirm with the user that the PR is ready to merge — do not merge without explicit instruction. Once confirmed, squash-merge it with a `report: <description>` message, and delete the source branch on the upstream repository:

    ```sh
    gh pr merge <number> --squash --subject "report: <short lowercase description>" --delete-branch
    ```

    The report, its `INDEX.md` row, and the new `REGISTER.md` rows all land together.

5.  Confirm the branch was deleted.

    In case the branch was not automatically deleted from the upstream repository, delete it directly:

    ```sh
    git push origin --delete report/<slug>
    ```

## Rules

- You MUST NOT merge a draft PR.

  Run [`/review-report`](../review-report/SKILL.md) first.

- You MUST NOT merge over unresolved review comments without explicit
  instruction.

- You MUST squash-merge with the conventional message.

  `report: <short lowercase description>`. The report, its `INDEX.md` row, and
  the register rows were all added together in
  [`/draft-report`](../draft-report/SKILL.md), so no further index or
  register update is needed at merge time.

- You MUST NOT merge without explicit instruction.
