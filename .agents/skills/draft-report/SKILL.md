---
name: draft-report
description: >-
  Draft a report for a threat modeling workshop. Use this skill when the user
  wants to prepare for a new threat modeling workshop or to write up a report
  from a recent workshop, or says something like "draft a report", "prepare a
  threat modeling workshop report", or "draft a new report". Do not use this
  skill to run the assessment, write the findings, or add rows to the risk
  register.
compatibility: >-
  requires Read, Write, Edit, Bash (git, gh, date)
license: CC0-1.0
---

# Draft report

Scaffold a new, blank threat modeling workshop report, ready for the findings
to be written into it. Take it from nothing to an open, draft pull request
with the artifacts in place. Do not run the assessment, write findings, or add
rows to the risk register.

## Parameters

Determine the following information from the surrounding context and
environment, if possible. If you're uncertain about the required parameters,
prompt the user for clarification.

- **Scope — REQUIRED.** The system, subsystems, services, or data flows being
  threat modeled, with the `owner/repo@commit` or the version where
  applicable.

- **Frameworks — OPTIONAL.** The threat modeling lenses the workshop will
  apply. Defaults to STRIDE, which is the minimum. Add LINDDUN where the
  system handles personal data, and the OWASP Top 10 or a similar checklist
  for web-facing systems.

- **Workshop date — OPTIONAL.** The date the workshop was, or will be, held.
  Assume today, resolved with the Unix `date` command.

- **Facilitator — OPTIONAL.** Who is running the workshop. Assume the current
  user, discovered from the Git configuration.

- **Prior workshop report — OPTIONAL.** An earlier report covering the same
  system, where this workshop revisits one. Discover it from
  [`risks/INDEX.md`](../../../risks/INDEX.md).

## Success criteria

- Branch `report/<slug>` MUST exist and be checked out.

- `risks/YYYY-MM-DD-<slug>/README.md` MUST exist, MUST follow the structure of
  [`risks/TEMPLATE.md`](../../../risks/TEMPLATE.md), and its metadata header
  MUST be filled in as far as the parameters allow.

- The report's assessment sections MUST still carry the template's placeholder
  prose, because the findings are authored after this skill has run.

- [`risks/INDEX.md`](../../../risks/INDEX.md) MUST carry a new row for this
  workshop, at the top of the table.

- A pull request titled `report: <short lowercase description>` MUST be open
  against `main` in its draft state, and the report's `PR` header field MUST
  name it.

- [`risks/REGISTER.md`](../../../risks/REGISTER.md) MUST be unchanged.

## Instructions

1.  Establish a short description of the workshop, in the present tense, full
    lowercase, and NOT terminated by a period. Keep this in memory: it becomes
    the commit message and the pull request title.

2.  Transform the description into a hyphen-delimited slug. For example, the
    description "payment flow threat model" becomes the slug `payment-flow`,
    and "checkout api" becomes `checkout-api`.

3.  Create the branch.

    ```sh
    git checkout main
    git pull --rebase
    git checkout -b report/<slug>
    ```

4.  Copy [`risks/TEMPLATE.md`](../../../risks/TEMPLATE.md) to
    `risks/YYYY-MM-DD-<slug>/README.md`, where `YYYY-MM-DD` is the workshop
    date.

5.  Fill in the metadata header only — facilitator, participants, workshop
    date, scope, and the chosen frameworks — and link the prior workshop
    report where there is one. Leave the `PR` field, which is not yet known,
    and leave the summary, business context, technical scope, system
    decomposition, threat assessment, risks raised, mitigation strategies, and
    follow-ups as the template's placeholders.

6.  Prepend a row to [`risks/INDEX.md`](../../../risks/INDEX.md), at the top of
    the table (newest first). Fill in the date, scope, and frameworks. Leave
    the count of risks raised as "TBC" — the assessment has not run yet.

7.  Commit and push. Stage the index alongside the report, so the row added in
    step 6 lands on the same branch.

    ```sh
    git add risks/
    git commit -m "report: <short lowercase description>"
    git push -u origin report/<slug>
    ```

8.  Open a draft pull request.

    ```sh
    gh pr create --draft --title "report: <short lowercase description>" --fill
    ```

    If the `gh` client is unavailable or unauthenticated, fail with an error.

    No discussion thread is required. A workshop report is findings to review
    through normal pull request comments, not a decision to debate.

9.  Record the returned PR number in the report's `PR` header field, then
    commit and push that change.

    ```sh
    git add risks/
    git commit -m "chore: add pr number to threat modeling workshop report"
    git push
    ```

10. Summarize what you did, and direct the user to the next stage: run the
    threat modeling workshop itself — decompose the system, assess it against
    the chosen frameworks, rate each threat, and promote the risks worth
    tracking into the risk register — writing into the report you just
    scaffolded rather than starting a new one.

## Rules

- There MUST be exactly one workshop per branch and per pull request.

  Do not bundle several scopes into one pull request. Each workshop is
  indexed, reviewed, and merged on its own.

- You MUST branch from `main`, not from any other branch.

  Workshops are always cut from `main`. If local `main` is behind the remote,
  pull first, using the rebase strategy to keep history linear.

- You MUST stage every file you changed, including
  [`risks/INDEX.md`](../../../risks/INDEX.md).

  The index row lives in a different file from the report, so staging only the
  report directory would leave the row uncommitted and it would never reach
  `main`.

- You MUST NOT run the assessment or write findings.

  Decomposing the system, assessing it against the frameworks, and rating
  threats all belong to the workshop, which writes into the report this skill
  creates.

- You MUST NOT add rows to [`risks/REGISTER.md`](../../../risks/REGISTER.md).

  Register rows record threats that an assessment actually raised. Seeding
  them beforehand would put unfounded entries in the living register, which is
  the single source of truth for current risk status.

## References

- [TS-54: Threat Modeling](https://github.com/kieranpotts/standards/tree/latest/dev/src/054) \
  Read when the report template's structure or the rating scheme is unclear.
  This is the standard the template follows.
