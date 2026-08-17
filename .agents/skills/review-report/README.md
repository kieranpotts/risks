# Review report

Checks that a threat modeling workshop report has enough substance for review,
and takes its pull request out of draft.

The check is deliberately shallow. It confirms the report's metadata header is
filled in, that the threat assessment has at least one rated entry, and that
every risk the report says it raised has a matching row in the
[risk register](../../../risks/REGISTER.md). Polish, mitigation detail, and
follow-ups are left for reviewers to comment on.

## Interactivity

Interactive. Where the target cannot be inferred from the checked-out
`latest/report/<slug>` branch, the agent lists the open draft pull requests and
asks which to mark ready. It also stops and reports back, rather than proceeding,
when the report is missing substance.

## How to invoke

Run from a `latest/report/*` branch:

> Review report

> Review this report

> This report is ready for review.

> Take the report out of draft

> Mark the report ready for review

Or name the target pull request:

> Review #42

## Recommended models

A fast, cheap model is sufficient. The completeness check is shallow and
mechanical, and the report itself is left untouched.

## Related skills

- [**draft-report**](../draft-report/) \
  Runs first, scaffolding the report and opening the draft pull request that
  this skill later marks ready.

- [**complete-report**](../complete-report/) \
  Runs next, once review has settled, squash-merging the report into `latest/main`.

- [**update-register**](../update-register/) \
  Maintains the risk register between workshops. This skill checks the
  register rows a workshop raised, but never edits them.
