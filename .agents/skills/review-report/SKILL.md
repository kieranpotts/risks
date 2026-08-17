---
name: review-report
description: >-
  Take a threat modeling workshop report's pull request out of draft once it
  has enough substance for reviewers to weigh in. Use this skill when the
  user says something like "review this report", "this report is ready for
  review", "take the report out of draft", "mark the report ready for
  review", or "review report". Do not use this skill to merge the pull
  request or to fill in gaps in the report.
compatibility: >-
  requires Read, Grep, Bash (git, gh)
license: CC0-1.0
---

# Review report

Take a threat modeling workshop report's pull request out of its draft state,
once there is enough substance for reviewers to weigh in. This is a light
completeness check, not a sign-off: confirm there is something real to review,
then hand the report to its reviewers untouched.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Target — REQUIRED.** The workshop whose pull request is to be marked
  ready. Infer it from the checked-out branch, which is named
  `latest/report/<slug>`. If `latest/main` is checked out, or the user names a
  pull request number instead, resolve the target from the open draft pull
  requests.

## Success criteria

- The target pull request MUST no longer be a draft, verifiable as
  `isDraft: false`.

- The report's metadata header — facilitator, workshop date, PR, scope,
  frameworks — MUST be filled in, with no literal template placeholder text
  left in it.

- The report's threat assessment MUST carry at least one rated entry, and
  every reference listed under "Risks raised" MUST have a matching row in
  [`risks/REGISTER.md`](../../../risks/REGISTER.md).

- Where any of the above does not hold, the pull request MUST have been left
  in draft and the gaps reported to the user.

- The repository's files MUST be unchanged. This skill alters the pull
  request's state, never its contents.

## Instructions

1.  Identify the workshop and its pull request.

    Infer the target from the checked-out `latest/report/<slug>` branch. If
    `latest/main` is checked out, list the open draft pull requests and ask the
    user which one to mark ready:

    ```sh
    gh pr list --draft --search "create:" --json number,title,headRefName
    ```

2.  Read `risks/YYYY-MM-DD-<slug>/README.md` in full, then check it for
    substance:

    - The header fields (facilitator, workshop date, PR, scope, frameworks)
      are filled in, not left as the template's placeholders — `Your Name`,
      `YYYY-MM-DD`, `owner/repo@<commit>`, `#...`.
    - The threat assessment carries at least one entry rated for likelihood,
      impact, and severity.
    - Every reference listed under "Risks raised" has a matching row in
      [`risks/REGISTER.md`](../../../risks/REGISTER.md).

    Report any gaps to the user and stop, leaving the pull request in draft.

    Do NOT block on stylistic polish, a thin "Mitigation strategies"
    narrative, or unticked "Follow-ups" boxes. Those are what review is for.

3.  Remove the pull request's draft status.

    ```sh
    gh pr ready <number>
    ```

4.  Summarize what you did, and note anything you chose not to block on.

## Rules

- You MUST NOT mark ready a report with no rated threats at all.

  A report with no threat assessment gives a reviewer nothing to weigh in on,
  and the workshop index would record a workshop that raised nothing.

- You MUST NOT require the report to be finished.

  Mitigation strategies and follow-ups MAY still be refined in response to
  review feedback. This skill checks for substance, not completion.

- You MUST NOT edit the report, the workshop index, or the risk register.

  Flag gaps to the user rather than filling them in on their behalf. The
  workshop's participants own the findings.

- You MUST NOT merge the pull request.

  Landing the report in `latest/main` is a separate, later stage of the workflow,
  and it happens only once review has settled.

## Edge cases

- The pull request is already out of draft.

  Report that it is already ready for review, run the completeness check
  anyway, and surface any gaps you find. Do not re-open it as a draft.

- The branch holds more than one workshop report.

  Stop and tell the user. The workflow expects exactly one workshop per branch
  and pull request, and a reviewer cannot weigh in on two scopes at once.
