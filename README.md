# ⚠️ Risk Register

**A template for capturing the outcomes of threat modeling sessions**, and for
tracking security and privacy risks over time via version control.

This repository is the home of the risk register for [Project Name]. It records
the security and privacy risks identified for the system, together with their
ratings, mitigation strategies, and current status.

There are two parts to the register:

- A **living risk register.** This is a single table of the risks currently
  being tracked, with their probability, impact, severity, mitigation steps,
  status, and residual risk. This is a mutable document. As mitigations are
  applied, risks are reassessed, and the risk register is updated accordingly.

- An **immutable, append-only log of threat modeling sessions.** For each
  threat modeling workshop, a report captures the system context assessed, the
  threat modeling frameworks applied (eg. STRIDE), and the risks identified in
  the workshop.

The two parts answer different questions. A threat modeling report says
_what we found_ in an historical security review workshop. The risk register
captures _the  current security posture of the system_ and tracks the
implementation of new threat mitigation strategies.

The artifacts in this repository complement the architecture audit reports
that are [captured separately](https://github.com/kieranpotts/audits). Both
architecture audits and threat modeling workshops assess the as-built system.
The difference between the two is in their focus. Architecture audits assess a
system's structural design with respect to its maintainability, extensibility,
auditability, and other product development concerns. Threat modeling sessions
evaluate a system's design against known security risks and question whether
suitable mitigation strategies are in place.

> [!NOTE]
> This repository is the reference implementation of
> **[TS-54: Threat Modeling](https://github.com/kieranpotts/standards/tree/dev/src/054)**.
> This technical standard defines _how_ to run threat modeling sessions and _what_
> a risk register should contain. Please refer to the technical standard for the
> underlying rationale for these choices. This repository is the ready-to-use
> template that helps to put TS-54 into practice.

## Ecosystem

This repository is one of six that form a coherent, version-controlled
documentation ecosystem modeling the software development lifecycle. Each is the
reference implementation of an opinionated workflow, and answers a different
question about a software system:

- [**📋 Software Requirements Specification (SRS)**](https://github.com/kieranpotts/specs):
  Records _what_ the system does, in business terms.

- [**💬 Requests for Comments (RFC)**](https://github.com/kieranpotts/rfc):
  Records _how_ significant technical decisions were made, and _why_.

- [**📐 Design Docs**](https://github.com/kieranpotts/design):
  Describe _what the system looks like_, its as-is architecture.

- [**🗺️ Delivery Plans**](https://github.com/kieranpotts/plans):
  Capture _when, and in what order_, the work gets done.

- [**🔍 Architecture Audits**](https://github.com/kieranpotts/audits):
  Evaluate the as-built system on its own terms.

- **⚠️ Risk Register**:
  Records the security and privacy risks the system carries (this repository).

The [**skills**](https://github.com/kieranpotts/skills) collection provides an
agentic workflow that operates across all of these repositories.

This separation into dedicated repositories is intended for application software
that spans multiple code repositories, and potentially multiple teams, where the
requirements, decisions, designs, plans, audits, and risks are shared concerns
that sit above any single codebase. For a standalone code repository – a small
utility library, say – it may be better to fold these artifacts and skills
directly into that repository, rather than maintain them separately.

## Contents

- [**Register**](./risks/REGISTER.md): The living register of tracked risks.

- [**Sessions**](./risks/): The permanent archive of threat modeling session
  reports, one directory per workshop.

  - The [`INDEX`](./risks/INDEX.md) lists every report merged into `main`,
    newest first.

  - The [`TEMPLATE`](./risks/TEMPLATE.md) is the starting point for a new
    threat modeling workshop report.

- [**Contributing**](./CONTRIBUTING.md): Step-by-step instructions for running a
  threat modeling session and maintaining the register.

- [**Agents**](./AGENTS.md) and [**Skills**](./.agents/skills/): Instructions
  for agentic tools to run sessions and maintain the register with a high degree
  of autonomy.

- [**Documentation**](./docs/): General guidance on threat modeling, risk
  rating, and keeping the register honest.

-----

Copyright © 2020-present Kieran Potts, [CC0 license](./LICENSE.txt)
