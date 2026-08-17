Briefly describe what this PR does — a write-up of a threat modeling workshop,
or an update to the living risk register.

> [!IMPORTANT]
> Use the PR comments for review feedback. The GitHub issue tracker is not part
> of this workflow — it is reserved for maintenance work on this repository
> itself.

----

## Checklist

Use whichever section applies. Delete the other.

### A threat modeling workshop report

On opening this PR (open it as a draft):

- [ ] The branch is named `latest/report/<slug>`.
- [ ] The report is saved at `risks/YYYY-MM-DD-<slug>/README.md`, with the
      metadata header filled in.
- [ ] Supporting artifacts live under `risks/YYYY-MM-DD-<slug>/` and are
      referenced from the report's `README.md`.
- [ ] A row is prepended to `risks/INDEX.md`.
- [ ] The report is written in American English.
- [ ] The report is dated, scoped, and cites the exact system context assessed.
- [ ] Every identified threat is classified using a named framework (STRIDE as
      a minimum) and rated by likelihood and impact, using a consistent
      scoring scheme.
- [ ] Every threat worth tracking is promoted into a new row of
      `risks/REGISTER.md`, with a fresh reference number.
- [ ] No code was changed, and no threat was verified by actively exploiting
      the system. This repository is discovery and record-keeping only.

Mark this PR ready for review when:

- [ ] No generic template text or unfilled placeholders remain.
- [ ] The PR title follows `create: <description>` — a short prose title,
      written full lowercase, eg. `create: payment flow threat model`.

Merge this PR when:

- [ ] Review feedback is resolved.
- [ ] The PR is squash-merged, using the PR title as the merge-commit message.

After merging, complete these tasks:

- Delete the `latest/report/*` branch.

> [!IMPORTANT]
> Workshop reports on `latest/main` are immutable. Do NOT edit a merged report
> — hold a new workshop instead.

### An update to the risk register

- [ ] The branch is named `latest/register/<slug>`, eg. `latest/register/mfa-rollout`.
- [ ] The affected rows of `risks/REGISTER.md` are updated in place —
      mitigation status, severity, residual risk, and review date.
- [ ] Each affected row records a mitigation strategy (or an explicit, reasoned
      decision to accept the risk) and its residual risk.
- [ ] Status is recorded only in the register. It is not scattered across
      workshop reports.
- [ ] Mitigation work itself is tracked in the relevant code repository's issue
      tracker, and the register links out to it.
- [ ] The PR title follows `update: <description>`.

Merge this PR when:

- [ ] The change it reflects is real — eg. the mitigation has actually shipped
      to production.
