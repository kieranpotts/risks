---
name: complete-report
description: >-
  Land in `main` the report from a threat modeling workshop. Use this skill
  when the user says something like "complete report", "land this report",
  "merge the report", or "the threat modeling workshop report is done". Do not
  use this skill to update the risk register without also landing a new
  workshop report.
compatibility: >-
  requires Read, Bash (git, gh)
license: CC0-1.0
---

# Complete report

Land a threat modeling workshop report in the `main` trunk, by squash-merging
its pull request and deleting its branch. Do not edit the report, the workshop
index, or the risk register: everything that lands was written earlier, on
this same branch.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target — REQUIRED.** The workshop whose pull request is to be merged.
  Infer it from the checked-out branch, which is named `report/<slug>`. If
  `main` is checked out, resolve the target from the user's description or
  from the open report pull requests.

- **Merge approval — REQUIRED.** The user's explicit instruction to merge.
  Merging is irreversible in practice — the branch is deleted and the workshop
  report becomes immutable — so it MUST NOT be inferred from context alone.

## Success criteria

- The pull request MUST be squash-merged into `main` with a
  `report: <short lowercase description>` message.

- The `report/<slug>` branch MUST be deleted from the upstream repository.

- `main` MUST now carry the workshop report, its row in
  [`risks/INDEX.md`](../../../risks/INDEX.md), and the rows the assessment
  raised in [`risks/REGISTER.md`](../../../risks/REGISTER.md).

- Where review is unsettled or the pull request is still a draft, the merge
  MUST NOT have happened, and the user MUST have been told why.

- The merge MUST NOT have been accompanied by any edit to the report, the
  workshop index, or the risk register.

## Instructions

1.  Identify the workshop and its pull request.

    Infer the target from the checked-out `report/<slug>` branch. If `main` is
    checked out, use the user's description, or list the open report pull
    requests and ask the user to choose:

    ```sh
    gh pr list --search "report:" --json number,title,headRefName
    ```

2.  Confirm the pull request is not still a draft.

    ```sh
    gh pr view <number> --json isDraft
    ```

    If it is a draft, stop. The report has not yet been through the stage that
    checks it has enough substance to review, so it is not ready to land.

3.  Confirm review is settled.

    Check that the review feedback on the pull request has been addressed. Do
    not merge over unresolved comments without the user's explicit
    instruction.

4.  Merge the pull request.

    Squash-merge with a `report: <description>` message, and delete the source
    branch on the upstream repository:

    ```sh
    gh pr merge <number> --squash \
      --subject "report: <short lowercase description>" --delete-branch
    ```

    The report, its index row, and the register rows the assessment raised all
    land together in one commit.

5.  Confirm the branch was deleted. Where the upstream repository did not
    delete it automatically, delete it directly:

    ```sh
    git push origin --delete report/<slug>
    ```

6.  Summarize what you did, naming the merge commit and the register rows that
    are now live.

## Rules

- You MUST NOT merge without the user's explicit instruction.

  Merging deletes the branch and freezes the report, which is treated as
  immutable from that point on. A reassessment then costs a whole new
  workshop.

- You MUST NOT merge a draft pull request.

  The draft state signals that the report has not yet been checked for enough
  substance to review.

- You MUST NOT merge over unresolved review comments unless the user tells you
  to.

- You MUST squash-merge, with a `report: <short lowercase description>`
  subject.

  A workshop lands as exactly one commit on `main`, matching the message the
  report was drafted under.

- You MUST NOT edit the report, the workshop index, or the risk register at
  merge time.

  All three were written on this branch — the report and index row when the
  report was scaffolded, the register rows when the assessment ran — so there
  is nothing left to add.

## Edge cases

- The pull request has merge conflicts against `main`.

  Stop and report them. Conflicts usually mean another workshop landed a
  register row or index row in the meantime, and resolving them is an edit to
  the report's branch, which is outside this skill's remit.
