# Overview

A risk register is a living account of the security and privacy risks a system
carries, and how they are being mitigated over time. It is fed primarily by
threat modeling sessions – structured workshops that analyze the system's design
to identify threats before they become incidents.

This repository implements [TS-54: Threat
Modeling](https://github.com/kieranpotts/standards/tree/latest/dev/src/054).

## Two artifacts, two natures

The repository holds two kinds of artifact, and the distinction between them is
the organizing principle of the whole thing:

- **Session reports are point-in-time and immutable.** A threat modeling session
  happens on a date, against a defined scope, and its report is true as of that
  moment. Like an [audit](https://github.com/kieranpotts/audits), it is frozen
  once merged. To reassess a system, you hold a new session – you do not edit an
  old report. The [`INDEX`](../risks/INDEX.md) accumulates a chronological trail
  of every session held.

- **The register is living.** A risk is not a moment; it has a lifetime. It is
  identified, rated, mitigated, reassessed, and eventually retired. The
  [`REGISTER`](../risks/REGISTER.md) tracks that lifetime, with each risk in one
  row, updated in place. Like [design docs](https://github.com/kieranpotts/design)
  and the [specification](https://github.com/kieranpotts/specs), it MUST reflect
  the current state of the system, and is merged alongside the change it
  describes.

Keeping these separate avoids two failure modes: a register that is really just
a pile of stale workshop notes, and a set of "living" reports that quietly drift
from the day they were written. The session says _what we found, when_. The
register says _where each risk stands now_.

## Where it sits in the lifecycle

Threat modeling works best when it is iterative and begins during the design
phase, not bolted on before a release. Because security and privacy are
cross-cutting architectural concerns, they are expensive to retrofit. A session
scheduled at the break point of each development cycle catches vulnerabilities
while they are still cheap to fix.

A session reasons about the system's _design_ – its components, data flows,
trust boundaries, and sensitive assets – which is why it draws on the
[design docs](https://github.com/kieranpotts/design) and the
[specification](https://github.com/kieranpotts/specs) as inputs, and benefits
from a multi-disciplinary group of participants who can see the system from
architecture, product, and operations perspectives at once.

## Relationship to audits

The [audits](https://github.com/kieranpotts/audits) repository and this one both
produce security findings, but they are complementary:

- An **audit** is an unbiased, point-in-time read of the _code as it stands_,
  covering both architecture and security, deliberately blind to the intended
  design. It stops at a report.

- A **session** reasons about the _system's design and data flows_ to anticipate
  threats, and every risk worth tracking is carried forward in the living
  register through to mitigation.

The two meet at one seam: a security finding from an audit MAY be promoted into
the register when it is a risk worth tracking over time. Use
[`/update-register`](../.agents/skills/update-register/) for that.
