---
name: review-report
description: >-
  Take a threat modeling workshop report's pull request out of draft once it
  has enough substance for reviewers to weigh in. Use this skill when the
  user says something like "review this report", "this report is ready for
  review", "take the report out of draft", "mark the report ready for
  review", or "review report".
license: CC0-1.0
metadata:
  interactive: yes
  preferred_model: ollama/WORKFLOW_BASIC
---

# Review report

Use this skill to remove a threat modeling workshop report's pull request
from its draft state once there is enough substance for reviewers to weigh
in. This is a light completeness check, not a final sign-off — the
mitigation strategies and follow-ups MAY still be refined in response to
review feedback. This skill only confirms there is something real to review.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Target — REQUIRED.** Infer the target session from the checked-out
  branch (`report/<slug>`). If on `main`, list open draft pull requests
  matching `report:` and ask the user to choose:

  ```sh
  gh pr list --draft --search "report:" --json number,title,headRefName
  ```

## Success criteria

You will achieve the following outcomes:

- The PR is no longer a draft (`isDraft: false`).

- The header metadata (facilitator, workshop date, scope, frameworks, PR) is
  filled in.

- The threat assessment has at least one rated entry, and every risk it
  raises has a matching row in `risks/REGISTER.md`.

- No literal template placeholder text remains in the header.

## Instructions

1.  Identify the session and its PR.

    Infer the target from the checked-out branch (`report/<slug>`). If on
    `main`, list open draft pull requests and ask the user to choose.

2.  Do a light completeness check.

    Read `risks/YYYY-MM-DD-<slug>/README.md` in full.

    - Confirm the header fields (facilitator, workshop date, scope,
      frameworks, PR) are filled in, not left as template placeholders
      (`Your Name`, `YYYY-MM-DD`, `owner/repo@<commit>`, `#...`).
    - Confirm the threat assessment has at least one rated entry.
    - Confirm every reference listed under `Risks raised` has a matching row
      in [`risks/REGISTER.md`](../../../risks/REGISTER.md).

    Report any gaps to the user and stop. Do NOT block on stylistic polish,
    an empty `Mitigation strategies` narrative, or open `Follow-ups`
    checkboxes — those are exactly what review is for.

3.  Remove the PR's draft status.

    ```sh
    gh pr ready <number>
    ```

4.  Output a summary of your actions.

## Rules

- You MUST NOT mark ready a report with no rated threats at all.

  A report with no threat assessment has nothing for a reviewer to weigh in
  on.

- You MUST NOT require the report to be finished.

  Mitigation strategies and follow-ups MAY still be refined in response to
  review feedback. This skill checks for substance, not completion.

- You MUST NOT edit the report's content yourself.

  Flag gaps to the user; do not fill them in on their behalf.

- You MUST NOT merge the pull request.

  That is [`/complete-report`](../complete-report/SKILL.md)'s job, once
  review is settled.
