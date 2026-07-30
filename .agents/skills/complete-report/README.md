# Complete report

Lands a threat modeling session once review is settled.

## What it does

- Confirms review feedback on the pull request has been addressed.

- Squash-merges the pull request with a `report: <description>` message.

- Deletes the branch. The report, its index row, and the new register rows land
  together.

## How to invoke

> Complete report

## Examples

- `/complete-report`: The agent infers the pull request from the checked-out
  `report/<slug>` branch, confirms it's ready, and merges it.

- `/complete-report <description>`: If not on the report branch, name the
  session so the agent can find its pull request.

Use [`/scaffold-report`](../scaffold-report/README.md) first to scaffold the
session, and [`/update-register`](../update-register/README.md) afterwards to
keep the raised risks current.
