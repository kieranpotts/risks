# Update register

Updates the living [risk register](../../../risks/REGISTER.md) in place —
mitigations shipped, scheduled reviews, or risks retired — without a workshop.

This is the one skill in this repository that operates on the register rather
than on a workshop report. The two artifacts have opposite lifecycles: a
workshop report is drafted, reviewed, merged, and then frozen, while the
register is living documentation whose rows are revised in place forever
afterwards. A workshop seeds its register rows on the report's own branch; from
the moment that report lands in `latest/main`, every further change to those
rows goes through this skill instead, on a `register/<slug>` branch of its own.

## Interactivity

Interactive. The agent may prompt for which risk changed and how, and it will
ask before merging whether the change is genuinely real yet — whether the
mitigation has actually reached production. It stops rather than guessing when
it cannot confirm that.

## How to invoke

> Update register

> The MFA mitigation for TA1 is shipped.

> Mark risk TA1 as mitigated

> Review the register

> Retire this risk

## Recommended models

A mid-tier model is sufficient. Editing the rows is mechanical, but judging
whether a mitigation has genuinely shipped, and whether a change is really
status-tracking rather than a new threat, needs a little more.

## Related skills

- [**draft-report**](../draft-report/) \
  Use that instead when the change is fresh threat identification. New threats
  are raised by a workshop, and its report seeds the register rows.

- [**review-report**](../review-report/) \
  Checks that the register rows a workshop claims to have raised really exist
  before the report goes out for review.

- [**complete-report**](../complete-report/) \
  Lands a workshop's initial register rows in `latest/main`. Every later
  revision of those rows belongs to this skill.
