# [Project Name] – Risk Register

The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD,
SHOULD NOT, OPTIONAL, and MAY are to be interpreted as described in
[IETF RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Project overview

See the [README](./README.md) for an overview of this project, and how it fits
alongside the sibling SRS, RFC, design, plans, and audits repositories.

This repository is documentation, not code. There is nothing to build, lint,
or run.

## Project structure

- `risks/`. The risk register and workshop reports.

  - `risks/REGISTER.md` is the living register of tracked risks. It is the
    single source of truth for where each identified risk stands right now.

  - `risks/INDEX.md` is the append-only catalog of every threat modeling
    workshop held, and its report merged into `latest/main`, newest first.

  - `risks/TEMPLATE.md` is the starting point for a new workshop report.

  - `risks/YYYY-MM-DD-<slug>/` is one workshop report, dated by when the
    workshop was held.

- `docs/`. General guidance for humans on threat modeling, risk rating, and
  maintaining the register.

## Workflow

There is no lifecycle state machine here. The risk register is simply a living
document updated in-place between workshops.

See [CONTRIBUTING.md > Workflow](./CONTRIBUTING.md#workflow) for the
step-by-step process for running a threat modeling session and for keeping the
register current between workshops.

## Rules

Agents MUST follow the rules in [CONTRIBUTING.md > Rules](./CONTRIBUTING.md#rules).
Re-read the rules before writing a workshop report or updating the register,
rather than relying on your memory of a prior state of the rules.

## Skills

The `.agents/skills/` directory provides on-demand skills for running
workshops and maintaining the register. See the [README](./.agents/skills/README.md)
for descriptions of the available skills and their triggers.

It is RECOMMENDED to use these skills to drive the workflow.

## References

The following technical standards (TS) govern this project. Fetch and ingest
the relevant standards as-and-when required for the task at hand.

- [**TS-54: Threat Modeling**](https://kieranpotts.com/standards/054) \
  Use when designing, reviewing, or iterating on a system's security posture.
  Covers threat modeling workshops and maintenance of a project's risk register.

- [**TS-25: Technical Documentation**](https://kieranpotts.com/standards/025) \
  Use when deciding what documentation a project needs, where it should live,
  who it's for, or whether it's still trustworthy.

- [**TS-26: Technical Writing Style Guide**](https://kieranpotts.com/standards/026) \
  Use when writing or editing the prose of a technical document. Covers
  tone-of-voice, headings, terminology, lists, and citations.

- [**TS-9: Version Control**](https://kieranpotts.com/standards/009) \
  Use when working with Git. Covers commits, branching, merging, integration
  strategies, cutting releases, and configuring Git/PR/CI tooling.
