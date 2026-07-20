# ⚠️ Risk Register

**A template for capturing the outcomes of threat modeling workshops**, and for
tracking security and privacy risks over time via version control.

This repository is the home of the risk register for [Project Name]. It records
the security and privacy risks identified for the system, together with their
ratings, mitigation strategies, and current status.

There are two parts to the register:

- A **living risk register.** This is a table of all the risks in the system
  currently being tracked. For each risk, the register records its probability,
  impact, and overall severity rating, plus the mitigation steps being put in
  place and the implementation status of those mitigations. The risk register
  is living documentation. As risks are discovered, mitigations applied, and
  risks subsequently reassessed, the risk register is updated accordingly.

- An **immutable, append-only log of reports from threat modeling workshops.**
  Threat modeling workshops drive the register. At the end of each workshop,
  a report is produced that captures the system context assessed (eg. the
  particular microservices that were reviewed), the threat modeling frameworks
  applied (eg. STRIDE), and the risks identified in the workshop.

The two parts answer different questions. A report captures what we found in a
past security review workshop. The register captures the current security
posture of the system and tracks the implementation of mitigation strategies
for all threats identified over the lifetime of the project.

The artifacts in this repository complement the architecture audit reports
that are [captured separately](https://github.com/kieranpotts/audits). Both
architecture audits and threat modeling workshops assess the as-built system.
The difference between the two is in their focus. Architecture audits assess a
system's structural design with respect to its maintainability, extensibility,
auditability, and other product development concerns. Threat modeling workshops
evaluate a system's design against known security risks and question whether
suitable mitigation strategies are in place.

> [!NOTE]
> See **[TS-54: Threat Modeling](https://github.com/kieranpotts/standards/tree/latest/dev/src/054)**
> for more guidance on running threat modeling workshops and managing risk
> registers.

## Ecosystem

This repository is one of six that form a coherent, version-controlled
documentation ecosystem. Each answers a different question about a software
system.

- [**📋 Software Requirements Specification (SRS)**](https://github.com/kieranpotts/specs) \
  Captures what the system does, in business terms.

- [**💬 Requests for Comments (RFC)**](https://github.com/kieranpotts/rfc) \
  Records how significant technical decisions were made, and why.

- [**📐 Design Docs**](https://github.com/kieranpotts/design) \
  Documents what the system looks like in production.

- [**🔍 Architecture Audits**](https://github.com/kieranpotts/audits) \
  Logs historical evaluations of the as-built system's structural integrity.

- [**🗺️ Delivery Plans**](https://github.com/kieranpotts/plans) \
  Tracks when, and in what order, the work gets done.

- [**⚠️ Risk Register**](https://github.com/kieranpotts/risks) (this repository) \
  Records the inherent security and privacy risks the system carries.

In addition, the [**✨ Agent Skills**](https://github.com/kieranpotts/skills)
collection offers composable agentic workflows that operate across all six
repositories.

This separation into dedicated repositories is intended for application software
that spans multiple code repositories, and potentially multiple teams, where the
requirements, decisions, designs, plans, audits, and risks are shared concerns
that sit above any single codebase.

For a standalone code repository – a small utility library, say – it may be
better to fold all documentation into the same repository.

## Contents

- [**Register**](./risks/REGISTER.md) \
  The living register of tracked risks.

- [**Reports**](./risks/) \
  The permanent archive of reports from threat modeling workshops, one directory
  per workshop.

  - The [`INDEX`](./risks/INDEX.md) lists every report merged into `main`,
    newest first.

  - The [`TEMPLATE`](./risks/TEMPLATE.md) is the starting point for a new
    threat modeling workshop report.

- [**Contributing**](./CONTRIBUTING.md) \
  Step-by-step instructions for running a threat modeling workshop and
  maintaining the register.

- [**Agents**](./AGENTS.md) and [**Skills**](./.agents/skills/) \
  Instructions for agents to run workshops and to maintain the register with
  a high degree of autonomy.

- [**Documentation**](./docs/) \
  General guidance on threat modeling, risk rating, and keeping the
  register honest.

-----

Copyright © 2020-present Kieran Potts, [CC0 license](./LICENSE.txt)
