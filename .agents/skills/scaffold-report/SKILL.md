---
name: scaffold-report
description: >-
  Scaffold a report for a threat modeling workshop — run a STRIDE-based (and,
  where relevant, LINDDUN / OWASP) threat model against a scoped system, write
  its report, seed the risk register, and open a pull request. Use this skill
  when the user wants to threat model a system, or says "run a threat
  modeling session", "threat model this", or "open a session PR". Do not use
  this skill to merge a session — use finalize-report for that. Do not use it
  to update an existing risk — use update-register.
license: MIT
metadata:
  interactive: yes
---

# Scaffold report

Scaffolds a threat modeling session: runs a structured threat model against a scoped system, and opens a pull request with its report and the new risk register rows it seeds. Like an audit, a session has no lifecycle state machine — this skill takes it from nothing to an open, reviewable pull request in one pass. It implements the session workflow from [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054).

**Input:** Scope — REQUIRED. The system, subsystems, services, or data flows
being threat modeled, and the `owner/repo@commit` or version where
applicable. Frameworks — OPTIONAL, defaults to STRIDE. Prior session report
and architecture/data-flow diagrams — OPTIONAL, if this revisits a system.

**Output:** A `session/<slug>` branch, with `risks/YYYY-MM-DD-<slug>/README.md`
written, new rows seeded into `risks/REGISTER.md`, a new row in
`risks/INDEX.md`, committed to a pull request opened against `main` (ready
for review, not draft).

## Before scaffolding

-   **Determine the scope.**

    Ask the user what is being threat modeled if not already stated — the system, subsystems, services, or data flows in scope, and the `owner/repo@commit` or version where applicable. Scope is the only REQUIRED input.

-   **Determine the frameworks.**

    STRIDE is the default and minimum. Add LINDDUN if the system handles personal data, and OWASP Top 10 or other checklists for web-facing systems. Confirm with the user if unsure.

-   **Gather the inputs.**

    Collect the architecture and data-flow diagrams, deployment topology, and the previous session report (if this revisits a system). A session reasons about the system's _design_, not just its code.

## Instructions

1.  **Determine the slug and description.**

    Establish a short, hyphen-delimited slug naming the scope, eg. `payment-flow` or `auth-service`. Decide a short prose description from the user's request.

2.  **Create the branch.**

    ```sh
    git checkout main
    git pull
    git checkout -b session/<slug>
    ```

3.  **Run the session.**

    Work through the [TS-54](https://github.com/kieranpotts/standards/tree/dev/src/054) method: decompose the system (components, data flows, sensitive assets, entry points, trust boundaries), then systematically assess each against the chosen frameworks, and rate each threat by likelihood and impact. Write the report to `risks/YYYY-MM-DD-<slug>/README.md`, copied from [`risks/TEMPLATE.md`](../../../risks/TEMPLATE.md), with the metadata header filled in.

4.  **Seed the register.**

    Promote each threat worth tracking into a new row of [`risks/REGISTER.md`](../../../risks/REGISTER.md), with a fresh unique reference number, its rating, mitigation strategy, and residual risk. List those references in the report's "Risks raised" section.

5.  **Prepend the index row.**

    Add a row to [`risks/INDEX.md`](../../../risks/INDEX.md), at the top (newest first): Date, Scope, Frameworks, and the count of risks raised.

6.  **Commit and open a pull request.**

    ```sh
    git add risks/
    git commit -m "session: <short lowercase description>"
    git push -u origin session/<slug>
    gh pr create --title "session: <short lowercase description>" --fill
    ```

    Open it ready for review, not as a draft. No discussion thread is required: a session report is findings to review via normal pull request comments, not a decision to debate.

## Rules

-   **One session per branch and pull request.**

    Do not bundle multiple scopes into one PR — scaffold a separate session for each.

-   **Branch from `main`, not from any other branch.**

    Sessions are always cut from `main`. If local `main` is behind the remote, pull first.

-   **Discovery only — never exploit.**

    Identify threats by reasoning about the design; do NOT actively attack, exploit, or modify the running system or its code. Mitigation _work_ is tracked in the code repository's own issue tracker, linked from the register.

-   **Classify and rate every threat.**

    Every threat MUST carry a framework classification (STRIDE, etc.) and a likelihood × impact rating. An unrated threat is not a finished finding.

-   **Do not restate existing register status.**

    If a risk is already tracked, do not silently duplicate it. Note the overlap in the report and, if its rating has changed, flag it for [`/update-register`](../update-register/SKILL.md) rather than editing the register's status here beyond adding genuinely new rows.

## Success criteria

- Branch `session/<slug>` exists and is checked out.

- `risks/YYYY-MM-DD-<slug>/README.md` exists, following [`TEMPLATE.md`](../../../risks/TEMPLATE.md), with every threat classified and rated.

- New rows for the risks worth tracking are added to `risks/REGISTER.md`.

- `risks/INDEX.md` has a new row for this session, at the top.

- A pull request titled `session: <short lowercase description>` is open, ready for review.

## References

- [`AGENTS.md`](../../../AGENTS.md): The full session and register workflows and conventions.

- [`risks/README.md`](../../../risks/README.md): The report and register structure.

- [TS-54: Threat Modeling](https://github.com/kieranpotts/standards/tree/dev/src/054): The standard this implements.
