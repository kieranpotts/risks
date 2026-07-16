# Draft report

Scaffolds a report from a threat modeling workshop, ready to open for review.

## What it does

- Creates a `report/<slug>` branch from `main`.

- Runs a structured threat model (STRIDE, plus LINDDUN / OWASP where relevant)
  against the scoped system, per
  [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054).

- Writes the report to `risks/YYYY-MM-DD-<slug>/README.md`, seeds new rows into
  `risks/REGISTER.md`, and adds a row to `risks/INDEX.md`.

- Commits, pushes, and opens a pull request titled `report: <description>`.

## How to invoke

> Draft report

> Threat model the payment flow.

## Examples

- `/draft-report`: The agent asks for the scope and frameworks, then scaffolds
  the branch and PR.

- `/draft-report <description>`: Provide the scope, from which the agent infers
  the slug and runs the workshop directly.

Once open, gather feedback via normal pull request comments, then use
[`/land-report`](../land-report/README.md) to merge it. Thereafter, keep the
raised risks current with [`/update-register`](../update-register/README.md).
