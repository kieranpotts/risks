# Draft report

Scaffolds a blank report to capture the findings from a new threat modeling
workshop.

Cuts a `latest/report/<slug>` branch from `latest/main`, prepares a fresh report
from [the template](../../../risks/TEMPLATE.md), prepends a row to the
[workshop index](../../../risks/INDEX.md), and opens a pull request in a draft
state. It does not run the workshop: the assessment and the risk register rows
it raises are authored afterwards, onto the same branch.

## Interactivity

Interactive. The agent may prompt for the scope of the workshop — the system,
subsystems, services, or data flows to be threat modeled — and for the threat
modeling frameworks to apply, where neither can be determined from context.
Everything else is inferred, so the skill runs to completion once the scope is
settled.

## How to invoke

> Draft report

> Draft a new report

> Prepare a threat modeling workshop report

> Threat model the payment flow.

## Recommended models

A fast, cheap model is sufficient. The skill only scaffolds the report and its
pull request; the judgment-heavy work of the workshop itself comes later.

## Related skills

- [**review-report**](../review-report/) \
  Runs next, once the workshop findings have been written into the scaffolded
  report, taking its pull request out of draft.

- [**complete-report**](../complete-report/) \
  Runs last, squash-merging the reviewed report into `latest/main`.

- [**update-register**](../update-register/) \
  Maintains the risk register between workshops. The register rows a workshop
  raises are added on the report's own branch; every later change to those
  rows goes through that skill instead.

## References

- [TS-54: Threat Modeling](https://kieranpotts.com/standards/054)
  — the standard the report template follows.
