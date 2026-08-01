---
name: draft-report
description: >-
  Draft a report for a threat modeling workshop. Use this skill when the
  user wants to prepare for a new threat modeling workshop or to write up a
  report from a recent workshop, or says something like "draft a report",
  "prepare a threat modeling workshop report", or "draft a new report".
license: MIT
metadata:
  interactive: yes
  preferred_model: ollama/prose-writing
---

# Draft report

Scaffold a new, blank threat modeling workshop report, ready for the findings
to be written into it. Do not run the assessment, and do not write findings.

Like an audit, a report has no lifecycle state machine — this skill takes it
from nothing to an open, draft pull request with the artifacts in place. The
assessment itself, and the register rows it raises, are authored separately
and land on this same branch.

## Parameters

Determine the following information from the surrounding context and
environment, if possible.

- **Scope — REQUIRED.** The system, subsystems, services, or data flows being
  threat modeled, and the `owner/repo@commit` or version where applicable.

- **Frameworks — OPTIONAL**, defaults to STRIDE. Recorded in the report's
  metadata header so the assessment knows which lenses to apply.

- **Prior session report — OPTIONAL**, if this revisits a system. Linked from
  the new report's header.

## Success criteria

You will achieve the following outcomes:

<!-- A `report/<slug>` branch, with a blank `risks/YYYY-MM-DD-<slug>/README.md`
created from the template and its header filled in, a new row in
`risks/INDEX.md`, committed to a pull request opened against `main` as a
draft. -->

- Branch `report/<slug>` exists and is checked out.

- `risks/YYYY-MM-DD-<slug>/README.md` exists, a copy of
  [`TEMPLATE.md`](../../../risks/TEMPLATE.md) with the metadata header filled
  in (facilitator, workshop date, scope, frameworks, PR) and the assessment
  sections left as placeholders.

- `risks/INDEX.md` has a new row for this session, at the top.

- A pull request titled `report: <short lowercase description>` is open, as a
  draft.

- The user has been directed to run the assessment and write the findings
  into the scaffolded report.

## Instructions

1.  Determine the scope.

    Ask the user what is being threat modeled if not already stated — the
    system, subsystems, services, or data flows in-scope, and the
    `owner/repo@commit` or version where applicable. Scope is the only
    REQUIRED input.

2.  Determine the frameworks.

    STRIDE is the default and minimum. Add LINDDUN if the system handles
    personal data, and OWASP Top 10 or other checklists for web-facing
    systems. Confirm with the user if unsure.

3.  Determine the slug and description.

    Establish a short, hyphen-delimited slug naming the scope, eg.
    `payment-flow` or `auth-service`. Decide a short prose description from the
    user's request.

4.  Create the branch.

    ```sh
    git checkout main
    git pull
    git checkout -b report/<slug>
    ```

5.  Create the report from the template.

    Copy [`risks/TEMPLATE.md`](../../../risks/TEMPLATE.md) to
    `risks/YYYY-MM-DD-<slug>/README.md`, where `YYYY-MM-DD` is the workshop
    date.

    Fill in the metadata header only — facilitator, workshop date, scope, and
    the chosen frameworks — and link the prior session report if there is
    one. Leave the summary, business context, decomposition, threat
    assessment, risks raised, mitigation strategies, and follow-ups as the
    template placeholders.

    Do not decompose the system, assess threats, or write findings. That is
    the assessment's job, not this skill's.

6.  Prepend the index row.

    Add a row to [`risks/INDEX.md`](../../../risks/INDEX.md), at the top
    (newest first): Date, Scope, Frameworks, and the count of risks raised.

7.  Commit and open a pull request.

    Stage the whole `risks/` directory so the index row committed in step 6
    lands alongside the report:

    ```sh
    git add risks/
    git commit -m "report: <short lowercase description>"
    git push -u origin report/<slug>
    gh pr create --draft --title "report: <short lowercase description>" --fill
    ```

    Open it as a draft, not ready for review. No discussion thread is
    required: a session report is findings to review via normal pull request
    comments, not a decision to debate.

    Record the returned PR number in the report's metadata header, then
    commit and push that change.

8.  Hand off to the assessment.

    The scaffold is now in place: branch, blank report, index row, and draft
    pull request. The next step is to run the threat modeling session itself
    — decompose the system, assess it against the chosen frameworks, rate
    each threat, and promote the risks worth tracking into
    [`risks/REGISTER.md`](../../../risks/REGISTER.md). Direct the user to do
    that, writing into the report this skill just created rather than
    starting a new one.

## Rules

- There MUST be exactly one session per branch and pull request.

  Do not bundle multiple scopes into one PR — draft a separate session for
  each.

- You MUST branch from `main`, not from any other branch.

  Sessions are always cut from `main`. If local `main` is behind the remote,
  pull first.

- You MUST NOT run the assessment or write findings.

  This skill only scaffolds the report and its pull request. Decomposing the
  system, assessing it against the frameworks, rating threats, and seeding
  [`risks/REGISTER.md`](../../../risks/REGISTER.md) are all out-of-scope —
  they belong to the assessment, which writes into the report this skill
  creates.

- You MUST NOT seed the register.

  Register rows record threats that an assessment actually raised. Adding
  them before the assessment has run would put unfounded entries in the
  living register.

- You MUST stage every file you changed, including `risks/INDEX.md`.

  The index row added in step 6 lives in a different file from the report.
  Staging only the report directory leaves the row uncommitted, so it never
  reaches `main`.

## References

- [TS-54: Threat Modeling](https://github.com/kieranpotts/standards/tree/latest/dev/src/054):
  The standard whose report structure this scaffold follows.
