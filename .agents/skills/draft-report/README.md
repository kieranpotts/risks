# Draft report

Drafts a report from a threat modeling workshop, opened as a draft.

## What it does

- Creates a `report/<slug>` branch from `main`.

- Runs a structured threat model (STRIDE, plus LINDDUN / OWASP where relevant)
  against the scoped system, per
  [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054).

- Writes the report to `risks/YYYY-MM-DD-<slug>/README.md`, seeds new rows into
  `risks/REGISTER.md`, and adds a row to `risks/INDEX.md`.

- Commits, pushes, and opens a draft pull request titled `report: <description>`.

## How to invoke

> Draft report

> Threat model the payment flow.

## Recommended models

A premium reasoning model is worth it here. Unlike the other skills in this
collection, this one runs the actual threat modeling workshop — identifying
STRIDE/LINDDUN/OWASP threats takes the same kind of independent judgment as
the [**probe**](https://github.com/kieranpotts/skills/tree/latest/dev/skills/probe)
skill it's built on.

## Examples

- `/draft-report`: The agent asks for the scope and frameworks, then
  drafts the branch and PR.

- `/draft-report <description>`: Provide the scope, from which the agent
  infers the slug and runs the workshop directly.

Once open, use [`/review-report`](../review-report/README.md) to take it out
of draft, gather feedback via normal pull request comments, then use
[`/complete-report`](../complete-report/README.md) to merge it. Thereafter,
keep the raised risks current with
[`/update-register`](../update-register/README.md).
