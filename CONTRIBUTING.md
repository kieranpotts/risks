# Contributing

<!-- Agents MUST read ./AGENTS.md. This document is for humans. -->

These contributing guidelines provide step-by-step instructions for running a
threat modeling workshop, landing its report, and keeping the risk register
current.

The focus here is on the mechanics and guardrails of the process. See the
[documentation](./docs/) for more general guidance on threat modeling and
risk rating.

See also [TS-54](https://github.com/kieranpotts/standards/tree/latest/dev/src/054)
for the technical standard that underpins this process.

****
The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD,
SHOULD NOT, OPTIONAL, and MAY herein are to be interpreted as described
in [IETF RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).
****

## Workflow

> [!TIP]
> [Agent skills](./.agents/skills/) are available to help automate some steps in
> this workflow.

1.  Scope the workshop. Decide what is being threat modeled – which systems,
    subsystems, services, or data flows are in-scope. Decide which threat
    modeling frameworks apply. Choose STRIDE as a minimum, extend with LINDDUN
    for privacy-sensitive systems, and consider adding OWASP categories and
    top-10 lists, etc.

2.  Gather the inputs the workflow needs: architecture diagrams, data-flow
    diagrams, the previous workshop report (if updating), and the deployment
    topology.

3.  Hold a threat modeling workshop to review the current risks in the target
    system or subsystem.

4.  Write up the findings from the workshop. Branch off `main` using the
    convention `report/<slug>`, where `<slug>` is a short, hyphen-delimited
    description of the workshop's scope, eg. `report/payment-flow`. Write up the
    report based on the [template](./risks/TEMPLATE.md). Save the report at
    `risks/YYYY-MM-DD-<slug>/README.md`, with the header metadata filled in.
    Prepend a row to the [workshop index](./risks/INDEX.md).

5.  Update the [risk register](./risks/REGISTER.md). Promote each threat worth
    tracking into a new row of the register, with a fresh reference number.

6.  Commit your changes with the message `report: <description>`, where
    `<description>` is a short prose title, written full lowercase, eg.
    `report: payment flow threat model`. Open a pull request with the same
    title as the commit nessage. Gather feedback via normal pull request
    comments.

7.  Once review is settled, squash-merge the pull request, using the PR title as
    the merge-commit message. Delete the `report/*` branch.

Between workshops, keep the risk register up-to-date, for example to reflect
newly-implemented mitigation strategies.

1.  Branch off `main` using the convention `register/<slug>`, eg.
    `register/mfa-rollout` or `register/q3-review`.

2.  Update the affected rows of the [risk register](./risks/REGISTER.md) in
    place – mitigation status, severity, residual risk, and the review date.

3.  Commit with a `register: <description>` message and open a pull request.

4.  Merge it once the change it reflects is real – eg. once the mitigation has
    actually shipped to production.

## Rules

- All artifacts MUST be written in American English.

- Every session report MUST be dated, scoped, and cite the exact system context
  assessed.

- Every identified threat MUST be classified using a named framework and rated
  by likelihood and impact, using a consistent scoring scheme.

- The register MUST be treated as the single source of truth for current risk
  status. Each tracked risk lives in exactly one register row, updated in
  place. Status MUST NOT be scattered across session reports.

- Every register row MUST record a mitigation strategy (or an explicit, reasoned
  decision to accept the risk) and its residual risk.

- Session reports MUST be treated as immutable once merged. To reassess the
  system, hold a new session – a merged report MUST NOT be edited.

- The GitHub issue tracker MUST be used only for maintenance work on this
  repository itself.

## Tools

### Pre-commit hooks

It is RECOMMENDED to install the [pre-commit](https://pre-commit.com) framework
to enable local validation hooks before committing. You need only to run the
following command once to install pre-commit system-wide:

```bash
pipx install pre-commit
```

Then install the pre-commit hooks in every local repository where you want
pre-commit checks to be run:

```bash
pre-commit install
```

This installs all hook types declared in `.pre-commit-config.yaml`
(`pre-commit`, `commit-msg`).

Edit `./.pre-commit-config.yaml` to configure the pre-commit validation checks
you want for your project. See the [pre-commit](https://pre-commit.com) docs for
details.

## Contributor license agreement

<!-- Delete this for closed source projects. -->

By opening a pull request to this repository, you accept and agree to the
following terms and conditions:

- You agree that your contribution may be distributed under the terms of the
  [CC0 1.0 Universal license](./LICENSE.txt), effectively releasing it to
  the public domain.

- You certify that your contribution is either created in whole by you and
  you have the right to distribute it under the designated license, or is
  based on a previous work with a compatible license that permits distribution
  and modification under the designated license.

- You understand and agree that your contribution is public and that a record
  of it, including all personal information you submit with it, is maintained
  indefinitely and may be redistributed consistent with the designated license.
