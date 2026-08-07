# Complete report

Lands a reviewed threat modeling workshop report in the `main` trunk.

Confirms the pull request is out of draft and its review is settled, then
squash-merges it and deletes the `report/<slug>` branch. The report, its row in
the [workshop index](../../../risks/INDEX.md), and the rows the assessment
raised in the [risk register](../../../risks/REGISTER.md) all land together in
one commit. Once merged, the report is immutable.

## Interactivity

Interactive. The agent asks which pull request to merge where the target
cannot be inferred from the checked-out branch, and it always requires an
explicit instruction before merging. It is therefore not suitable for
away-from-keyboard use.

## How to invoke

Run from a `report/*` branch:

> Complete report

> Land this report

> Merge the report

> The threat modeling workshop report is done.

## Recommended models

A mid-tier model is sufficient. The merge itself is mechanical, but judging
whether the review feedback has been settled needs a little more.

## Related skills

- [**draft-report**](../draft-report/) \
  Runs first, scaffolding the report and opening the pull request this skill
  eventually merges.

- [**review-report**](../review-report/) \
  Runs immediately before, taking the pull request out of draft. This skill
  refuses to merge until that has happened.

- [**update-register**](../update-register/) \
  Takes over once the report has landed. From then on, every change to the
  register rows this workshop raised goes through that skill, because merged
  reports are never edited.
