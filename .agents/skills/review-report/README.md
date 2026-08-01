# Review report

Takes a threat modeling workshop report's pull request out of draft once it
has enough substance for review.

## What it does

- Identifies the session from the current branch (or asks).

- Checks the header metadata is filled in, the threat assessment has at
  least one rated entry, and every raised risk has a matching register row.

- Marks the pull request ready for review (`gh pr ready`).

## How to invoke

Run from a `report/*` branch:

> Review report

> This report is ready for review.

Or specify the target PR:

> Review #42

## Notes

This is a light check, not a completeness gate — mitigation strategies and
follow-ups MAY still evolve based on review feedback. Once review is
settled, use [`/complete-report`](../complete-report/README.md) to land it.
