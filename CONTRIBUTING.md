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

> [!NOTE]
> The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD,
> SHOULD NOT, OPTIONAL, and MAY herein are to be interpreted as described
> in [IETF RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Artifacts

This repository holds two types of artifact, each with its own workflow:

- **Workshop reports** are point-in-time snapshots of the outcomes of threat
  modeling workshops, immutable once merged to the `main` trunk.

- **The register** is a living database of currently-tracked risks, edited
  in-place as risks evolve.

Threat modeling workshops, when they uncover new threats, will seed new entries
in the risk register. From then on, the implementation of mitigation strategies
and residual risks are tracked indefinitely via the register.

## Workflow

### Step 1: Scope the workshop

Decide what is being threat modeled – which system, subsystems, services, or
data flows are in scope.

Decide which threat modeling frameworks apply. Choose STRIDE as a minimum,
extend with LINDDUN for privacy-sensitive systems, and consider adding OWASP
categories and top-10 lists, etc.

Gather the inputs the workflow needs: architecture diagrams, data-flow diagrams,
the previous workshop report (if updating), and the deployment topology.

### Step 2: Open a pull request

1.  Branch off `main` using the convention `report/<slug>`, where `<slug>`
    is a short, hyphen-delimited description, eg. `report/payment-flow`.

2.  Hold the workshop against the scoped system.

3.  Save the report at `risks/YYYY-MM-DD-<slug>/README.md`, copied from
    [`TEMPLATE.md`](./risks/TEMPLATE.md), with the metadata header filled in:
    facilitator, participants, date, scope.

3.  Promote each threat worth tracking into a new row of the
    [risk register](./risks/REGISTER.md), with a fresh reference number,
    its rating, mitigation strategy, and residual risk.

4.  Prepend a row to the [index](./risks/INDEX.md).

5.  Commit your changes and open a pull request titled `report: <description>`,
    where `<description>` is a short prose title, written full lowercase,
    eg. `report: payment flow threat model`.

### Step 3: Review

Gather feedback via normal pull request comments. No discussion thread is
required. A workflow report is a set of findings to review, not a decision to
debate.

### Step 4: Merge

Once review is settled, squash-merge the pull request, with a message of
the form `report: <description>`. Delete the branch. The report, its index
row, and the new register rows all land together.

## The register workflow

Between workshops, the register is kept current as risks evolve – a mitigation
lands, a risk is reassessed, a scheduled review is performed, or a security
finding from an [audit](https://github.com/kieranpotts/audits) is promoted into
a tracked risk.

1.  Branch off `main` using the convention `register/<slug>`, eg.
    `register/mfa-rollout` or `register/q3-review`.

2.  Update the affected rows of [`risks/REGISTER.md`](./risks/REGISTER.md) in
    place – mitigation status, severity, residual risk, and the `Date reviewed`.
    Do NOT touch any merged session report.

3.  Commit with a `register: <description>` message and open a pull request.
    Merge it once the change it reflects is real – eg. once the mitigation has
    actually shipped to production, so the register never overstates the
    system's security posture.

## Rules

- All documents MUST be written in American English.

- Every session report MUST be dated, scoped, and cite the exact system context
  assessed.

- Every identified threat MUST be classified using a named framework and rated
  by likelihood and impact, using a consistent scoring scheme.

- The register is the single source of truth for current risk status. Each
  tracked risk lives in exactly one register row, updated in place. Do not
  scatter status across session reports.

- Every register row MUST record a mitigation strategy (or an explicit, reasoned
  decision to accept the risk) and its residual risk.

- Session reports are immutable once merged. To reassess the system, hold a new
  session – never edit a merged report.

- This repository is discovery and record-keeping only. It MUST NOT change any
  code, and threat identification MUST NOT include actively exploiting the
  system.

- The GitHub issue tracker is used only for maintenance work on this repository
  itself (the `MAINTENANCE` template). Open-ended brainstorming happens in
  [discussions](https://github.com/kieranpotts/risks/discussions).

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
