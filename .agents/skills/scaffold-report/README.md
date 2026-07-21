# Scaffold report

Scaffolds a report from a threat modeling workshop, ready to open for review.

## What it does

- Creates a `session/<slug>` branch from `main`.

- Runs a structured threat model (STRIDE, plus LINDDUN / OWASP where relevant)
  against the scoped system, per
  [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054).

- Writes the report to `risks/YYYY-MM-DD-<slug>/README.md`, seeds new rows into
  `risks/REGISTER.md`, and adds a row to `risks/INDEX.md`.

- Commits, pushes, and opens a pull request titled `session: <description>`.

## How to invoke

> Scaffold report

> Threat model the payment flow.

## Examples

- `/scaffold-report`: The agent asks for the scope and frameworks, then
  scaffolds the branch and PR.

- `/scaffold-report <description>`: Provide the scope, from which the agent
  infers the slug and runs the workshop directly.

Once open, gather feedback via normal pull request comments, then use
[`/complete-report`](../complete-report/README.md) to merge it. Thereafter,
keep the raised risks current with
[`/update-register`](../update-register/README.md).
