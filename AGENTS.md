# [Project Name] – Risk Register

The capitalized words REQUIRED, MUST, MUST NOT, RECOMMENDED, SHOULD,
SHOULD NOT, OPTIONAL, and MAY are to be interpreted as described in
[IETF RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

## Project overview

This repository holds the risk register for [Project Name] – the security and
privacy risks the system carries, and how they are being mitigated over time.
It is the reference implementation of [TS-54: Threat
Modeling](https://github.com/kieranpotts/standards/tree/dev/src/054). It is
documentation, not code. There's nothing to build, lint, or run.

The repository has two parts, with two different natures:

- **`risks/REGISTER.md` is living documentation.** It is the single table of
  currently-tracked risks. Its rows are updated in place – as mitigations
  progress, risks are reassessed, or reviews are performed – and merged into
  `main` alongside the change they describe. Like the sibling
  [`design`](https://github.com/kieranpotts/design) and
  [`specs`](https://github.com/kieranpotts/specs) repositories, it MUST reflect
  the current state of the system, never drifting from reality.

- **Session reports are immutable.** Each threat modeling session is captured in
  a dated report under `risks/YYYY-MM-DD-<slug>/`. A report is a point-in-time
  snapshot, true at the moment the session was held, and immutable once merged –
  like an [`audit`](https://github.com/kieranpotts/audits). It is superseded not
  by editing it, but by running a new session.
  [`risks/INDEX.md`](./risks/INDEX.md) is the append-only catalog of every
  session held.

A **threat modeling session** systematically analyzes representations of the
system – its components, data flows, trust boundaries, and assets – to identify
security and privacy threats, using frameworks such as STRIDE. Each identified
threat is rated by likelihood and impact, given a mitigation strategy, and – if
it is a risk worth tracking – promoted into the register. See TS-54 for the
method.

## Project structure

- **`risks/`:**
  The register and the session reports.

  - **`risks/REGISTER.md`** is the living register of tracked risks – the single
    source of truth for where each risk stands right now.

  - **`risks/INDEX.md`** is the append-only catalog of every session merged into
    `main`, newest first.

  - **`risks/TEMPLATE.md`** is the starting point for a new session report.

  - **`risks/YYYY-MM-DD-<slug>/`** is one session report, dated by when the
    session was held.

- **`docs/`:**
  General guidance for humans on threat modeling, risk rating, and maintaining
  the register.

## How work is introduced

There are two workflows, matching the two artifacts.

### Threat modeling sessions

A session has no lifecycle state machine – it is scoped, held, and merged in one
pass, like an audit:

1. A session is opened as a pull request on a `session/<slug>` branch,
   scaffolded by the [scaffold report](./.agents/skills/scaffold report/) skill.
   The report is written from [`risks/TEMPLATE.md`](./risks/TEMPLATE.md), and
   any risks worth tracking are added as rows to
   [`risks/REGISTER.md`](./risks/REGISTER.md) in the same pull request.

2. The report is reviewed via normal pull request comments. No discussion thread
   is required – a session report is findings to review, not a decision to
   debate.

3. Once review is settled, [finalize report](./.agents/skills/finalize-report/)
   squash-merges the pull request, along with the `INDEX.md` row and the new
   register rows (all added in the same PR).

### Register updates

Between sessions, the register is kept current as risks evolve – a mitigation
lands, a risk is reassessed, a scheduled review is performed. These updates are
made via the [`update-register`](./.agents/skills/update-register/) skill, on a
`register/<slug>` branch, and merged when the change they reflect is real.
Updating the register MUST NOT touch any merged session report.

## Rules

- Write in American English.

- **The register is the single source of truth for current risk status.** Every
  tracked risk lives in exactly one row of [`risks/REGISTER.md`](./risks/REGISTER.md).
  Its probability, impact, severity, mitigation status, and residual risk are
  updated in place. Do not scatter status across session reports.

- **Session reports are immutable once merged.** To reassess the system, hold a
  new session – never edit a merged report. Correcting a report's _factual
  record_ (eg. a wrong commit hash) is the only exception, and MUST be called
  out in the commit message.

- Every session report MUST be dated and scoped, and cite the exact system
  context assessed (the components, data flows, and versions/`repo@commit` where
  applicable), so it is a reproducible point-in-time snapshot.

- Every identified threat MUST be classified using a named framework (STRIDE is
  the RECOMMENDED minimum; LINDDUN for privacy; OWASP categories; etc.), and
  rated by likelihood and impact to yield a severity, using a consistent
  scoring scheme.

- Every register row MUST record a mitigation strategy (or an explicit, reasoned
  decision to accept the risk with no mitigation) and its residual risk.

- This repository is discovery and record-keeping only. It MUST NOT change any
  code, and threat identification MUST NOT include actively exploiting the
  system. Mitigation _work_ is tracked in the relevant code repository's own
  issue tracker; the register links out to it.

- The GitHub issue tracker is used only for maintenance work on this
  repository itself (via the `MAINTENANCE` template). Open-ended
  brainstorming happens in
  [discussions](https://github.com/kieranpotts/risks/discussions).

## Skills

The [`.agents/skills/`](./.agents/skills/) directory is reserved for on-demand
skills that help run sessions and maintain the register. See the
[README](./.agents/skills/README.md) for details.
